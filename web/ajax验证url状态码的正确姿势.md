# 原始版
```
$.ajax({
type: 'get',
cache: false,
url: 'https://www.baidfdsfdsfsdfsdfsdfsdfsddu.com',
dataType: "jsonp", //跨域采用jsonp方式
processData: false,
//timeout:10000, //超时时间，毫秒
complete: function (data) {
if (data.status==200) {
alert(e);
} else {
alert("无效链接");
}

}
});
```


# 精简版通用的
```
$.ajax({
type: 'get',
cache: false,
url: 'https://www.baidfdsfdsfsdfsdfsdfsdfsddu.com',
dataType: "jsonp", //跨域采用jsonp方式
processData: false,
//timeout:10000, //超时时间，毫秒
complete: function (data) {
alert(data.status);

}
});


```
