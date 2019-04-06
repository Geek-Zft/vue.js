* 由于浏览器的安全性限制，不允许AJAX访问 协议不同，域名不同，端口号不同的数据接口，浏览器觉得这不安全
* 可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式称作JSONP（根据原理可知，jsonp只支持get请求）

## 具体实现过程

* 现在客户端定义一个回调方法，预定义对数据的操作
* 再把这个回调方法的名称，通过URL传参的形式，提交到服务器的数据接口
* 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去执行
* 客户端拿到服务器返回的字符串之后，当做script脚本去解析执行，这样就能够拿到JSONP的数据了

伪代码

前端

```js
<script>
showInfo(data) {
	//回调方法，做一些处理
}
</script>


<script src="http://localhost:3000/getscript/?callback=showInfo"></script>
```

后端

```java
@RequestMapping(/getscript)
public String jsonp() {
	
	String callback = request.getParameter("callback"); //showInfo
	User user = new User();
	user.setName = "Geek-Z";
	user.setAge = 20;
	String json = JSON.toJSONString(user);
	return callback + "(" + json + ")";
	
}
```