

```
function  IsLoad(_url,fun){
   $.ajax({
       url:_url,
       type: "get" ,
       success: function (){
         //说明请求的url存在，并且可以访问
         if ($.isFunction(fun)){
                 fun( true );
               }
       },
       statusCode:{
         404: function (){
           //说明请求的url不存在
           if ($.isFunction(fun)){
             fun( false );
           }
         }
       }
     });
}
//调用
IsLoad( 'www.baidu.com' , function (res){
     if (res){
       alert( '请求的url可以访问' );
     }
});
————————————————

参考地址：https://blog.csdn.net/milijiangjun/article/details/78496634

```
