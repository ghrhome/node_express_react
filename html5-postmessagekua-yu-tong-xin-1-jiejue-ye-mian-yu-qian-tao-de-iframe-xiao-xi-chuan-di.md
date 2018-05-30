# html5 postMessage跨域通信 1.解决页面与嵌套的iframe消息传递

postMessage\(data,origin\)

1、父页面获取iframe的数据  
第一步：iframe向父页面发送数据

```
var  idata="abc";
// *代表向所有父级发送，或者可以指定地址
window.parent.postMessage(idata,"*");

```

第二步：父页面接收iframe的数据

```
window.addEventListener('message',function(rs){ 
         console.log(rs.data);
})
```

2、iframe获取父页面数据

第一步：父页面向iframe发送数据

```
<iframe src="tpl/map.html" name="myFrame" width="100%" height="600px" scrolling="no" id="editmap"></iframe>
//获取iframe的id 
var ifr = document.getElementById('editmap');
//iframe的路径
var url=document.location.origin+'/admin/tpl/maptxt.html';
//要发送的数据
var fdata=[$scope.tablesData[0].long,$scope.tablesData[0].lat];
//页面加载完后，执行发送方法
setTimeout(function(){
    ifr.contentWindow.postMessage(fdata,url);
},2000);
```

第二步：iframe接收数据

```
window.addEventListener('message',function(rs){
    var pointdata=rs.data;
    console.log(pointdata);     
})
```















