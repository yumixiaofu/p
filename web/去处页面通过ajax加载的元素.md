# 代码部分
```

jQuery.fn.wait = function (selector, func, times, interval) {
    var _times = times || -1, //100次
    _interval = interval || 20, //20毫秒每次 
    _self = this,
    _selector = selector, //选择器
    _iIntervalID; //定时器id
    if( this.length ){ //如果已经获取到了，就直接执行函数
        func && func.call(this);
    } else {
        _iIntervalID = setInterval(function() {
            if(!_times) { //是0就退出
                clearInterval(_iIntervalID);
            }
            _times <= 0 || _times--; //如果是正数就 --
            
            _self = $(_selector); //再次选择
            if( _self.length ) { //判断是否取到
                func && func.call(_self);
                clearInterval(_iIntervalID);
            }
        }, _interval);
    }
    return this;
}

$(选择器.wait(选择器, function() {
   选择器.remove();
});


```
# 解释说明
在遇到页面元素动态加载的时候只能用这个方法来解决！
