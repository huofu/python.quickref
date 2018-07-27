# Retrieve PDFs from Google Search

## Decode Search Result Item
The item format depends on the actual result returned by Google.
```python
import re
from urlparse import urlparse

def decode_item(item):
    temp = item.find('h3', class_='r')
    link = temp.find('a').attrs['href']
    if re.search(r'http.*pdf', link) is not None:
        pdfurl = re.search(r'http.*pdf', link).group()
    else:
        pdfurl = re.search(r'www.*pdf', link).group
    pdfdomain = urlparse(pdfurl).netloc
    pdfname = temp.find('a').getText()
    return {'name': pdfname, 'url': pdfurl, 'host': pdfdomain}
```

## Set Request Cookies
This steps aims to avoid being identified as robot and blocked by Google.
```python
from selenium import webdriver
import requests

url_base = 'https://www.google.com.hk/search?q=%s&start=%s'
q, start = ('ico+filetype:pdf', 0)
driver = webdriver.Chrome()
driver.get(url_base % (q, start))

x = requests.Session()
for cookie in driver.getCookies():
    x.cookies.set(cookie['name'], cookie['value'])
```

## Request Google Search Result
Delay is added to avoid being blocked by Google
```python
import time
from bs4 import BeautifulSoup as BS

records = []
for start in range(0, 6000, 10):
    try:
        r = x.get(url_base % (q, start))
        page = BS(r.text, 'html.parser')
        for item in page.find_all('div', class_='g'):
            records.append(decode_item(item))
        time.sleep(0.5)
    except Exception, e:
        print '-------------------'
        print start, e        
        continue
```
