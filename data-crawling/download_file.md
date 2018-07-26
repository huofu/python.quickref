# Download File from URL

## Quick One
```python
import requests

url = 'http://google.com/favicon.ico'
r = requests.get(url, allow_redirects=True)
open('google.ico', 'wb').write(r.content)
```
## Check Download-able
```python
import requests

def is_downloadable(url):
    """
    Does the url contain a downloadable resource
    """
    h = requests.head(url, allow_redirects=True)
    header = h.headers
    content_type = header.get('content-type')
    if 'text' in content_type.lower():
        return False
    if 'html' in content_type.lower():
        return False
    return True

print is_downloadable('https://www.youtube.com/watch?v=9bZkp7q19f0')
# >> False
print is_downloadable('http://google.com/favicon.ico')
# >> True
```

## Check and Limit File Size 
```python
h = requests.head(url, allow_redirects=True)
header = h.headers
content_length = header.get('content-length', None)
if content_length and content_length > 2e8:  # 200 mb approx
  return False
```

## Get Filename from URL 
```python
url = 'http://aviaryan.in/images/profile.png'
if url.find('/'):
  print url.rsplit('/', 1)[1]

```
```python
import requests
import re

def get_filename_from_cd(cd):
    """
    Get filename from content-disposition
    """
    if not cd:
        return None
    fname = re.findall('filename=(.+)', cd)
    if len(fname) == 0:
        return None
    return fname[0]


url = 'http://google.com/favicon.ico'
r = requests.get(url, allow_redirects=True)
filename = get_filename_from_cd(r.headers.get('content-disposition'))
open(filename, 'wb').write(r.content)
```

## Reference
- [Downloading Files from URLs in Python](https://www.codementor.io/aviaryan/downloading-files-from-urls-in-python-77q3bs0un)
