---
layout: post
title: "Precalculus Fundamentals, Part I 🧮"
date: 2022-03-27 00:01:00 -0700
categories: math algebra precalculus
published: false
---


## **Content Covered**

<!-- After passing college mathematics with perfect scores some years ago,  -->

I thought it would be interesting to create a series of math tutorials spanning from college algebra to calculus. Basic math knowledge is assumed, but I may add descriptions and clarifications for the younger audiences for which the first half of these articles are written.  Hopefully my notes will be useful to others going through these courses.

> Algebra is a branch of mathematics in which arithmetical operations and formal manipulations are applied to abstract symbols rather than specific numbers. &mdash; [Britannica (paraphrase)](https://www.britannica.com/science/algebra)

Here is an outline of my notes, which I used to pass my three month class, with perfect scores, in sixteen days:
- Essential Concepts
    <!-- - Real Numbers, Negative Numbers, Fractions, Absolute Values, Order of Operations -->
- Exponents
    <!-- - Scientific Notation -->
- Radicals
    <!-- - Rational Exponents, Rationalizing Denominators -->
- Expressions
    <!-- - Fractional Expressions, Rational Expressions -->
- Polynomials
    <!-- - Defining, Adding & Subtracting, Multiplying & Dividing, Factoring -->
- Complex Numbers
- Logarithms
- Equations & Inequalities
    <!-- - Linear, Quadratic, Absolute Value Inequalities, Systems of Equations & Inequalities, Exponential, Logarithmic Equations -->
- Functions
- Applications
    <!-- - Area, Volume, Pythagorean Theorem, Unit Conversion (1&2pdf), Arithmetic Sequences, Distance, Rate, Time, Percentage Problems, Mixture Problems, Weighted Average (2), System of Equations (3), Geometric Sequences -->



## **The Reunion of Broken Parts**

The Strassburg tablet is the oldest recorded use of algebra, dating back to 1800 B.C. during the time of the Old Babylonian Empire. The tablet shows the solution of a quadratic elliptic equation. The Plimpton 322 tablet gives a table of Pythagorean triples in Babylonian Cuneiform script. al-Khwarizmi established algebra as a mathematical discipline that is independent of geometry and arithmetic in his mathematical treatise "The Compendious Book on Calculation by Completion and Balancing," and it is from his early 9th century book cIlm **al-jabr** wa l-muqābala "The Science of Restoring and Balancing," where we get the word for Algebra.

> The roots of algebra can be traced to the ancient Babylonians, who developed an advanced arithmetical system with which they were able to do calculations in an algorithmic fashion. The Babylonians developed formulas to calculate solutions for problems typically solved today by using linear equations, quadratic equations, and indeterminate linear equations. By contrast, most Egyptians of this era, as well as Greek and Chinese mathematics in the 1st millennium BC, usually solved such equations by geometric methods, such as those described in the Rhind Mathematical Papyrus, Euclid's Elements, and The Nine Chapters on the Mathematical Art. &mdash; Wikipedia

<!-- > "a generalization of arithmetic in which letters representing numbers are combined according to the rules of arithmetic" &mdash; [Websters](https://www.merriam-webster.com/dictionary/algebra) -->

<!-- We will be making use of python's symbolic mathematics library `SymPy`. I thought it would be interesting to compare the processes between performing algebra by hand and by using the python computer programming language.

```py
from sympy import *
x, y, z = symbols('x,y,z')
init_printing(use_unicode=True, wrap_line=False)
``` -->

## **Essential Concepts**

# Set Theory, Roster Method, & Set-Builder Notation

A set is a collection of objects whose contents can be clearly determined. The objects in a set are called the elements of the set. 

- The **Roster method** uses commas to separate the elements of the set. 
- A set can also be written in **Set-builder notation** In which the elements of the set are described but not listed. 
- **Finite sets** have a countable number of elements. For example, $$\{a,b,c\}$$ is a set of three elements, thus it is a finite set. 
- **Infinite sets** contain an uncountable number of elements. For example, $$\{a,b,c,\dotsc\}$$ is a set with an infinite number of elements, thus it is an infinite set. 

|Notation|Description|
|:-:|:-:|
|$$\{\}$$|Braces indicate the beginning and end of a set notation; elements are separated by commas.|
|$$\dots$$|An ellipsis indicates the continuation of a pattern or an infinite set with no last element i.e. $$\{2, 4, 6, \dotsc, 16, \dotsc\}$$|
|$$\vert$$|This symbol is read *"such that"*.|
|$$\in$$|*"is a member of"* or *"is an element of"*|
|$$\notin$$|*"is not a member of"* or *"is not an element of"*|
|$$\varnothing$$|An empty or null set is a set which contains no elements, but which is a subset of all sets. This can also be written as $$\{\}$$|
|Roster Method|$$\{1, 2, 3, 4, 5, 6, 7\}$$|
|Set-Builder|$$\{x \vert x < 8, x \; is \; a \; natural \; number\}$$<br>$$\{x \vert x \leq 7, x \; is \; a \; natural \; number\}$$<br>$$\{x \vert 0 < x < 8, x \; is \; a \; whole \; number\}$$<br>$$\{x \vert 0 < x \leq 7, x \; is \; a \; whole \; number\}$$|

We can also perform operations on sets.

|Notation|Description|
|:-:|:-:|
|$$\cap$$|The Intersection of $$A \cap B$$ is the set of elements common to both sets.|
|$$\cup$$|The Union of $$A \cup B$$ is the set of elements that are members of set A or set B or both.|

<!-- # Number Systems, Categories, and Groups -->

# The Set of Real Numbers

The five categories of real numbers are natural, whole, integers, rational, and irrational. 

The sets that make up the real numbers are referred to as **subsets** of the real numbers, meaning that all elements in each subset are also elements in the set of real numbers.
- **Rational numbers** can be written as a ratio of two integers, and their decimal form has a *terminating or repeating* decimal pattern.
- **Irrational numbers** cannot be written as a ratio of two integers, and their decimal form has a *non-terminating and non-repeating* decimal pattern. Rational and irrational numbers together form the set of all real numbers.

|Set of All Real Numbers|$$\mathbb{R} = \{x \vert x \; is \; rational \; or \; x \; is \; irrational\}$$<br>$$\mathbb{R} = \{x \vert x \; is \; rational\} \cup \{x \vert x \; is \; irrational\}$$|
|:-:|:-:|
|Set of Rational Numbers|$$\mathbb{Q} = \{x\vert x = \frac{p}{q} \, with \, p \in \mathbb{Z}, q \in \mathbb{Z}, and \, q \ne 0\}$$<br>$$\mathbb{Q} = \{\frac{a}{b} \vert \, a \; and \; b \; are \; integers \; and \; b \ne 0\}$$|
|Set of Irrational Numbers|$$\mathbb{I} = \{x\vert x \in \mathbb{R}, x \notin \mathbb{Q}\}$$|
||$$\sqrt{3}, \sqrt{5}, \sqrt[2]{2}, \sqrt[3]{2}, \sqrt[3]{10}, \frac{3}{\pi^2}, ...$$<br>$$\pi \approx 3.141592..., \, e \approx 2.718281...$$|

<!-- The positive and negative integers, fractions, and zero together are called the rational numbers. The properties of the set of rational numbers are: *infinite*, *ordered* $$(a<b)$$, and *dense everywhere* $$(a<c<b)$$. -->

|Set of Rational Numbers|$$\mathbb{Q} = \{x\vert x = \frac{p}{q} \, with \, p \in \mathbb{Z}, q \in \mathbb{Z}, and \, q \ne 0\}$$|
|:-:|:-:|
|Set of Integers|$$\mathbb{Z} = \{...,-2,-1,0,1,2,...\}$$|
|Set of Whole Numbers|$$\mathbb{N}_{0} \, or \, \mathbb{W} = \{0,1,2,3,...\}$$|
|Set of Natural Numbers (*non-negative integers*)|$$\mathbb{N} = \{1,2,3,...\}$$|

<!-- # Irrational and Transcendental Numbers

Irrational numbers are characterized by a non-terminating decimal pattern and a non-repeating decimal pattern. For example, pi can be expressed as 3.141592654.... but it contains an infinite number of decimals, with no recognizable pattern of digits.

The set of rational numbers is not satisfactory for calculus. The simplest examples of algebraic irrationals are the real roots of $$x^n − a = 0 \, (a > 0)$$, as numbers of the form $$\sqrt[n]{a}$$, if they are not rational. The irrational numbers which are not algebraic irrationals are called transcendental.

|Set of Irrational Numbers|$$\mathbb{I} = \{x\vert x \in \mathbb{R}, x \notin \mathbb{Q}\}$$|
|:-:|:-:|
|Irrational Numbers|$$\sqrt{3}, \sqrt{5}, \sqrt[2]{2}, \sqrt[3]{2}, \sqrt[3]{10}, \frac{3}{\pi^2}, ...$$|
|Transcendental|$$\pi = 3.141592..., \, e = 2.718281...$$| -->

# The Properties of Real Numbers

Subtraction and division neither work with commutative operations nor associative. The number 0 is special for addition; it is called the additive identity because $$a + 0 = a$$ for any real number a, in much the same way as the number 1 is special for multiplication; it is called the multiplicative identity because $$a \cdot 1 = a$$ for any real number a.

|||
|:-:|:-:|
|**Commutative Property**|**Order**|
|Commutative Property of Addition|$$a + b = b + a$$|
|Commutative Property of Multiplication|$$ab = ba$$|
|**Associative Property**|**Grouping**|
|Associative Property of Addition|$$(a + b) + c = a + (b + c)$$|
|Associative Property of Multiplication|$$(ab)c = a(bc)$$|
|**Distributive Property**||
|Distributive Property of Multiplication over Addition|$$a \cdot (b + c) = a \cdot b + a \cdot c$$<br>$$a(b+c) = ab + ac$$<br>$$(b+c)a = ab + ac$$|
|**Identity Property**||
|Identity Property of Addition|$$a + 0 = a$$<br>$$0 + a = a$$|
|Identity Property of Multiplication|$$a \cdot 1 = a$$<br>$$1 \cdot a = a$$|
|**Inverse Property**||
|Inverse Property of Addition|$$a + (-a) = 0$$<br>$$(-a) + a = 0$$|
|Inverse Property of Multiplication|$$a \cdot \frac{1}{a} = 1, \; a \ne 0$$<br>$$\frac{1}{a} \cdot a = 1, \; a \ne 0$$|

# The Properties of Negative Numbers

When adding two numbers with matching signs, add the two numbers (as if they are positive) and keep the sign. When adding two numbers with opposite signs, subtract the smaller number from the larger number (as if they are positive), and keep the sign of the larger number. The product or quotient of two positive numbers is positive. The product or quotient of two negative numbers is also positive. The product or quotient of a positive and negative number is negative.

|Properties of Negatives|
|:-:|
|$$(-1)a = -a$$|
|$$-(-a) = a$$|
|$$(-a)b = a(-b) = -(ab)$$|
|$$(-a)(-b) = ab$$|
|$$-(a+b) = -a-b$$<br>i.e. $$–(a + b) = –1(a + b) = (–1)a + (–1)b = –a + (–b) = –a − b$$|
|$$-(a-b) = b-a$$|

# The Properties of Fractions

To add fractions, you must have a common denominator. Fractions that
have common denominators are called *like* fractions. Fractions that have
different denominators are called *unlike* fractions.

|Properties of Fractions||
|:-:|:-:|
|Multiply|$$\frac{a}{b} \cdot \frac{c}{d}=\frac{ac}{bd}$$|
|Divide<br>*"Keep, change, flip"*|$$\frac{a}{b} \div \frac{c}{d}=\frac{a}{b} \cdot \frac{d}{c}=\frac{ad}{bc}$$|
|Add<br>(*Like* Fractions)|$$\frac{a}{c} + \frac{b}{c}=\frac{a+b}{c}$$|
|Add<br>(*Unlike* Fractions)|$$\frac{a}{b} + \frac{c}{d}=\frac{ad+bc}{bd}$$|
|Cancel|$$\frac{ac}{bc} = \frac{a}{b}$$|
|Cross-multiply|$$If \; \frac{a}{b}=\frac{c}{d}, \, then \, ad=bc$$|

We usually find the Least Common Denominator (LCD) when adding *unlike* fractions. Then we treat them the same way as when we add *like* fractions. Remember that factors of a number are whole numbers that when multiplied together yield the number. Here is a small refresher on [finding the GCF & LCM](https://calcworkshop.com/fractions/gcf-lcm/).

|Find the LCD (The Right Way)|$$\frac{7}{36}+\frac{8}{48}$$|
|:-:|:-:|
|**Simplify**|$$\frac{8}{48}$$|
|Prime factorization of 8|$$2^3$$|
|Prime factorization of 48|$$2^4 \cdot 3$$|
|Greatest Common Factor|$$2^3 = 8$$|
|**Use Common Factor**|$$\frac{8 \, \div \, 8}{48 \, \div \, 8} = \frac{1}{6}$$|
|*\*Rewrite for clarity*|$$\frac{7}{36}+\frac{1}{6}$$|
|Prime factorization of 36|$$2^2 \cdot 3^2$$|
|Prime factorization of 6|$$2 \cdot 3$$|
|**Use Least Common Multiple**|$$2^2 \cdot 3^2 = 36$$|
|$$\frac{a}{c} + \frac{b}{c}=\frac{a+b}{c}$$|$$\frac{7 \, \cdot \, 1}{36 \, \cdot \, 1} + \frac{1 \, \cdot \, 6}{6 \, \cdot \, 6}$$<br>$$\frac{7}{36} + \frac{6}{36} = \frac{7+6}{36} = \frac{13}{36}$$|

If we don't simplify first, like we did above, we will create more work for ourselves in the longrun.

|Find the LCD (The Long Way)|$$\frac{7}{36}+\frac{8}{48}$$|
|:-:|:-:|
|Prime factorization of 36|$$2^2 \cdot 3^2$$|
|Prime factorization of 48|$$2^4 \cdot 3$$|
|**Use Least Common Multiple**|$$2^4 \cdot 3^2 = 144$$|
||$$36 \cdot x = 144$$<br>$$x = \frac{144}{36}$$<br>$$x = 4$$|
||$$48 \cdot x = 144$$<br>$$x = \frac{144}{48}$$<br>$$x = 3$$|
|$$\frac{a}{c} + \frac{b}{c}=\frac{a+b}{c}$$|$$\frac{7 \, \cdot \, 4}{36 \, \cdot \, 4} + \frac{8 \, \cdot \, 3}{48 \, \cdot \, 3}$$<br>$$\frac{28}{144} + \frac{24}{144} = \frac{28 + 24}{144} = \frac{52}{144}$$|
|**Simplify**|$$\frac{52}{144}$$|
|Prime factorization of 52|$$2^2 \cdot 13$$|
|Prime factorization of 144|$$2^4 \cdot 3^2$$|
|Greatest Common Factor|$$2^2 = 4$$|
|**Use Common Factor**|$$\frac{52 \, \div \, 4}{144 \, \div \, 4} = \frac{13}{36}$$|

This method is better for computer algorithms rather than a method used by hand, although the euclidean algorithm is just about as handy as a factor tree when finding the GCF.

|Solving without LCD (Textbook)|$$\frac{7}{36}+\frac{8}{48}$$|
|:-:|:-:|
|Cross-multiply then divide over the product of the denominators<br>$$\frac{a}{b} + \frac{c}{d}=\frac{ad+bc}{bd}$$|$$\frac{7 \, \cdot \, 48 \, + \, 36 \, \cdot \, 8}{36 \, \cdot \, 48}$$|
|Simplify|$$\frac{624}{1728}$$|
|Find GCF via Euclidean Algorithm|48|
|Use the GCF to find the result|$$\frac{624 \, \div \, 48}{1728 \, \div \, 48} = \frac{13}{36}$$|

# Absolute Values

<!-- Absolute value is defined as the distance a number is away from 0 on a number line. Because it's a distance, it will always be a non-negative number. In other words, it will either be zero or a positive number.  -->

The important thing to remember, in context to addition and subtraction, is to evaluate the absolute value first, and then perform the mathematical operations. The absolute value sign acts as a grouping symbol, not unlike a parenthetical. When multiplying and dividing absolute values, the product and quotient properties of absolute values will allow us to simplify absolute value expressions.

<!-- $$f(x)=\begin{cases}1, & x\in\mathbb{Q}\\ 0, & x\notin\mathbb{Q} \\ \end{cases}$$ -->

|Definition of Absolute Value|$$\lvert a\rvert = \begin{cases}\quad a, & if \, a \ge 0\\ \, -a, & if \, a \lt 0\\ \end{cases}$$|
|:-:|:-:|
|The absolute value of a number is always positive or zero.|$$\lvert a\rvert \ge 0$$|
|A number and its negative have the same absolute value.|$$\lvert a\rvert = \lvert -a\rvert$$|
|The absolute value of a product is the product of the absolute values.|$$\lvert ab\rvert = \lvert a\rvert \lvert b\rvert$$|
|The absolute value of a quotient is the quotient of the absolute values.|$$\lvert \frac{a}{b}\rvert = \frac{\lvert a\rvert}{\lvert b\rvert}$$|
|Triangle Inequality|$$\lvert a+b\rvert \le \lvert a\rvert + \lvert b\rvert$$|

# 📏 The Order of Operations

When PEMDAS becomes unclear, remember that math problems are solved left to right. Multiplication has the same precedence as division. The same is true of addition and subtraction. Grouping symbols fall under parentheses in PEMDAS, they include fraction bars, absolute value signs, and radicals.

|PE(MD)(AS)|$$8 \div 2(2+2)$$|
|:-:|:-:|
|(P)arentheses|$$8 \div 2 \cdot 4$$|
|(E)xponents <span style="font-weight:bold;color:#990000;">or</span><br>Square Roots|N/A|
|(M)ultiplication <span style="font-weight:bold;color:#990000;">or</span><br>(D)ivision<br><span style="font-style:italic;color:#990000;">Left-to-Right!</span>|$$4 \cdot 4$$<br>16|
|(A)ddition <span style="font-weight:bold;color:#990000;">or</span><br>(S)ubtraction<br><span style="font-style:italic;color:#990000;">Left-to-Right!</span>|N/A|

|PE(MD)(AS)|$$10 \cdot 4 - 2 \cdot (4^2 \div 4) \div 2 \div \frac{1}{2} + 10$$|
|:-:|:-:|
|(P)arentheses|$$(4^2 \div 4)$$<br>You could cancel<br>$$4^2 \div 4 = \frac{4^2}{4} = \frac{4 \, \cdot \cancel{4}}{\cancel{4}} = \frac{4}{1} = 4$$<br>See (E)xponents|
|(E)xponents|$$(4^2 \div 4) = (16 \div 4) = 4$$<br>$$10 \cdot 4 - 2 \cdot 4 \div 2 \div \frac{1}{2} + 10$$|
|(M)ultiplication<br>(D)ivision|$$40 - 2 \cdot 4 \div 2 \div \frac{1}{2} + 10$$<br>$$40 - 8 \div 2 \div \frac{1}{2} + 10$$<br>$$40 - 4 \div \frac{1}{2} + 10$$<br>$$40 - 4 \cdot 2 + 10$$<br>$$40 - 8 + 10$$|
|(A)ddition<br>(S)ubtraction|$$32 + 10$$<br>42|

<!-- # Simplifying Algebraic Expressions

An algebraic expression is simplified when parentheses have been removed and
like terms have been combined. -->

## 📏 **Exponents**

The word exponent comes from the Latin "expo" meaning "out of" and "ponere" meaning "place." The Babylonians living in present day Iraq were the first to work with exponents, dating back to the 23rd century BC or earlier!

|Laws of Exponents||
|:-:|:-:|
|Product Property<br>$$a^ma^n=a^{m+n}$$|$$a^3a^2 = (aaa)(aa) = a^5$$|
|Quotient Property<br>$$\frac{a^m}{a^n}=a^{m-n}$$|$$\frac{a^5}{a^2} = \frac{aaaaa}{aa} = aaa = a^3$$|
|Power of Power<br>$$(a^m)^n=a^{m \cdot n}$$|$$(a^2)^3 = a^2\cdot a^2\cdot a^2 = a^6$$|
|Power of Product<br>$$(ab)^n=a^nb^n$$|$$(ab)^3 = (ab)(ab)(ab) = a^3b^3$$|
|Power of Quotient<br>$$(\frac{a}{b})^n=\frac{a^n}{b^n}$$|$$(\frac{a}{b})^3 = (\frac{a}{b})(\frac{a}{b})(\frac{a}{b}) = \frac{a^3}{b^3}$$|
|||
|**Additional Properties of Exponents**||
|||
|Definition of Rational or<br>Property of Fractional Exp.<br>$$a^{\frac{m}{n}} = (\sqrt[n]{a})^m$$<br>$$a^{\frac{m}{n}} = \sqrt[n]{a^m}$$|$$(\sqrt[6]{3x})^5 = (3x)^{\frac{5}{6}}$$|
||$$\frac{1}{(\sqrt[3]{xy})^2} = (xy)^{-\frac{2}{3}}$$|
|Zero Property<br>$$a^0=1, \, if \, a \ne 0$$|$$7^0 = 1$$<br>$$(7x^3)^0 = 1$$<br>$$(-8)^0 = 1$$<br>$$-8^0 = -1$$|
|Property of Negative Ex.<br>$$a^{-n}=\frac{1}{a^n}, \, if \, n \gt 0$$|$$\frac{a^3b^{-2}c}{2d^{-1}e^{-4}f^2} = \frac{a^3cde^4}{2b^2f^2}$$|
|Property of Negative Ex.<br>$$(\frac{b}{a})^{-n} =\frac{b^{-n}}{a^{-n}}=\frac{a^{n}}{b^{n}}=(\frac{a}{b})^n$$|$$7^{-3} = \frac{1}{7^3} = \frac{1}{343}$$|
|Property of Negative Ex.<br>$$\frac{b^{-m}}{a^{-n}} = \frac{a^n}{b^m}$$||

# 📏 Scientific Notation

The Greek mathematician Archimedes (287 BC - 212 BC), developed a system for representing large numbers using a system very similar to scientific notation. Scientific notation is a way to express numbers as the product of a decimal number and a power of 10. It is important to remember that when you're writing numbers in scientific notation, you can only have one non-zero digit to the left of the decimal, but you can have any number of digits to the right. When you're writing in scientific notation, Keep in mind:
- Moving the decimal to the left is going to increase your exponent
- Moving the decimal to the right is going to decrease your exponent
- A positive exponent indicates moving the decimal to the right
- A negative exponent indicates moving the decimal to the left

<!-- [](https://en.wikipedia.org/wiki/Lists_of_astronomical_objects) -->
<!-- [list of the largest cosmic structures](https://en.wikipedia.org/wiki/List_of_largest_cosmic_structures) -->

<!-- We will use the mass of well known cosmological objects as an example. Tree, Pyramid, Mountain, Moon, Planet, Star, Star Cluster, Star Globular Cluster, Primordial Blackholes, Steller Blackholes, Quasi Star, Quasi Black Hole (Supermassive [A*] & Ultramassive Black Hole), Galaxy, Galaxy Group, Galaxy Cluster, Galaxy Supercluster, Galaxy Filament, Observable Universe. -->


Let's make use of the article [Orders of Magnitude](https://en.wikipedia.org/wiki/Orders_of_magnitude_(mass)), and for the sake of simplicity we will define:
- `Solar Mass` as $$M_☉ \approx 2 \times 10^{30}$$
- `Earth Mass` as $$M_🜨 \approx 6 \times 10^{24}$$

<!-- - `Earth Mass` as $$M_🜨 \approx 6 \times 10^{24} \; (3.0 \times 10^{−6} M_☉)$$ -->

|Scientific Notation|Mass In Kilograms|
|:-:|:-:|
|General Sherman<br>(The trunk of the largest living tree)<br>$$1 \times 10^6 \, kg$$|One Million Kilograms<br>1,000,000 kg|
|Great Pyramid of Giza<br>$$6 \times 10^9 \, kg$$|Six Billion Kilograms<br>6,000,000,000 kg|
|Total mass of the world's human population<br>$$4 \times 10^{11} \, kg$$|Four-Hundred Billion Kilograms<br>400,000,000,000 kg|
|A teaspoon (5 ml) of neutron star material<br>$$5.5 \times 10^{12} \, kg$$|Five Trillion Five Hundred Billion Kilograms<br>5,500,000,000,000 kg<br>12,125,424,420,168 lbs<br>6,062,712,210.0841 US Tons|
|[Mount Everest](http://www.viktorplamenov.com/home/estimating-the-mass-of-mount-everest)<br>$$2.96471\times 10^{14} \, kg$$|Two-Hundred and Ninety Six Trillion Four-Hundred and Seventy One Billion Kilograms<br>296,471,000,000,000 kg|
|Earth's Atmosphere<br>$$5.1\times 10^{18} \, kg$$|Five Quintillion One-Hundred Quadrillion Kilograms<br>5,100,000,000,000,000,000 kg|
|Earth's Oceans<br>$$1.4\times 10^{21} \, kg$$|One-Sextillion Four-Hundred Quintillion Kilograms<br>1,400,000,000,000,000,000,000 kg|
|The Moon<br>(Earth's Moon)<br>$$7.3\times 10^{22} \, kg$$|Seventy-Three Sextillion Kilograms<br>73,000,000,000,000,000,000,000 kg|
|The Planet Earth<br>$$6\times 10^{24} \, kg$$|Six Septillion Kilograms<br>6,000,000,000,000,000,000,000,000 kg<br>1 $$M_🜨$$ Earth Mass|
|The Sun<br>$$2 \times 10^{30} \, kg$$|Two Nonillion Kilograms<br>2,000,000,000,000,000,000,000,000,000,000 kg<br>One Solar Mass $$1 M_☉$$<br>$$\approx$$ Three-Hundred and Thirty Three Thousand Earth Masses<br>$$333,333.\bar{33} \, M_🜨$$|
|Sagittarius A*<br>Supermassive black hole at the center of the Milky Way<br>$$7–8\times 10^{36} \, kg$$|Eight Undecillion Kilograms<br>8,000,000,000,000,000,000,000,000,000,000,000,000 kg<br>Four Million Solar Masses<br>4,000,000 $$M_☉$$|
|Ton 618<br>Ultramassive black hole<br>$$6.6 \times 10^{10} * 1 \, M_☉$$<br>$$\approx 1.32 \times 10^{41}$$|One Hundred and Thirty Two Duodecillion<br>132,000,000,000,000,000,000,000,000,000,000,000,000,000 kg<br>Sixty-Six Billion Solar Masses<br>66,000,000,000 $$M_☉$$|
|The Milky Way Galaxy<br>$$1.2\times 10^{42} \, kg$$|One Tredecillion Two-Hundred<br>Duodecillion Kilograms<br>1,200,000,000,000,000,000,000,000,000,000,000,000,000,000 kg<br>Six-Hundred Billion Solar Masses<br>600,000,000,000 $$M_☉$$|
|Local Group<br>$$2–3\times 10^{42} \, kg$$|3,000,000,000,000,000,000,000,000,000,000,000,000,000,000 kg<br>One-Trillon Five Hudred Billion Solar Masses<br>1,500,000,000,000 $$M_☉$$|
|Hercules–Corona Borealis Great Wall<br>(The largest structure in the known universe)<br>$$4\times 10^{49} \, kg$$|40 Quindecillion Kilograms<br>40,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000 kg<br>Twenty Quintillion Solar Masses<br>20,000,000,000,000,000,000 $$M_☉$$|
|Mass of the observable universe<br>$$4.4506\times 10^{52} \, kg$$ **to** $$6\times 10^{52} \, kg$$|Sixty Sexdecillion Kilograms<br>60,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000 kg<br>Thirty Sextillion Solar Masses<br>30,000,000,000,000,000,000,000 $$M_☉$$|

<!-- |Cygnus A<br>2.5b sm???||
|Messier 87|6.5 sm???|
|OJ 287 Ultra 18 billion of sm???|| -->

<!-- |Earth's atmosphere||
|Earth's oceans|| -->

<!-- ```py
print(f"{2*10**30:,}")
# 2,000,000,000,000,000,000,000,000,000,000
print(f"{2000000000000000000000000000000:.0e}")
# 2e+30
``` -->

# Multiplication & Division With Scientific Notation

When multiplying and dividing in scientific notation, deal with the non-exponential and exponential (containing the power of 10) separately. Multiply or divide the decimal number part first, then multiply or divide the part with exponents, applying the property of exponents to either add or subtract the exponents

|Multiplication|$$(2.1\times 10^{-7})(3.7\times 10^5)$$|
|:-:|:-:|
|Deal with Numbers First|$$2.1\cdot 3.7 = 7.77$$|
|Use Product Property on Exponents|$$10^{-7}\cdot 10^5$$|
||$$10^{-7+5} = 10^{-2}$$|
|Solution|$$7.77\times 10^{-2}$$|

Often when we multiply or divide in scientific notation the end result is not in scientific notation. We will then have to convert the front number into scientific notation and then combine the 10’s using the product property of exponents and adding the exponents.

|Earth Times The Sun|$$(6\times 10^{24})(2\times 10^{30})$$|
|:-:|:-:|
|Deal with Numbers First|$$6\cdot 2 = 12$$|
|Convert to Scientific Notation|$$1.2\times 10^1$$|
|Use Product Property on Exponents|$$10^1\cdot 10^{24}\cdot 10^{30}$$|
||$$10^{1+24+30} = 10^{55}$$|
|Solution|$$1.2\times 10^{55}$$|

|Earth Cubed|$$(6\times 10^{24})^3$$|
|:-:|:-:|
|Power of Product Rule|$$(6)^3\times (10^{24})^3$$|
|Deal with Numbers First|$$6^3 = 216$$|
|Convert to Scientific Notation|$$2.16\times 10^2$$|
|*Rewrite for Clarity|$$2.16\times 10^2\cdot (10^{24})^3$$|
|Use Power of Power Rule on Exponents|$$(10^{24})^3 = 10^{24\cdot 3} = 10^{72}$$|
|*Rewrite for Clarity|$$2.16\times 10^2\cdot 10^{72}$$|
|Use Product Property on Exponents|$$10^2\cdot 10^{72} = 10^{2+72} = 10^{74}$$|
|Solution|$$2.16\times 10^{74}$$|

A similar process is used to divide in scientific notation. First, we divide the decimal part of the number, and then apply a property of exponents to easily divide the powers of 10.

|Division|$$\frac{4.96\times 10^4}{3.1\times 10^{-3}}$$|
|:-:|:-:|
|Deal with Numbers First|$$\frac{4.96}{3.1} = 1.6$$|
|Use Quotient Property on Exponents|$$\frac{10^4}{10^{-3}}$$|
||$$10^{4-(-3)} = 10^{4+3} = 10^7$$|
|Solution|$$1.6\times 10^7$$|

|The Sun Divided By The Earth|$$\frac{2\times 10^{30}}{6\times 10^{24}}$$|
|:-:|:-:|
|Deal with Numbers First|$$\frac{2}{6} = 0.3333333333333333$$|
|Convert to Scientific Notation|$$3.333333333333333\times 10^{-1}$$|
|Use Quotient Property on Exponents|$$\frac{10^{30}}{10^{24}}$$|
||$$10^{30-24} = 10^6$$|
|Use Product Property on Exponents|$$10^{-1}\cdot 10^6$$|
||$$10^{-1+6} = 10^5$$|
|Solution|$$3.333333333333333\times 10^5 \, kg$$|
||1 $$M_☉$$<br>333,333.333333333333333 $$M_🜨$$|

|Earth Times The Sun<br>Divided By The Moon|$$(6\times 10^{24})(2\times 10^{30}) \over 7.3\times 10^{22}$$|
|:-:|:-:|
|Solution from above|$$1.2\times 10^{55} \over 7.3\times 10^{22}$$|
|Deal with Numbers First|$$\frac{1.2}{7.3} = 0.1643835616438356$$|
|Convert to Scientific Notation|$$0.1643835616438356 = 1.643835616438356\times 10^{-1}$$|
|*Rewrite for Clarity|$$1.643835616438356\times 10^{-1} \cdot \frac{10^{55}}{10^{22}}$$|
|Use Quotient Property|$$\frac{10^{55}}{10^{22}}=10^{55-22}=10^{33}$$|
|*Rewrite for Clarity|$$1.643835616438356\times 10^{-1} \cdot 10^{33}$$|
|Use Product Property|$$10^{-1}\cdot 10^{33} = 10^{-1+33} = 10^{32}$$|
|Solution|$$1.643835616438356\times 10^{32}$$|

# Addition & Subtraction With Scientific Notation

|Addition (Like Powers)<br>A teaspoon (5 ml) of<br>neutron star material<br>(5000 million tonnes)|$$(5.5\times 10^{12})+(5.5\times 10^{12})$$|
|:-:|:-:|
|Group Decimals First|$$(5.5+5.5)\times 10^{12}$$|
|Convert to Scientific Notation|$$11\times 10^{12} = 1.1\times 10^{13}$$|
|Solution<br>(2 teaspoons)|$$1.1\times 10^{13}$$|

|Subtraction (Like Powers)<br>The Milky Way Galaxy<br>from the Local Group|$$(3\times 10^{42})-(1.2\times 10^{42})$$|
|:-:|:-:|
|Group Decimals First|$$(3-1.2)\times 10^{42} = 1.8\times 10^{42}$$|
|Solution|$$1.8\times 10^{42}$$|

|Subtraction (Like Powers)<br>The Milky Way Galaxy<br>from the Local Group|$$(2\times 10^{42})-(1.2\times 10^{42})$$|
|:-:|:-:|
|Group Decimals First|$$(2-1.2)\times 10^{42} = 0.8\times 10^{42}$$|
|Convert to Scientific Notation|$$0.8\times 10^{42} = 8\times 10^{41}$$|
|Solution|$$8\times 10^{41}$$|

Remember that when converting into scientific notation, if we move the decimal to the left, this increases the exponent. If we move the decimal to the right, this decreases the exponent. Note that it does not matter which number you choose to rewrite. We would get the same result if we chose to rewrite the other number.

|Addition (Unlike Powers)<br>Earth's Atmosphere and Oceans|$$(5.1\times 10^{18}) + (1.4\times 10^{21})$$|
|:-:|:-:|
|Rewrite to have a common power|$$(5.1\times 10^{18}) + (1400\times 10^{18})$$|
|Group Numbers First|$$(5.1+1400)\times 10^{18} = 1405.1\times 10^{18}$$|
|Convert to Scientific Notation|$$1.4051\times 10^{21}$$|

|Subtraction (Unlike Powers)<br>Earth's Atmosphere and Oceans<br>from Earth's Mass|$$(6\times 10^{24})-(1.4051\times 10^{21})$$|
|:-:|:-:|
|Rewrite to have a common power|$$(6000\times 10^{21})-(1.4051\times 10^{21})$$|
|Group Numbers First|$$(6000-1.4051)\times 10^{21} = 5998.5949\times 10^{21}$$|
|Convert to Scientific Notation|$$5.9985949\times 10^{24}$$|

## 📏 **Radicals**

Just as multiplication and division are inverse operations of one another, radicals and exponents are also inverse operations of each other, meaning they cancel each other out. Fractional exponents and radicals can be written as one another by using the property of fractional exponents. The largest advantage of being able to change a radical expression into an exponential
expression is that we are now allowed to use all of our exponent properties to simplify.

|Properties of Radicals||
|:-:|:-:|
|Definition of n<sup>th</sup> root<br>$$\sqrt[n]{a}=b, \, means \, b^n=a$$<br>By the definition of n<sup>th</sup> root<br>$$a^{\frac{1}{n}}=\sqrt[n]{a}$$|$$(a^{\frac{1}{n}})^n=a^{(\frac{1}{n})n}=a^1=a$$|
|Root of a power<br>$$\sqrt[n]{a^m}=(\sqrt[n]{a})^m, \, if \, a \ge 0$$||
|Product Property<br>$$\sqrt[n]{ab} = \sqrt[n]{a} \cdot \sqrt[n]{b}$$<br>$$\sqrt[n]{x} \cdot \sqrt[n]{x} = x$$|$$\sqrt[3]{27xy^3} = \sqrt[3]{27}\cdot \sqrt[3]{x}\cdot \sqrt[3]{y^3}$$<br>$$3\cdot \sqrt[3]{x}\cdot y = 3y\sqrt[3]{x}$$|
|Definition of Rational or<br>Property of Fractional Exp.<br>$$a^{\frac{m}{n}} = (\sqrt[n]{a})^m$$<br>$$a^{\frac{m}{n}} = \sqrt[n]{a^m}$$|$$(\sqrt[6]{3x})^5 = (3x)^{\frac{5}{6}}$$<br>$$\frac{1}{(\sqrt[3]{xy})^2} = (xy)^{-\frac{2}{3}}$$|
|Quotient Property<br>$$\frac{\sqrt[n]{a}}{\sqrt[n]{b}}=\sqrt[n]{\frac{a}{b}}, \, if \, b \ne 0$$|$$\frac{\sqrt[3]{x^2y}}{\sqrt[3]{8xy^2}} = \sqrt[3]{\frac{x^2y}{8xy^2}}$$<br>$$\sqrt[3]{\frac{x}{8y}} = \sqrt[3]{\frac{1}{8}\cdot \frac{x}{y}}$$<br>$$\frac{\sqrt[3]{1}}{\sqrt[3]{8}}\cdot \sqrt[3]{\frac{x}{y}} = \frac{1}{2}\sqrt[3]{xy^{-1}}$$|
|$$\sqrt[n]{a^n} = a, \, if \, n \, is \, odd$$<br>$$\sqrt[n]{a^n} = \|a\|, \, if \, n \, is \, even$$||
|Root of a root<br>$$\sqrt[m]{\sqrt[n]{a}}=\sqrt[m \cdot n]{a}$$|$$\sqrt{\sqrt[3]{729}} = \sqrt[6]{729} = 3$$<br>$$\sqrt{x\sqrt{x}} = \sqrt{xx^{\frac{1}{2}}}$$<br>$$\sqrt{xx^{\frac{1}{2}}} = (xx^{\frac{1}{2}})^{\frac{1}{2}}$$<br>$$(x^{\frac{1}{1}}\cdot x^{\frac{1}{2}})^{\frac{1}{2}} = (x^{\frac{1}{1}+\frac{1}{2}})^{\frac{1}{2}}$$<br>$$(x^{\frac{3}{2}})^{\frac{1}{2}} = x^{\frac{3}{4}}$$<br>$$\sqrt[4]{x^3} = (\sqrt[4]{x})^3 = (x^3)^{\frac{1}{4}} = x^{\frac{3}{4}}$$|

# 📏 Rational or Fractional Exponents and Radicals

|Simplify|$$\sqrt[6]{y^5}\sqrt[3]{y^2}$$|
|:-:|:-:|
|Rewrite as Rational Exponents|$$y^{\frac{5}{6}} \cdot y^{\frac{2}{3}}$$|
|Product Property of Exponents|$$y^{\frac{5}{6}+\frac{2}{3}}$$|
|Find the LCM of 6 & 3|$$6 = 2 \cdot 3$$<br>$$3 = 1 \cdot 3$$<br>$$LCM \; is \; 6$$|
|Add Unlike Fractions w\ LCD|$$(\frac{5 \, \cdot \, 1}{6 \, \cdot \, 1}) + (\frac{2 \, \cdot \, 2}{3 \, \cdot \, 2}) = \frac{5+4}{6}$$|
|Find the GCF of 9 & 6|$$9 = 3^2$$<br>$$3 = 1 \cdot 3$$<br>$$GCF \; is \; 3$$|
|Simplify Fraction \w GCD|$$\frac{9 \, \div \, 3}{6 \, \div \, 3} = \frac{3}{2}$$|
|Solution|$$y^{\frac{3}{2}}$$|

|Simplify|$$\sqrt[3]{y\sqrt{y}} = y^{\frac{1}{2}}$$|
|:-:|:-:|
|Definition of Rational Exponents|$$\sqrt[3]{y\sqrt[2]{y^1}} = \sqrt[3]{y\cdot y^{\frac{1}{2}}}$$|
|Definition & Product Property|$$(y\cdot y^{\frac{1}{2}})^{\frac{1}{3}} = (y^{\frac{1}{1} + \frac{1}{2}})^{\frac{1}{3}} = (y^{\frac{2}{2} + \frac{1}{2}})^{\frac{1}{3}}$$|
|Power of power property|$$(y^{\frac{3}{2}})^{\frac{1}{3}} = y^{\frac{3}{2}\cdot \frac{1}{3}} = y^{\frac{3}{6}}$$|
|Solution|$$y^{\frac{1}{2}}$$|

# 📏 Adding & Subtracting Radical Expressions

Two or more square roots can be combined using the distributive property if they have the same radicand (number underneath the radical sign). Radicals are called **like radicals** if they have the same radicand and the same index (square root, cubed root, or fifth root). Sometimes you can break down a radicand into factors to simplify the radicand, you might then be able to combine it with another like term. In many ways, like radicals are analogous to like terms.

|Adding Like Radicals|$$7\sqrt{8}+6\sqrt{8}$$|
|:-:|:-:|
|Apply Distributive Property|$$(7+6)\sqrt{8}$$<br>$$13\sqrt{8}$$|
|Simplify|$$13 \cdot \sqrt{2 \cdot 4}$$<br>$$13 \cdot \sqrt{4} \cdot \sqrt{2}$$<br>$$13 \cdot 2 \cdot \sqrt{2}$$<br>$$26\sqrt{2}$$|

|Subtracting Like Radicals|$$7\sqrt{12}-8\sqrt{12}$$|
|:-:|:-:|
|Apply Distributive Property|$$(7-8)\sqrt{12}$$<br>$$-1\sqrt{12}$$|
|Simplify|$$-1 \cdot \sqrt{4 \cdot 3}$$<br>$$-1 \cdot \sqrt{4} \cdot \sqrt{3}$$<br>$$-1 \cdot 2 \cdot \sqrt{3}$$<br>$$-2\sqrt{3}$$|

When dealing with radicals with an unlike radicand, the first step is to simplify the radicands first.

|Unlike Radicals|$$4\sqrt{50x} - 6\sqrt{32x}$$|
|:-:|:-:|
|Simplify|$$4 \cdot \sqrt{50 \cdot x} - 6 \cdot \sqrt{32 \cdot x}$$<br>$$4 \cdot \sqrt{25 \cdot 2 \cdot x} - 6 \cdot \sqrt{16 \cdot 2 \cdot x}$$<br>$$4 \cdot 5 \cdot \sqrt{2 \cdot x} - 6 \cdot 4 \cdot \sqrt{2 \cdot x}$$<br>$$20 \cdot \sqrt{2x} - 24 \cdot \sqrt{2x}$$|
|Apply Distributive Property|$$(20-24) \cdot \sqrt{2x}$$<br>$$-4\sqrt{2x}$$|

# Multiplying Radical Expressions

|Multiplying Radicals|$$$$|
|:-:|:-:|
|||

# Rationalizing The Denominator or Numerator

If a fraction has a denominator of this form, we may rationalize the denominator by multiplying numerator and denominator by the conjugate radical.

|Rationalizing|$$A+B\sqrt{C}$$|
|:-:|:-:|
|Conjugate Radical|$$(A-B\sqrt{C})$$|
|Multiply|$$(A+B\sqrt{C})(A-B\sqrt{C})$$|
|notes*|$$A^2-B^2C$$|

*This works because, by special product forumla: $$(A+B)(A-B)=A^2-B^2$$, the product of the denominator and its conjugate radical does not contain a radical.

|Rationalizing|$$\frac{1}{1+\sqrt{2}}$$|
|:-:|:-:|
|Conjugate Radical|$$1-\sqrt{2}$$|
|Multiply|$$\frac{1}{1+\sqrt{2}} \cdot \frac{1-\sqrt{2}}{1-\sqrt{2}}$$|
||$$\frac{1 \cdot 1 - \sqrt{2}}{(1+\sqrt{2})(1-\sqrt{2})}$$|
||$$\frac{1 - \sqrt{2}}{(1)^2-(\sqrt{2})^2}$$|
||$$\frac{1 - \sqrt{2}}{1-2}$$|
|Any expression divided<br>by -1 equals it's opposite.<br>Any expression divided by<br>it's opposite equals -1.|$$\frac{1 - \sqrt{2}}{-1}$$|
||$$-(1 - \sqrt{2})$$|
||$$\sqrt{2}-1$$|

|Rationalizing|$$\frac{\sqrt{4+h}-2}{h}$$|
|:-:|:-:|
||$$\frac{\sqrt{4+h}-2}{h} \cdot \frac{\sqrt{4+h}+2}{\sqrt{4+h}+2}$$|
||$$\frac{(\sqrt{4+h})^2-(2)^2}{h(\sqrt{4+h}+2)}$$|
||$$\frac{4+h-4}{h(\sqrt{4+h}+2)}$$|
||$$\frac{h}{h(\sqrt{4+h}+2)}$$|
|Cancel|$$\frac{1}{\sqrt{4+h}+2}$$|


## Polynomials

$$a_nx^n+a_{n-1}x^{n-1} + a_{n-2}x^{n-2} + \dots + a_1x + a_0,$$

A **polynomial** is composed of a single term or a sum of two or more terms containing variables with whole-number exponents. If a polynomial can be considered simplified when it contains no grouping symbols and no like terms, then a simplified polynomial of precisely one term is called a **monomial**. A simplified polynomial of two and three terms are referred to as a **binomial** and a **trinomial** respectively. Simplified polynomials with four or more terms have no special names. 

Another way of looking at it might be to say that a polynomial is simply a sum of monomials, a binomial is a sum of two monomials, and a trinomial is the sum of three monomials. The **standard form** of a polynomial is written with the terms in the order of descending powers of the variable. **The degree of a polynomial** is the greatest degree of all the terms of the polynomial. 

|Defining Polynomials||
|:-:|:-:|
|Linear ($$x^1$$) Binomial (2 Terms)|$$5x+1$$|
|Quadratic ($$x^2$$) Trinomial (3 Terms)|$$2x^2-3x+4$$|
|Cubic ($$x^3$$) Four Term Polynomial<br>(Not written in standard form)|$$3-x+x^2-\frac{1}{2}x^3$$|

# 📏 Manipulating Algebraic Expressions
<!-- # Adding & Subtracting Polynomials -->

|Adding Polynomials||
|:-:|:-:|
|Find the sum|$$(x^3-6x^2+2x+4) + (x^3+5x^2-7x)$$|
|Group Like Terms|$$(x^3+x^3) + (-6x^2+5x^2) + (2x-7x) + 4$$|
|Combine Like Terms|$$2x^3 - x^2 - 5x + 4$$|

<!-- - **Find the sum:** $$(x^3-6x^2+2x+4) + (x^3+5x^2-7x)$$

    Since applying distributive property to the plus sign on the right hand side of the expression (cubic polynomial) yields no discernable changes, our first course of action is to group like terms using commutative & associative properties. Finally, combining like terms yields the result. -->

|Subtracting Polynomials||
|:-:|:-:|
|Find the difference|$$(x^3-6x^2+2x+4) - (x^3+5x^2-7x)$$|
|Distribute Negative|$$x^3-6x^2+2x+4-x^3-5x^2+7x$$|
|Group Like Terms|$$(x^3-x^3) + (-6x^2-5x^2) + (2x+7x) + 4$$|
|Combine Like Terms|$$-11x^2 + 9x + 4$$|

<!-- # Manipulating Algebraic Expressions In Python

- **Find the Sum:** $$(x^3-6x^2+2x+4) + (x^3+5x^2-7x)$$

    ```py
    e1 = x**3-6*x**2+2*x+4
    e2 = x**3+5*x**2-7*x

    print(e1.as_poly() + e2.as_poly())
    ```
    Result: $$2x^3 - x^2 - 5x + 4$$
    ```py
    Poly(2*x**3 - x**2 - 5*x + 4, x, domain='ZZ')
    ```

- **Find the difference:** $$(x^3-6x^2+2x+4) - (x^3+5x^2-7x)$$

    ```py
    e1 = x**3-6*x**2+2*x+4
    e2 = x**3+5*x**2-7*x

    print(e1.as_poly() - e2.as_poly())
    ```
    Result: $$-11x^2 + 9x + 4$$
    ```py
    Poly(-11*x**2 + 9*x + 4, x, domain='ZZ')
    ``` -->


# 📏 Multiplying Polynomials

|Multiplying Binomials Using FOIL|$$(2x+1)(3x-5)$$|
|:-:|:-:|
|(**F**)irst|$$2x \cdot 3x=\bf6x^2$$|
|(**O**)uter|$$2x-5=\bf-10x$$|
|(**I**)nner|$$1 \cdot 3x=\bf3x$$|
|(**L**)ast|$$1 \cdot -5=\bf-5$$|
|Group like terms|$$\bf6x^2-10x+3x-5$$|
|Combine like terms|$$6x^2-7x-5$$|

|Multiplying Polynomials|$$(2x+3)(x^2-5x+4)$$|
|:-:|:-:|
|Distribute|$$2x(x^2-5x+4) \, \boldsymbol{+} \, 3(x^2-5x+4)$$|
||$$(2x^3-10x^2+8x) + (3x^2-15x+12)$$|
|Group like terms|$$2x^3 + (-10x^2+3x^2) + (8x-15x) + 12$$|
|Combine like terms|$$2x^3-7x^2-7x+12$$|

# Factoring Polynomials

|Factoring Binomials|$$3x^2-6x$$|
|:-:|:-:|
|Find the GCF|$$3x$$|
|Factored|$$\boldsymbol{3x}(x-2)$$|

|Factoring Trinomials|$$8x^4y^2+6x^3y^3-2xy^4$$|
|:-:|:-:|
|Find the GCF|$$2xy^2$$|
||$$8, 6, -2$$ have a GCF of $$\bf2$$|
||$$x^4, x^3, x$$ have a GCF of $$\bf{x}$$|
||$$2xy^2$$ is the GCF of the trinomial|
|Factored|$$2xy^2(4x^3+3x^2y-y^2)$$|

|Factoring Four Terms|$$(2x+4)(x-3)-5(x-3)$$|
|:-:|:-:|
|Find the GCF|$$(x-3)$$|
||Sometimes, the GCF of an expression is not just a monomial but an entire parenthetical quantity. The two terms $$(2x+4)$$ and $$-5$$ have a common factor $$(x-3)$$|
|Factored|$$[(2x+4)-5](x-3)$$|
|Distribute & Simplify|$$(2x-1)(x-3)$$|

- Factoring quadratic trinomials of the form $$x^2+bx+c$$
    - Find two numbers whose sum equals the coefficient of the x term (b) and whose product is equal to the constant term (c): $$x^2-4x-12$$
    - Negative six, and positive two: $$(x-6)(x+2)$$

- Factoring quadratic trinomials of the form $$ax^2+bx+c \, where \, a \ne 1$$
    - Find two numbers whose sum equals the linear coefficient (b) and whose product is equal to the leading coefficient (a) by the additive constant (c)
    - If the constant term is negative then the signs in each factor must be different, if the sign is positive then they are the same.
    - : $$2x^2+9x-5$$
    - : $$2x^2+(10-1)x-5$$
    - : $$2x^2+10x-x-5$$
    - : $$2x(x+5)-1(x+5)$$
    - : $$(x+5)(2x-1)$$

# Factoring Expressions With Fractional Exponents

|Polynomial with Fractional Exponents|$$3x^{\frac 3 2}-9x^{\frac 1 2}+6x^{-\frac 1 2}$$|
|:-:|:-:|
|Factor out $$3x^{-\frac 1 2}$$|$$3x^{-\frac 1 2}(x^2-3x+2)$$|
|To factor out the $$x^{-\frac 1 2}$$ from $$x^{\frac 3 2}$$,<br> we subtract exponents|$$x^{-\frac 1 2}(x^{\frac{3}{2}-(-\frac{1}{2})})$$ <br> $$x^{-\frac 1 2}(x^{\frac{3}{2}+\frac{1}{2}})$$ <br> $$x^{-\frac 1 2}(x^2)$$|
|Factor Quadratic|$$3x^{-\frac 1 2}(x-1)(x-2)$$|
        
# Special Product & Factoring Formulas

If A and B are any real numbers or algebraic expressions, then ...

| Difference of Squares |
|:-:|
|$$A^2-B^2=(A-B)(A+B)$$|
|$$4x^2-25$$ <br> $$(2x)^2-5^2$$ <br> $$(2x-5)(2x+5)$$|
|$$(x+y)^2-z^2$$ <br> $$(x+y-z)(x+y+z)$$|
|Sum and product of same terms <br> $$(2x - \sqrt{y})(2x + \sqrt{y})$$ <br> $$(2x)^2 - (\sqrt{y})^2$$ <br> $$4x^2 - y$$|
|$$(x+y-1)(x+y+1)$$ <br> $$[(x+y)-1][(x+y)+1]$$ <br> $$(x+y)^2-1^2$$ <br> $$x^2+2xy+y^2-1$$|

<!-- <p style="text-align: center;">XXX</p> -->

|Square of a Sum|
|:-:|
|$$(A+B)^2=A^2+2AB+B^2$$|
|$$(3x+5)^2$$ <br> $$(3x)^2 + 2(3x)(5) + (5)^2$$ <br> $$9x^2+30x+25$$|
|Perfect Square <br> $$x^2+6x+9$$ <br> $$x^2+2x3+3^2$$ <br> $$(x+3)^2$$|

|Square of a Difference|
|:-:|
|$$(A-B)^2=A^2-2AB+B^2$$|
|Perfect Square <br> $$4x^2-4xy+y^2$$ <br> $$(2x)^2-2(2x)(y)+y^2$$ <br> $$(2x-y)^2$$|

<!-- i.e. (see square of a sum) -->

<!-- # Exclusively Special Product Formulas -->

|Cube of a sum|
|:-:|
|$$(A+B)^3=A^3+3A^2B+3AB^2+B^3$$|

<p style="text-align:center;">
i.e. (see cube of a difference)
</p> 

<!--<hr style="height:1px;background-color:grey;border:none;"> -->

|Cube of a difference|
|:-:|
|$$(A-B)^3=A^3-3A^2B+3AB^2-B^3$$|
|$$(x^2-2)^3$$ <br> $$(x^2)^3-3(x^2)^2(2)+3(x^2)(2)^2-2^3$$ <br> $$x^6-6x^4+12x^2-8$$|

<!-- # Exclusively Special Factoring Formulas -->

|Difference of Cubes|
|:---:|
|$$A^3-B^3=(A-B)(A^2+AB+B^2)$$|
|$$27x^3-1$$ <br> $$(3x)^3-1^3$$ <br> $$(3x-1)((3x)^2+(3x)(1)+1^2)$$ <br> $$(3x-1)(9x^2+3x+1)$$|

|Sum of Cubes|
|:---:|
|$$A^3+B^3=(A+B)(A^2-AB+B^2)$$|
|$$x^6+8$$ <br> $$(x^2)^3+2^3$$ <br> $$(x^2+2)((x^2)^2-(x^2)2+2^2)$$ <br> $$(x^2+2)(x^4-2x^2+4)$$| 

<!-- # Multiplying & Factoring Polynomials in Python

- **Using expand() to multiply:** $$(2x+3)(x^2-5x+4)$$

    ```py
    expand((2*x+3)*(x**2-5*x+4))
    # result: 2*x**3 - 7*x**2 - 7*x + 12
    ```
    result: $$2x^3-7x^2-7x+12$$

- **Using factor() to factor:** $$8x^4y^2+6x^3y^3-2xy^4$$

    ```py
    factor(8*x**4*y**2+6*x**3*y**3-2*x*y**4)
    # result: 2*x*y**2*(4*x**3 + 3*x**2*y - y**2)
    ```
    result: $$2xy^2(4x^3+3x^2y-y^2)$$

- **Using gcd() to find the GCF of:** $$8x^4y^2+6x^3y^3-2xy^4$$

    ```py
    gcd([8, 6, -2])
    # result: 2
    gcd([x**4, x**3, x])
    # result: x
    gcd([y**2, y**3, y**4])
    # result: y**2
    gcd([8*x**4*y**2, 6*x**3*y**3, 2*x*y**4])
    # result: 2*x*y**2
    ```
    result: $$2xy^2$$  -->

## **Complex Numbers**

|Complex Numbers|
|:-:|
|$$i = \sqrt{-1}$$|
|$$i^2 = -1$$<br>$$\sqrt[n]{x} \cdot \sqrt[n]{x} = x$$|
|$$i^3 = -i$$<br>$$-\sqrt{-1}$$|
|$$i^4 = 1$$<br>$$-1\cdot\sqrt{-1}\cdot\sqrt{-1}$$<br>$$i\cdot i\cdot i\cdot i$$<br>$$(i\cdot i)\cdot (i\cdot i)$$<br>$$-1\cdot-1=1$$|
|$$i^5 = i$$|

## **Logarithms**

|Properties of Logarithms||
|:-:|:-:|
||$$\log_b y=x, \, y=b^x$$|
|Product|$$\log_b xy = \log_b x + \log_b y$$|
|Quotient|$$\log_b \frac{x}{y} = \log_b x - \log_b y$$|
|Power|$$\log_b x^p = p \cdot \log_b x $$|
|Change of Base|$$\log_a x = \frac{\log_b x}{\log_b a}$$|
|Equality|$$\log_b x = \log_b y, \, x=y$$|



## **Applications**

# Pythagorean Theorem

We can use the **Pythagorean Theorem** to calculate the length of a diagonal. Remember that $$a^2 + b^2 = c^2$$ or alternatively $$c = \sqrt{a^2 + b^2}$$, where c represents the diagonal or hypotenuse. The Pythagorean Theorem utilizes the opposite & adjacent legs of the right triangle to find the hypotenuse. 

[![TRI](/assets/images/math/tri.png){:height="250px" width="250px" style="display:block; margin-left:auto; margin-right:auto"}](https://sciencenotes.org/wp-content/uploads/2016/09/trigtriangle.png)

|Pythagorean Theorem|$$a=7 ft, \, b=8 ft$$|
|:-:|:-:|
|Substitute|$$a^2 + b^2 = c^2$$<br>$$(7 \, ft)^2 + (8 \, ft)^2 = c^2$$|
|Square|$$49 \, ft^2 + 64 \, ft^2 = c^2$$|
|Combine|$$113 \, ft^2 = c^2$$|
|Square Both Sides|$$\sqrt{113 \, ft^2} = \sqrt{c^2}$$|
|Solution|$$c = 10.63014581273465 \, ft$$|
|Round to tenths place|$$c = 10.63 \, ft$$|

# Common Formulas

There are a few common formulas you should be familiar with below before moving on to the next article.

![GEO](/assets/images/math/geo.jpg)


<!-- # Unit Conversion -->






<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

