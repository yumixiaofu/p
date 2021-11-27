//jquery检查连接的url服务是否有效,适用所有浏览器
const $ = require('jquery')
$(document).ready(function () {
  // 执行代码
  function NetPing(pingUrl) {
    $.ajax({
      type: "GET",
      cache: false,
      url: pingUrl,
      data: "",
      success: function () {
        alert("网址有效，开始加载。。");
        ipcRenderer.send('asynchronous-message',pingUrl); //异步将渲染进程的数据传给主进程
      },
      error: function () {
        alert("无效网址，请重新输入!");
        $("#vueUrl").val("");
      }
    });
  }
 
  //点击连接vue前端服务
  $("#linkBtn").click(function(){
    let urlVal = $("#vueUrl").val();
    NetPing(urlVal);
  })
});
