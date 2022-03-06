---
layout: post
title: "Web Scraping: Requests And BeautifulSoup 🌐"
date: 2022-02-22 12:00:05 -0700
categories: python web spidering crawling scraping data extraction
published: true
---

Well, it's 2/22/2022 on a Tuesday, and in the past 2 weeks I've read over 22 books on wisdom and philosophy. I thought that was odd and that I should just go ahead and publish this article. The books which resonated with me you can read on my about page. Happy 2's Day!

## **Content Covered**

The *Web Scraping* series of articles cover various methods and libraries that achieve the end goal of crawling websites and performing data extraction via scraping. Making use of a websites REST API has been covered by previous articles. These articles will focus on the methods available to us when we encounter a site without a provided application programming interface.

- The Requests & BeautifulSoup Libraries
    - IPs, geolocation, dowloading images
    - Three examples of pagination
    - DataFrame string presentation

<!-- - Presentation of Data
    - DataFrame, JSON, CSV, Excel  -->

## **The Basics Of The Requests Library**

|:-:|
|[Requests Documentation](https://docs.python-requests.org/en/latest/)|


All of Requests’ functionality can be accessed by 7 methods. They all return an instance of the Response object. For the purposes of this article we will be focusing mainly upon the get and post methods and how they relate to the request method via `**kwargs`.

# The ***requests.request()*** Method

<!-- & The ***requests.Response*** Object -->

|:-:|:-:|
|[Documentation](https://docs.python-requests.org/en/latest/api/#requests.request)|[Source Code](https://docs.python-requests.org/en/latest/_modules/requests/api/#request)|

<!-- |:-:|:-:|
|[Documentation](https://docs.python-requests.org/en/latest/api/#requests.Response)|[Source Code](https://docs.python-requests.org/en/latest/_modules/requests/models/#Response)| -->

<!-- 
requests.***request***(method, url, **kwargs)

Constructs and sends a ***Request***.

- Parameters
    - **method** – method for the new ***Request*** object: **`GET`**, **`OPTIONS`**, **`HEAD`**, **`POST`**, **`PUT`**, **`PATCH`**, or **`DELETE`**.
    - **url** – URL for the new ***Request*** object.
    - **params** – (*optional*) Dictionary, list of tuples or bytes to send in the query string for the ***Request***.
    - **data** – (*optional*) Dictionary, list of tuples, bytes, or file-like object to send in the body of the ***Request***.
    - **json** – (*optional*) A JSON serializable Python object to send in the body of the ***Request***.
    - **headers** – (*optional*) Dictionary of HTTP Headers to send with the ***Request***.
    - **cookies** – (*optional*) Dict or CookieJar object to send with the ***Request***.
    - **files** – (*optional*) Dictionary of 'name': file-like-objects (*or {'name': file-tuple}*) for multipart encoding upload. file-tuple can be a 2-tuple (*'filename', fileobj*), 3-tuple (*'filename', fileobj, 'content_type'*) or a 4-tuple (*'filename', fileobj, 'content_type', custom_headers*), where 'content-type' is a string defining the content type of the given file and custom_headers a dict-like object containing additional headers to add for the file.
    - **auth** – (*optional*) Auth tuple to enable Basic/Digest/Custom HTTP Auth.
    - **timeout** (float or tuple) – (*optional*) How many seconds to wait for the server to send data before giving up, as a float, or a (connect timeout, read timeout) tuple.
    - **allow_redirects** (bool) – (*optional*) Boolean. Enable/disable GET/OPTIONS/POST/PUT/PATCH/DELETE/HEAD redirection. Defaults to True.
    - **proxies** – (*optional*) Dictionary mapping protocol to the URL of the proxy.
    - **verify** – (*optional*) Either a boolean, in which case it controls whether we verify the server’s TLS certificate, or a string, in which case it must be a path to a CA bundle to use. Defaults to True.
    - **stream** – (*optional*) if False, the ***response*** content will be immediately downloaded.
    - **cert** – (*optional*) if String, path to ssl client cert file (*.pem*). If Tuple, (*‘cert’, ‘key’*) pair.
-->

<!-- ```py
def request(method, url, **kwargs):
    """Constructs and sends a :class:`Request <Request>`.

    :param method: method for the new :class:`Request` object: ``GET``, ``OPTIONS``, ``HEAD``, ``POST``, ``PUT``, ``PATCH``, or ``DELETE``.
    :param url: URL for the new :class:`Request` object.
    :param params: (optional) Dictionary, list of tuples or bytes to send
        in the query string for the :class:`Request`.
    :param data: (optional) Dictionary, list of tuples, bytes, or file-like
        object to send in the body of the :class:`Request`.
    :param json: (optional) A JSON serializable Python object to send in the body of the :class:`Request`.
    :param headers: (optional) Dictionary of HTTP Headers to send with the :class:`Request`.
    :param cookies: (optional) Dict or CookieJar object to send with the :class:`Request`.
    :param files: (optional) Dictionary of ``'name': file-like-objects`` (or ``{'name': file-tuple}``) for multipart encoding upload.
        ``file-tuple`` can be a 2-tuple ``('filename', fileobj)``, 3-tuple ``('filename', fileobj, 'content_type')``
        or a 4-tuple ``('filename', fileobj, 'content_type', custom_headers)``, where ``'content-type'`` is a string
        defining the content type of the given file and ``custom_headers`` a dict-like object containing additional headers
        to add for the file.
    :param auth: (optional) Auth tuple to enable Basic/Digest/Custom HTTP Auth.
    :param timeout: (optional) How many seconds to wait for the server to send data
        before giving up, as a float, or a :ref:`(connect timeout, read
        timeout) <timeouts>` tuple.
    :type timeout: float or tuple
    :param allow_redirects: (optional) Boolean. Enable/disable GET/OPTIONS/POST/PUT/PATCH/DELETE/HEAD redirection. Defaults to ``True``.
    :type allow_redirects: bool
    :param proxies: (optional) Dictionary mapping protocol to the URL of the proxy.
    :param verify: (optional) Either a boolean, in which case it controls whether we verify
            the server's TLS certificate, or a string, in which case it must be a path
            to a CA bundle to use. Defaults to ``True``.
    :param stream: (optional) if ``False``, the response content will be immediately downloaded.
    :param cert: (optional) if String, path to ssl client cert file (.pem). If Tuple, ('cert', 'key') pair.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response

    Usage::

      >>> import requests
      >>> req = requests.request('GET', 'https://httpbin.org/get')
      >>> req
      <Response [200]>
    """

    # By using the 'with' statement we are sure the session is closed, thus we
    # avoid leaving sockets open which can trigger a ResourceWarning in some
    # cases, and look like a memory leak in others.
    with sessions.Session() as session:
        return session.request(method=method, url=url, **kwargs)
``` -->

```py
import requests

def main():
    url  = "https://httpbin.org/get"
    head = {"accept": "application/json"}
    res  = requests.request('GET', url, headers=head)
    
    if res.ok:
        print(res.text)
    
if __name__ == "__main__":
    main()
```

```json
{
  "args": {},
  "headers": {
    "Accept": "application/json",
    "Accept-Encoding": "gzip, deflate",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.24.0",
    "X-Amzn-Trace-Id": "Root=1-61f8b284-21f6e43a74afc2812004b83e"
  },
  "origin": "138.199.21.31",
  "url": "https://httpbin.org/get"
}
```


# The ***requests.get()*** Method

|:-:|:-:|:-:|
|[Documentation](https://docs.python-requests.org/en/latest/api/#requests.get)|[Source Code](https://docs.python-requests.org/en/latest/_modules/requests/api/#get)|[RFC: GET Method](https://datatracker.ietf.org/doc/html/rfc2616/#section-9.3)|

<!-- ```py
def get(url, params=None, **kwargs):
    r"""Sends a GET request.

    :param url: URL for the new :class:`Request` object.
    :param params: (optional) Dictionary, list of tuples or bytes to send
        in the query string for the :class:`Request`.
    :param \*\*kwargs: Optional arguments that ``request`` takes.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    """

    return request('get', url, params=params, **kwargs)
``` -->

Let's use one of the [Console Services](https://github.com/chubin/awesome-console-services) from the linked repository to demonstrate the get method functionality. Continuing on from the requests.request() example, let's take the provided IP address and geolocate it's origin using ip-api.com.

```py
import requests

def main():
    ip  = "138.199.21.31"
    url = f"http://ip-api.com/json/{ip}"
    res = requests.get(url)
    
    if res.ok:
        for key, value in res.json().items():
            print(f"{key.title():<11}: {value}")
        
if __name__ == "__main__":
    main()
```

```
Status     : success
Country    : Japan
Countrycode: JP
Region     : 13
Regionname : Tokyo
City       : Tokyo
Zip        : 102-0082
Lat        : 35.6893
Lon        : 139.6899
Timezone   : Asia/Tokyo
Isp        : Datacamp Limited
Org        : Datacamp Limited
As         : AS212238 Datacamp Limited
Query      : 138.199.21.31
```

# The ***requests.post()*** Method

|:-:|:-:|:-:|
|[Documentation](https://docs.python-requests.org/en/latest/api/#requests.post)|[Source Code](https://docs.python-requests.org/en/latest/_modules/requests/api/#post)|[RFC: POST Method](https://datatracker.ietf.org/doc/html/rfc2616/#section-9.5)|

<!-- ```py
def post(url, data=None, json=None, **kwargs):
    r"""Sends a POST request.

    :param url: URL for the new :class:`Request` object.
    :param data: (optional) Dictionary, list of tuples, bytes, or file-like
        object to send in the body of the :class:`Request`.
    :param json: (optional) json data to send in the body of the :class:`Request`.
    :param \*\*kwargs: Optional arguments that ``request`` takes.
    :return: :class:`Response <Response>` object
    :rtype: requests.Response
    """

    return request('post', url, data=data, json=json, **kwargs)
``` -->

```py
import requests

def main():
    url  = "https://httpbin.org/post"
    head = {"accept": "application/json"}
    res  = requests.post(url, headers=head)
    
    if res.ok:
        print(res.text)

if __name__ == "__main__":
    main()
```

```json
{
  "args": {},
  "data": "",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "application/json",
    "Accept-Encoding": "gzip, deflate",
    "Content-Length": "0",
    "Host": "httpbin.org",
    "User-Agent": "python-requests/2.24.0",
    "X-Amzn-Trace-Id": "Root=1-61f8b35e-17d43f54571b9c9e201b27d3"
  },
  "json": null,
  "origin": "138.199.21.31",
  "url": "https://httpbin.org/post"
}
```

# Sessions & Persistence

|:-:|:-:|:-:|
|[Advanced](https://docs.python-requests.org/en/latest/user/advanced/)|[Documentation](https://docs.python-requests.org/en/latest/api/#requests.Session)|[Source Code](https://docs.python-requests.org/en/latest/_modules/requests/sessions/#Session)|

```py
import requests

def main():
    url = "https://google.com"
    
    with requests.Session() as s:
        res = s.get(url)
        
        if res.ok:
            for key, value in res.cookies.items():
                print(f"{key:<6}: {value}")

if __name__ == "__main__":
    main()                
```

```
1P_JAR: 2022-02-01-05
NID   : 511=GbFw1r7NlDnQm0sznSvZEv_CMReI3stJikhPuuxmzcVPG9XSQDZxrJnfluAMtA4wNNZYoX0uaO5VaKA1tO73H6bv0cQ1S784Qe4onYK40aUtj6jodxVcIid-Rwdzp0IE6RMT6NzYMb2TQI1CVFjWqqK_16kBGICqPZ8DB2TjtVI
```

```py
import requests

def main():
    urlSet = "https://httpbin.org/cookies/set"
    urlDel = "https://httpbin.org/cookies/delete"
    
    param  = {"hello" : "cookie"}
    head   = {"accept": "text/plain"}
    
    with requests.Session() as s:
        res = s.get(urlSet, params=param, headers=head)
    
        if res.ok:
            print(res.text)
            
            res = s.get(urlDel, params=param, headers=head)
            print(res.text)
            
            # urlGet = "https://httpbin.org/cookies"
            # head = {"accept": "application/json"}
            # res  = s.get(urlGet, headers=head)
            # print(res.text)

if __name__ == "__main__":
    main()
```

```json
{
  "cookies": {
    "hello": "cookie"
  }
}

{
  "cookies": {}
}
```

# Downloading Images

```py
import requests

def main():
    url  = "https://httpbin.org/image/jpeg"
    head = {"accept": "image/jpeg"}
    res  = requests.get(url, headers=head)
    
    if res.ok:
        with open('image.jpg', 'wb') as f:
            f.write(res.content)

if __name__ == "__main__":
    main()
```

```py
import requests

def main():
    url  = "https://httpbin.org/image/jpeg"
    head = {"accept": "image/jpeg"}
    res  = requests.get(url, headers=head, stream=True)
    
    if res.ok:
        with open('image.jpg', 'wb') as f:
            for chunk in res.iter_content(chunk_size=2**10):
                f.write(chunk)

if __name__ == "__main__":
    main()
```

## **The Basics of BeautifulSoup**

|:-:|
|[BeautifulSoup Documentation](https://beautiful-soup-4.readthedocs.io/en/latest/)|

```html
<!DOCTYPE html>
<html lang="en-us">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width,initial-scale=1" />
        <title>BS4 Example</title>
        <style>
            table,td,th{border:1px solid #000}
        </style>
    </head>

    <body>
        <h1>Header Example</h1>
        <p>Paragraph Example</p>
        <table style="width:50%">
            <tr>
                <th>Name</th>
                <th>Age</th>
            </tr>
            <tr>
                <td>Jack</td>
                <td>35</td>
            </tr>
            <tr>
                <td>Jill</td>
                <td>25</td>
            </tr>
        </table>
    </body>
</html>
```

> Beautiful Soup transforms a complex HTML document into a complex tree of Python objects. But you’ll only ever have to deal with about four kinds of objects: **Tag**, **NavigableString**, **BeautifulSoup**, and **Comment**.

```py
from bs4 import BeautifulSoup

def main():
    html = """<!DOCTYPE html><html lang="en-us"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>BS4 Example</title><style>table,td,th{border:1px solid #000}</style></head><body><h1>Header Example</h1><p>Paragraph Example</p><table style="width:50%"><tr><th>Name</th><th>Age</th></tr><tr><td>Jack</td><td>35</td></tr><tr><td>Jill</td><td>25</td></tr></table></body></html>"""

    # Pass the document into the BeautifulSoup constructor to parse it. 
    soup = BeautifulSoup(html, 'html.parser')

    # First, the document is converted to Unicode, and HTML entities are converted to Unicode characters
    print(type(soup), soup)
    
    '''
    <class 'bs4.BeautifulSoup'> <!DOCTYPE html>
    <html lang="en-us"><head><meta charset="utf-8"/><meta content="width=device-width,initial-scale=1" name="viewport"/><title>BS4 Example</title><style>table,td,th{border:1px solid #000}</style></head><body><h1>Header Example</h1><p>Paragraph Example</p><table style="width:50%"><tr><th>Name</th><th>Age</th></tr><tr><td>Jack</td><td>35</td></tr><tr><td>Jill</td><td>25</td></tr></table></body></html>
    '''
    
    print(type(soup.head), soup.head)
    
    '''
    <class 'bs4.element.Tag'> <head><meta charset="utf-8"/><meta content="width=device-width,initial-scale=1" name="viewport"/><title>BS4 Example</title><style>table,td,th{border:1px solid #000}</style></head>
    '''
    
    contents = soup.head.contents
    print(type(contents), contents)
    
    for i in contents:
        print(i)
    
    '''
    <class 'list'> [<meta charset="utf-8"/>, <meta content="width=device-width,initial-scale=1" name="viewport"/>, <title>BS4 Example</title>, <style>table,td,th{border:1px solid #000}</style>]
    
    <meta charset="utf-8"/>
    <meta content="width=device-width,initial-scale=1" name="viewport"/>
    <title>BS4 Example</title>
    <style>table,td,th{border:1px solid #000}</style>
    '''
    
    children = soup.head.children
    print(type(children), children)
    
    '''
    <class 'list_iterator'> <list_iterator object at 0x0000018C4A512400>
    
    <meta charset="utf-8"/>
    <meta content="width=device-width,initial-scale=1" name="viewport"/>
    <title>BS4 Example</title>
    <style>table,td,th{border:1px solid #000}</style>
    '''
    
    descendants = soup.head.descendants
    print(type(descendants), descendants)
    
    for i in descendants:
        print(i)
    
    '''
    <class 'generator'> <generator object Tag.descendants at 0x00000283F3996350>
    
    <meta charset="utf-8"/>
    <meta content="width=device-width,initial-scale=1" name="viewport"/>
    <title>BS4 Example</title>
    BS4 Example
    <style>table,td,th{border:1px solid #000}</style>
    table,td,th{border:1px solid #000}
    '''
    
    print(type(soup.body), soup.body)
    
    '''
    <class 'bs4.element.Tag'> <body><h1>Header Example</h1><p>Paragraph Example</p><table style="width:50%"><tr><th>Name</th><th>Age</th></tr><tr><td>Jack</td><td>35</td></tr><tr><td>Jill</td><td>25</td></tr></table></body>
    '''
    
    # A Tag object corresponds to an XML or HTML tag in the original document
    # Every tag has a name, accessible as .name, If you change a tag’s name, the change will be reflected in any HTML markup generated by Beautiful Soup
    tag = soup.p # soup.body.p
    print(type(tag), tag, tag.text, tag.name, sep=", ")
    
    '''
    <class 'bs4.element.Tag'>, <p>Paragraph Example</p>, Paragraph Example, p
    '''
    
    # A tag may have any number of attributes. The tag <b id="boldest"> has an attribute “id” whose value is “boldest”. You can access a tag’s attributes by treating the tag like a dictionary
    tag = soup.meta
    print(type(tag), tag.attrs, tag['charset'], sep=", ")
    
    '''
    <class 'bs4.element.Tag'>, {'charset': 'UTF-8'}, UTF-8
    '''
    
    tags = soup.find_all('meta')
    print(type(tags), tags)
    
    '''
    <class 'bs4.element.ResultSet'> [<meta charset="utf-8"/>, <meta content="width=device-width,initial-scale=1" name="viewport"/>]
    '''
    
    # Another common task is extracting all the text from a page
    print(soup.get_text())
    
    '''
    BS4 ExampleHeader ExampleParagraph ExampleNameAgeJack35Jill25
    '''
    
    # If a tag has only one child, and that child is a NavigableString, the child is made available as .string
    # If a tag contains more than one thing, then it’s not clear what .string should refer to, so .string is defined to be None
    h = soup.find('h1')
    print(h.string)
    
    '''
    Header Example
    '''
    
if __name__ == "__main__":
    main()
```

## Example I. The Simplest Example Of Pagination

There's **two ways** we can go about implementing pagination. The first is where we're already privy to the amount of pages, and the query string is easily manipulated. For example: **`/?p=1`**, becomes **`/?p=2`**, then **`/?p=3`**, and so on. [https://nyaa.si/](https://nyaa.si/), a popular anime torrent site, is just such an example.

# Inspecting The Source Code

```html
<ul class="pagination">
    <li class="disabled"><a href="#">«</a></li>
        <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
        <li><a href="/?p=2">2</a></li>
        <li><a href="/?p=3">3</a></li>
        <li><a href="/?p=4">4</a></li>
        <li><a href="/?p=5">5</a></li>
        <li><a href="/?p=6">6</a></li>

    <li><a rel="next" href="/?p=2">»</a></li>
</ul>
```

# The Main Program 

```py
import requests

from bs4 import BeautifulSoup

def main():
    url = "https://nyaa.si/"
    
    for page in range(1,100+1):
        param = {'s':'seeders', 'o':'desc', 'p':page}
        res   = requests.get(url, params=param)
    
        if not res.ok:
            raise requests.HTTPError(res.url)
        
        print(f"[*] Scraping: {res.url}")
        
        # Process Data
    
        soup = BeautifulSoup(res.text, 'html.parser')
        rows = soup.table.tbody.find_all('tr')
        
        for r, row in enumerate(rows, start=1):
            for t, td in enumerate(row.find_all('td'), start=1):
                print(f"[*] Analyzing <pg: {page}; tr: {r}; td: {t}>\n")
                print(td.prettify(), end="\n\n")
            
            # Only first row
            break
        
        # Only first page
        break

if __name__ == "__main__":
    main()
```

## Example II: Pagination By Extracting Next

The second is the best way, and instead of hard coding the page count and iterating through said range, we instead extract the next page's whereabouts so we do not have to guess. This is especially useful when dealing with websites with a confusing and complex design where the page count is obscured or not as predictable.

# Inspecting The Source Code

In the last example, the "next" string was found in a hyperlink's relation attribute. Now that we've made a query, we find that the "next" string is not in a rel attribute, but in a list item's class attribute.

```html
<ul class="pagination">
    <li class="previous disabled unavailable"><a> « </a></li>
    <li class="active"><a>1</a></li>
    <li><a href="/?f=0&amp;c=0_0&amp;q=one+punch+man&amp;s=seeders&amp;o=desc&amp;p=2">2</a></li>
    <li><a href="/?f=0&amp;c=0_0&amp;q=one+punch+man&amp;s=seeders&amp;o=desc&amp;p=3">3</a></li>
    <li><a href="/?f=0&amp;c=0_0&amp;q=one+punch+man&amp;s=seeders&amp;o=desc&amp;p=4">4</a></li>
    <li><a href="/?f=0&amp;c=0_0&amp;q=one+punch+man&amp;s=seeders&amp;o=desc&amp;p=5">5</a></li>
    <li class="disabled"><a>...</a></li>
    <li><a href="/?f=0&amp;c=0_0&amp;q=one+punch+man&amp;s=seeders&amp;o=desc&amp;p=13">13</a></li>
    <li><a href="/?f=0&amp;c=0_0&amp;q=one+punch+man&amp;s=seeders&amp;o=desc&amp;p=14">14</a></li>
    <li class="next"><a href="/?f=0&amp;c=0_0&amp;q=one+punch+man&amp;s=seeders&amp;o=desc&amp;p=2">»</a></li>
</ul>
```

# The Main Program

```py
import requests

from bs4 import BeautifulSoup

def main():
    base = "https://nyaa.si"
    link = "/?f=0&c=0_0&q=one+punch+man&s=seeders&o=desc"
    page = 1 # A simple counter
    
    while True:
        url = base if not link else base+link
        res = requests.get(url)
    
        if not res.ok:
            raise requests.HTTPError(res.url)
        
        print(f"[*] Scraping: {res.url}")
        
        # Process Data
        soup = BeautifulSoup(res.text, 'html.parser')
        rows = soup.table.tbody.find_all('tr')
        
        for r, row in enumerate(rows, start=1):
            for t, td in enumerate(row.find_all('td'), start=1):
                print(f"[*] Analyzing <pg: {page}; tr: {r}; td: {t}>\n")
                print(td.prettify(), end="\n\n")
            
            # Only first row
            break
        
        # # Find link for next page
        try:
            next_ = soup.find('li', {"class": "next"})
            link = next_.a.get('href')
        except:
            data = soup.find('a', rel="next")
            link = data.get('href')
            
        # Handle last page
        if not link:
            print(f"[-] last page reached ...")
            break

        # Increase page counter
        page+=1
        
        # Only first 2 pages
        if page > 2:
            break

if __name__ == "__main__":
    main()
```

# The Output

```
[*] Scraping: https://nyaa.si/?f=0&c=0_0&q=one+punch+man&s=seeders&o=desc
[*] Analyzing <pg: 1; tr: 1; td: 1>

<td>
 <a href="/?c=1_2" title="Anime - English-translated">
  <img alt="Anime - English-translated" class="category-icon" src="/static/img/icons/nyaa/1_2.png"/>
 </a>
</td>


[*] Analyzing <pg: 1; tr: 1; td: 2>

<td colspan="2">
 <a class="comments" href="/view/1329204#comments" title="22 comments">
  <i class="fa fa-comments-o">
  </i>
  22
 </a>
 <a href="/view/1329204" title="[Judas] One Punch Man (Seasons 1-2 + OVAs + Specials) [BD 1080p][HEVC x265 10bit][Dual-Audio][Multi-Subs] (Batch)">
  [Judas] One Punch Man (Seasons 1-2 + OVAs + Specials) [BD 1080p][HEVC x265 10bit][Dual-Audio][Multi-Subs] (Batch)
 </a>
</td>


[*] Analyzing <pg: 1; tr: 1; td: 3>

<td class="text-center">
 <a href="/download/1329204.torrent">
  <i class="fa fa-fw fa-download">
  </i>
 </a>
 <a href="magnet:?xt=urn:btih:31e236514cbba971c479b47bd09581ff56b8a93c&amp;dn=%5BJudas%5D%20One%20Punch%20Man%20%28Seasons%201-2%20%2B%20OVAs%20%2B%20Specials%29%20%5BBD%201080p%5D%5BHEVC%20x265%2010bit%5D%5BDual-Audio%5D%5BMulti-Subs%5D%20%28Batch%29&amp;tr=http%3A%2F%2Fnyaa.tracker.wf%3A7777%2Fannounce&amp;tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&amp;tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&amp;tr=udp%3A%2F%2Fexodus.desync.com%3A6969%2Fannounce&amp;tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce">
  <i class="fa fa-fw fa-magnet">
  </i>
 </a>
</td>


[*] Analyzing <pg: 1; tr: 1; td: 4>

<td class="text-center">
 11.2 GiB
</td>


[*] Analyzing <pg: 1; tr: 1; td: 5>

<td class="text-center" data-timestamp="1610919900">
 2021-01-17 21:45
</td>


[*] Analyzing <pg: 1; tr: 1; td: 6>

<td class="text-center">
 200
</td>


[*] Analyzing <pg: 1; tr: 1; td: 7>

<td class="text-center">
 38
</td>


[*] Analyzing <pg: 1; tr: 1; td: 8>

<td class="text-center">
 7661
</td>


[*] Scraping: https://nyaa.si/?f=0&c=0_0&q=one+punch+man&s=seeders&o=desc&p=2
[*] Analyzing <pg: 2; tr: 1; td: 1>

<td>
 <a href="/?c=1_2" title="Anime - English-translated">
  <img alt="Anime - English-translated" class="category-icon" src="/static/img/icons/nyaa/1_2.png"/>
 </a>
</td>


[*] Analyzing <pg: 2; tr: 1; td: 2>

<td colspan="2">
 <a href="/view/1111733" title="[Erai-raws] One Punch Man - 01 ~ 12 [1080p][Multiple Subtitle]">
  [Erai-raws] One Punch Man - 01 ~ 12 [1080p][Multiple Subtitle]
 </a>
</td>


[*] Analyzing <pg: 2; tr: 1; td: 3>

<td class="text-center">
 <a href="/download/1111733.torrent">
  <i class="fa fa-fw fa-download">
  </i>
 </a>
 <a href="magnet:?xt=urn:btih:1b4894accb757b18b30959ac86bbd08303008f61&amp;dn=%5BErai-raws%5D%20One%20Punch%20Man%20-%2001%20~%2012%20%5B1080p%5D%5BMultiple%20Subtitle%5D&amp;tr=http%3A%2F%2Fnyaa.tracker.wf%3A7777%2Fannounce&amp;tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&amp;tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&amp;tr=udp%3A%2F%2Fexodus.desync.com%3A6969%2Fannounce&amp;tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce">
  <i class="fa fa-fw fa-magnet">
  </i>
 </a>
</td>


[*] Analyzing <pg: 2; tr: 1; td: 4>

<td class="text-center">
 9.5 GiB
</td>


[*] Analyzing <pg: 2; tr: 1; td: 5>

<td class="text-center" data-timestamp="1548004730">
 2019-01-20 17:18
</td>


[*] Analyzing <pg: 2; tr: 1; td: 6>

<td class="text-center">
 7
</td>


[*] Analyzing <pg: 2; tr: 1; td: 7>

<td class="text-center">
 1
</td>


[*] Analyzing <pg: 2; tr: 1; td: 8>

<td class="text-center">
 2354
</td>
```

## Example III: A More Thorough Example Of Pagination

In this example we will organize our code by making use of object orientation. We will bifurcate the functionality of our code into two classes. One that handles networking via requests, and the other which handles the extraction and output via BeautifulSoup. The latter will inherit from the former. 

# The Nest Class (Networking)

This class will be handling all networking functionality.

```py
class Nest:
    def __init__(self, url):
        self.__url  = url
        self.__page = 1
```

```py
@property
def url(self):
    return self.__url

@url.setter
def url(self, value):
    if not isinstance(value, str):
        raise TypeError("Must be a string!")
    
    self.__url = value

@property
def page(self):
    return self.__page

@page.setter
def page(self, value):
    if not isinstance(value, int):
        raise TypeError("Must be an integer!")
    
    self.__page = value
```

```py
@classmethod
def nyaa(cls, link=None):
    base = "https://nyaa.si"

    if not link:
        link = "/?" + urlencode({
            'f': '0',             # Filter
            'c': '0_0',           # Category
            'q': 'one punch man', # Query
            's': 'seeders',       # Sort by
            'o': 'desc'           # Order
        }, doseq = True, quote_via = quote_plus)

    else:
        if not isinstance(link, str):
            raise TypeError("Must be a string!")

    return cls(base+link)
```

```py
def fetch(self, url=None):
    if url is not None: # if url:
        self.url = url

    responseObject = requests.get(self.url)

    if not responseObject.ok:
        raise requests.HTTPError(responseObject.url)
    
    soup = BeautifulSoup(responseObject.text, 'html.parser')
    
    return soup, responseObject
```

# The Spider Class (Extraction)

```py
class Spider(Nest):
    def __init__(self, *args):
        super().__init__(*args)
        columns      = ['category', 'name', 'magnet', 'size', 'date', 'seeds', 'leeches', 'downloads']
        self.column  = dict(zip( range(1, len(columns)+1), columns ))
```
```py
# Handle Table Data (Innermost)
def __handle_data(self, row):
    for index, data in enumerate(row.find_all('td'), start=1):
        if   index == 1:
            yield self.column[index], data.a.get('title'),
        elif index == 2: 
            category = self.column[index]
            name     = data.find_all('a')[-1].text
            
            try:
                comments = data.find('a', {'class':'comments'}).text
            except:
                comments = '0'

            nameAscii = ''.join([i for i in name.strip() if i.isascii()])
            yield category.strip(), nameAscii
            yield "comments", comments.strip()
        elif index == 3: 
            yield self.column[index], data.find_all('a')[-1].get('href')
        else:
            yield self.column[index], data.text
```

```py
{'category': 'Anime - English-translated',
 'comments': '22',
 'date': '2021-01-17 21:45',
 'downloads': '7654',
 'leeches': '27',
 'magnet': 'magnet:?xt=urn:btih:31e236514cbba971c479b47bd09581ff56b8a93c&dn=%5BJudas%5D%20One%20Punch%20Man%20%28Seasons%201-2%20%2B%20OVAs%20%2B%20Specials%29%20%5BBD%201080p%5D%5BHEVC%20x265%2010bit%5D%5BDual-Audio%5D%5BMulti-Subs%5D%20%28Batch%29&tr=http%3A%2F%2Fnyaa.tracker.wf%3A7777%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fexodus.desync.com%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce',
 'name': '[Judas] One Punch Man (Seasons 1-2 + OVAs + Specials) [BD '
         '1080p][HEVC x265 10bit][Dual-Audio][Multi-Subs] (Batch)',
 'seeds': '176',
 'size': '11.2 GiB'}
```

```py
# Handle Table Rows (Outer)
def __handle_rows(self, rows):
    for r, row in enumerate(rows, start=1):
        yield r, dict(self.__handle_data(row))
```

```py
{1: {'category': 'Anime - English-translated',
     'comments': '22',
     'date': '2021-01-17 21:45',
     'downloads': '7654',
     'leeches': '27',
     'magnet': 'magnet:?xt=urn:btih:31e236514cbba971c479b47bd09581ff56b8a93c&dn=%5BJudas%5D%20One%20Punch%20Man%20%28Seasons%201-2%20%2B%20OVAs%20%2B%20Specials%29%20%5BBD%201080p%5D%5BHEVC%20x265%2010bit%5D%5BDual-Audio%5D%5BMulti-Subs%5D%20%28Batch%29&tr=http%3A%2F%2Fnyaa.tracker.wf%3A7777%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fexodus.desync.com%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce',
     'name': '[Judas] One Punch Man (Seasons 1-2 + OVAs + Specials) [BD '
             '1080p][HEVC x265 10bit][Dual-Audio][Multi-Subs] (Batch)',
     'seeds': '176',
     'size': '11.2 GiB'}}
```

```py
# Handle Page Data (Outmost)
def __handle_pages(self, max_pages):
    base   = self.url[:15]
    link   = None
    data   = {}

    # Mainloop ("Spiderweb")
    while True:

        # Retrieve objects
        soup, res = self.fetch(base+link) if link else self.fetch()
        print(f"[*] Scraping: {res.url}")

        # Process soup data
        rows = soup.table.tbody.find_all('tr')
        data[self.page] = dict(self.__handle_rows(rows))

        # Find link for next page
        next_ = soup.find('li', {"class": "next"})
        link = next_.a.get('href')

        # Handle last page
        if not link:
            print(f"[-] last page reached ...")
            break

        # Increase page counter
        self.page += 1
        
        # Only first n pages
        if max_pages:
            if self.page > max_pages:
                break

    return data
```

We can examine the **`__handle_pages()`** return data by extracting only the first row from each page, we can then take a closer look at the data structure. The structure is a dictionary of pages, associated to another dictionary of rows which contains an even more deeply nested dictionary of table data.

```py
{1: {1: {'category': 'Anime - English-translated',
         'comments': '22',
         'date': '2021-01-17 21:45',
         'downloads': '7654',
         'leeches': '26',
         'magnet': 'magnet:?xt=urn:btih:31e236514cbba971c479b47bd09581ff56b8a93c&dn=%5BJudas%5D%20One%20Punch%20Man%20%28Seasons%201-2%20%2B%20OVAs%20%2B%20Specials%29%20%5BBD%201080p%5D%5BHEVC%20x265%2010bit%5D%5BDual-Audio%5D%5BMulti-Subs%5D%20%28Batch%29&tr=http%3A%2F%2Fnyaa.tracker.wf%3A7777%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fexodus.desync.com%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce',
         'name': '[Judas] One Punch Man (Seasons 1-2 + OVAs + Specials) [BD '
                 '1080p][HEVC x265 10bit][Dual-Audio][Multi-Subs] (Batch)',
         'seeds': '156',
         'size': '11.2 GiB'}},
 2: {1: {'category': 'Anime - English-translated',
         'comments': '0',
         'date': '2019-07-03 01:04',
         'downloads': '10218',
         'leeches': '0',
         'magnet': 'magnet:?xt=urn:btih:b5d1c8d3e1a63055b6f3c126ff798161fb0953e8&dn=One%20Punch%20Man%20S02E12%20%281080p%20WEBRip%20x265%20HEVC%2010bit%20AAC%202.0%20theincognito%29%20%5BUTR%5D&tr=http%3A%2F%2Fnyaa.tracker.wf%3A7777%2Fannounce&tr=udp%3A%2F%2Fopen.stealth.si%3A80%2Fannounce&tr=udp%3A%2F%2Ftracker.opentrackr.org%3A1337%2Fannounce&tr=udp%3A%2F%2Fexodus.desync.com%3A6969%2Fannounce&tr=udp%3A%2F%2Ftracker.torrent.eu.org%3A451%2Fannounce',
         'name': 'One Punch Man S02E12 (1080p WEBRip x265 HEVC 10bit AAC 2.0 '
                 'theincognito) [UTR]',
         'seeds': '8',
         'size': '330.8 MiB'}}}
```

```py
def crawl(self, max_pages=None):

    data = self.__handle_pages(max_pages)

    # Combine each pages rows into one DataFrame
    def __format():
        counter = 1
        for page, rows in data.items():
            for row, td in rows.items():
                yield counter, td
                counter += 1

    df = pd.DataFrame.from_dict(data=dict(__format()), orient='index')

    return df
```

# The Main Program

```py
import requests

from bs4          import BeautifulSoup
from urllib.parse import urlencode, quote_plus

import pandas as pd

def main():
    # Set url via @classmethod
    spider = Spider.nyaa()

    # Crawl 2 pages
    df = spider.crawl(2)

    # Display neatly
    print(
        df.to_string(
            justify         = 'left', 
            max_rows        = 16, 
            max_colwidth    = 32, 
            show_dimensions = True
        )
    )

if __name__ == "__main__":
    main()
```

# The Output

```
[*] Scraping: https://nyaa.si/?f=0&c=0_0&q=one+punch+man&s=seeders&o=desc
[*] Scraping: https://nyaa.si/?f=0&c=0_0&q=one+punch+man&s=seeders&o=desc&p=2
    category                         name                             comments magnet                           size       date              seeds leeches downloads
1         Anime - English-translated  [Judas] One Punch Man (Seaso...  22       magnet:?xt=urn:btih:31e23651...   11.2 GiB  2021-01-17 21:45  162   28       7654
2         Anime - English-translated  [HorribleSubs] One Punch Man...  23       magnet:?xt=urn:btih:b9dd7539...  909.7 MiB  2019-05-28 22:38   45    0      80178
3         Anime - English-translated  [HorribleSubs] One Punch Man...  39       magnet:?xt=urn:btih:f29fda82...  852.6 MiB  2019-05-21 17:43   44    0      84523
4         Anime - English-translated  [HorribleSubs] One Punch Man...  21       magnet:?xt=urn:btih:f2da25e0...  769.1 MiB  2019-06-18 17:40   44    0      80148
5         Anime - English-translated  [HorribleSubs] One Punch Man...  22       magnet:?xt=urn:btih:610f81ab...    1.0 GiB  2019-06-25 17:47   43    0      78400
6         Anime - English-translated  [HorribleSubs] One Punch Man...  45       magnet:?xt=urn:btih:7498c4c8...  835.8 MiB  2019-04-30 17:40   43    0      84956
7         Anime - English-translated  [HorribleSubs] One Punch Man...  49       magnet:?xt=urn:btih:6959d8ee...  803.3 MiB  2019-04-09 18:36   42    1      84984
8         Anime - English-translated  [HorribleSubs] One Punch Man...  23       magnet:?xt=urn:btih:e5c44615...  793.8 MiB  2019-04-16 17:44   42    0      82664
..                               ...                              ...      ...                              ...        ...               ...   ...     ...       ...
143       Anime - English-translated  [Erai-raws] One Punch Man (2...   0       magnet:?xt=urn:btih:7cc5a563...    1.2 GiB  2019-04-18 19:43    3    0       4144
144       Anime - English-translated  One.Punch.Man.S02E12.Cleanin...   0       magnet:?xt=urn:btih:e49a8342...  519.8 MiB  2019-07-02 18:18    3    0       5673
145       Anime - English-translated  [LostYears] One Punch Man - ...   1       magnet:?xt=urn:btih:5e4dfc14...    1.5 GiB  2019-11-03 22:15    3    0       1205
146       Anime - English-translated  [SSA] One Punch Man (Season ...   2       magnet:?xt=urn:btih:55fae8bc...    1.8 GiB  2021-05-18 05:32    3    0        270
147       Anime - English-translated  One.Punch.Man.S02E07.The.Cla...   1       magnet:?xt=urn:btih:a412fba6...  354.8 MiB  2019-05-21 18:07    3    0      10503
148       Anime - English-translated  [HorribleRips] One-Punch Man...   3       magnet:?xt=urn:btih:d49fde98...   10.0 GiB  2020-10-23 22:25    3    1        665
149       Anime - English-translated  [Erai-raws] One Punch Man (2...   1       magnet:?xt=urn:btih:2b8939f9...  361.8 MiB  2019-06-18 18:15    3    0       7067
150  Literature - English-translated  [Pajeet] One Punch Man (Orig...   7       magnet:?xt=urn:btih:1221db3b...  417.8 MiB  2020-04-13 10:56    3    1        669

[150 rows x 9 columns]
```

<!-- ## **Presentation Of Data** -->

<!-- ## Formatting Data To EXCEL

```py
with pd.ExcelWriter('example.xlsx') as writer:
    df.to_excel(writer)
``` -->