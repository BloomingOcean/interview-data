## 一、有哪几种设置跨域

1、jsonp

这是一种前端的技术。方法是利用<script scr=''xxx'>这个标签（这个标签的网络请求是天然跨域的）

（如果了解同源策略，可以讲）与Ajax相比，Ajax属于同源策略，JSONP属于非同源策略（可以跨域）

```javascript
// 实现方式
<script type="text/javascript">
    // 回调函数
    function fn(data){
        alert(data.msg);
    }
</script>
// 在最后有个jsonp的参数，参数值就是请求后的回调函数
<script type="text/javascript" src="http://crossdomain.com/jsonServerResponse?jsonp=fn">
```

弊端：只支持GET请求方式，并且是异步请求

2、在HTTP请求的头部加上两个属性（这个判断是否能跨域是浏览器判断的，通过查看HTTP请求的这两个属性）

```javascript
// 允许跨域的地址（这个地址指的是网站的地址，不是服务器的地址）
header("Access-Control-Allow-Origin:*");
// 允许跨域的方法
header("Access-Control-Allow-Methods:POST,GET");
```

3、WebSocket

WebSocket是一种HTML5的一个持久化协议，实现了浏览器和服务器的全双工通信（可以互相通信）。

所以可以利用WebSocket和服务端建立连接，然后直接进行通信

4、postMessage（我感觉可以不用了解，过于冷门）

二、

