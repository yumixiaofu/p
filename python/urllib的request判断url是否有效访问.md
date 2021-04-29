# urllib.request判断url是否有效或者可以访问
```

# -!- coding: UTF-8 -!-
import urllib.request
def jc(url):
    opener = urllib.request.build_opener()
    opener.addheaders = [('User-agent', 'Mozilla/49.0.2')]
    try:
        opener.open(url)
        print(url+"正常访问！")
    except Exception as error:
        print(error)
        print(url + '=访问页面出错')

```
