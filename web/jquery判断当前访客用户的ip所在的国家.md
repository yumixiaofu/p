
# 代码
```

$.get("https://ipinfo.io", function(response) {
    console.log(response.city, response.country);
}, "jsonp");

```

# 返回的json 说明
```
city: "Hong Kong"
country: "HK"
ip: "45.153.171.51"
loc: "22.2783,114.1747"
org: "AS141159 Incomparable(HK)Network Co., Limited"
readme: "https://ipinfo.io/missingauth"
region: "Central and Western"
timezone: "Asia/Hong_Kong"

```
具体可使用 alert(response.ip)测试。

# 说明
需要在最前面引入jquery库


# 参考地址

https://www.itranslater.com/qa/details/2325934030266565632
