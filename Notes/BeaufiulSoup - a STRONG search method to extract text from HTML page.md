I got stuck for writing HTML parser for a while until I found a search method beautifulsoup. Oh my goodness, it is awesome! It should be renamed as beautifullife since it makes life easier.

I wrote a python class as to show how to use BeautifulSoup to exact needed data, my blog is used as an example.

```python
# This file is created by Danxue(Dannie) Huang, as an example of using BeautifulSoup
# The goal of this example is to get specified data from a website
# I use my blog as an example, let's say, I try to get the title of my blog

# First of all, we need to import urllib to read an URL
import urllib.request
# Second, we need to import BeautifulSouop from package bs4
from bs4 import BeautifulSoup

class example_bs():
    # Pass in an URL. I use my blog as an example here
    def parse_page(self, url):
        page_str = self._read_html_page(url)
        return BeautifulSoup(page_str, "html.parser")

    def _read_html_page(self, url, codec="utf-8"):
        url_response = urllib.request.urlopen(url)
        return url_response.read().decode(codec)

# Pass in an URL for test here, I use my blog as an example here
blog_url = "https://danxuehuang.blogspot.com/"
blog_data = example_bs()
blog = blog_data.parse_page(url = blog_url)
# Look at the source code of HTML, find out where the source code of data we need is locasted
blog_title = blog.find(name="div", attrs={"class": "titlewrapper"}).find(name="h1", attrs={"class": "title"})
# print the data we need
print(blog_title)
# print the plain text of data we need
print(blog_title.string)
```

Boom! The output is:
```python
<h1 class="title">
RTEMS Release Notes Generator &amp; RTEMS POSIX User Guide Generator
</h1>

RTEMS Release Notes Generator & RTEMS POSIX User Guide Generator


Process finished with exit code 0

```
