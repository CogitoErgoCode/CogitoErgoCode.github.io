---
layout: post
title: "PDF To Speech 💬"
date: 2022-02-23 12:00:02 -0700
categories: pdf tts speech
published: true
---

## **The First Attempt**

⚠️ PyPDF library does not work with all PDFs!

So the past couple weeks I've read over 22 books thanks to a little trick I've been employing on youtube with audiobooks and a special chrome extension that allows me to speed up said audiobooks past 2x playback speed. 

I got to thinking, that it would be nice if I had the same capability with some of my PDFs, as most of my library is composed of hundreds of PDFs.

>  This works **well for some** PDF files, but **poorly for others**... &mdash; [extractText()](https://pythonhosted.org/PyPDF2/PageObject.html#PyPDF2.pdf.PageObject.extractText)

I'm a little annoyed at my first attempt, mostly because I programmed it only to find out that the underlying library only extracts texts well, from *"some"* PDFs. Anyhow, I'll share what I have, it does work for *"some"* PDFs afterall.

# The Main Program

|:-:|:-:|
|[PyPDF Documentation](https://pythonhosted.org/PyPDF2/index.html)|[pyttsx3 Documentation](https://pyttsx3.readthedocs.io/en/latest/engine.html)|

The main function is setup in such a way that if you just press enter it'll skip the simple page choice, and call the `pdf.read()` method, which will display the PDFs outline, allowing you to select a chapter instead of a page.

```py
from PyPDF4    import PdfFileReader
from threading import Thread

import pyttsx3, os

def main():
    path   = 'Six Easy Pieces.pdf'
    pdf    = PDFExtract(path)
    choice = input("Enter page number: ")

    if choice.isdigit():
        page = int(choice)
        pdf.read_from(page)
    else:
        pdf.read()

if __name__ == "__main__":
    main()
```

# Sample output from `pdf.read()`.

The program will essentially read the PDF aloud, sentence by sentence, while printing what is read to stdout.

```
Six Easy Pieces.pdf

Author     : Richard P. Feynman, Robert B. Leighton, Matthew Sands
Creator    : None
Producer   : None
Subject    : Basic Books,2011
Title      : Six Easy Pieces: Essentials of Physics Explained by Its Most Brilliant Teacher
Page Count : 177

Generating Outline

   1 CONTENTS
   2 PUBLISHER’S NOTE
   3 INTRODUCTION
   4 SPECIAL PREFACE
   5 FEYNMAN’S PREFACE
   6 1 Atom in Motion
   7 2 Basic Physics
   8 3 The Relation of Physics to Other Sciences
   9 4 Conservation of Energy
  10 5 The Theory of Gravitation
  11 6 Quantum Behavior
  12 Index

Select: 7
1: BASIC PHYSICS Introduction In this chapter, we shall examine the most fundamental ideas that we have about physics—the nature of things as we see them at the present time.
```

# Convenience Functions

Whenever I program something that employs a wait routine on the current thread, I usually spin this function off in the main thread to catch the CTRL+C. The `handle_ctrl_c()` function handles ctrl+c by foregrounding input() and catching EOFError. If we thread the handling of ctrl+c we essentially background the program and listen for input.

```py
def handle_ctrl_c():
    try:
        input()
    except EOFError:
        print("ctrl+c")
        os._exit(1)
```

# The PDFExtract Class

This class will initialize pyttsx3 and set the word rate to 400 words per minute upon instantiation.

```py
class PDFExtract:
    def __init__(self, path, rate=200, multiplier=2):
        self.__path = path

        self.engine = pyttsx3.init()
        self.engine.setProperty('rate', rate*multiplier)

    @property
    def path(self):
        return self.__path
```

# The Read Method

This is really the main focus of our entire program. We will first present the PDFs metadata neatly, followed by the PDFs outline. The user will select a chapter, and the program will begin to read from that chapter. The program will continue to read until CTRL+C or EOF.

```py
def read(self):
    # Display Full Path
    print(f"{self.path}\n")

    with open(self.path, 'rb') as f:
        pdf    = PdfFileReader(f, strict=False)
        menu   = self.extract_info(pdf)
        keys   = menu.keys()

        choice = input("\nSelect: ")
        
        # if not choice.isdigit() and not len(choice) > len(keys):
        if not (choice.isdigit() or len(choice) > len(keys)):
            raise TypeError('Integers Only!')

        choice = int(choice)
        
        if not choice in keys:
            raise ValueError(f"Out of Range: {min(keys)}-{max(keys)}")

        # Read From Selection
        start, title, pageObj = menu[choice]
        self.engine.say(f"{title}, page: {start}.") 
        self.engine.runAndWait()
        self.read_page(pageObj)

        input("Continue?")
        
        for page in range(start+1, pdf.numPages):
            pageObj = pdf.getPage(page)
            self.engine.say(f"Page: {page}.") 
            self.engine.runAndWait()
            self.read_page(pageObj)
```

# The Read From Method

This is a simpler version of the above method that skips the meta and outline and just starts reading to EOF from page input.

```py
def read_from(self, pageNumber):
    # Display Full Path
    print(f"{self.path}\n")

    with open(self.path, 'rb') as f:
        pdf = PdfFileReader(f, strict=False)

        for page in range(pageNumber-1, pdf.numPages):
            pageObj = pdf.getPage(page)
            self.engine.say(f"Page: {page+1}.") 
            self.engine.runAndWait()
            self.read_page(pageObj)
```

# The Read Page Method

This method is used in both the `read()` method and `read_from()` method.

```py
def read_page(self, pageObj):
    text  = pageObj.extractText()
    lines = [i.replace("\n", ' ').strip() + '.' for i in text.split('.')]
    
    Thread(target=handle_ctrl_c).start()

    for i, line in enumerate(lines, start=1):
        print(f"{i}: {line}", end="\n\n")
        self.engine.say(line) 
            self.engine.runAndWait()
```

# The Extract Info Method

This method is the workhorse of the text extraction. This is where the outline is parsed as well.

```py
def extract_info(self, pdf):
    '''
    This method will prompt the PDF's outline and return a menu dictionary.

    The menu dictionary's data structure is as follows:

    {
        index: (pageNumber, pageObject), 
        index: ...,
        ...
    }
    '''
    docInfo  = pdf.documentInfo
    numPages = pdf.numPages
    outlines = pdf.outlines
    menu     = dict()

    # Extract PDF Metadata
    for property in ['author', 'creator', 'producer', 'subject', 'title']:
        print(f"{property.capitalize():<11}: {getattr(docInfo, property)}")

    # Display Page Count
    print(f"Page Count : {numPages}")

    # Display Outline Neatly
    print(f"\nGenerating Outline\n")
    for i, obj in enumerate(outlines, start=1):
        print(f"{i:>4} {obj.title}")
        pageNumber = pdf.getDestinationPageNumber(obj)
        menu[i]    = (pageNumber, obj.title, pdf.getPage(pageNumber))

    return menu
```

## **Conclusion**

While the above program is functional, I'm looking for a solution in a different library that will support all PDFs without having to hand design my own PDF extraction library from scratch. If I find one, I'll update this article with a second attempt just above the conclusion section.

I've tried pdfminer.six but the documentation leaves much to be desired.

If you have any recommendations, please email me. Any suggestions you provide will be credited to you in future sections.