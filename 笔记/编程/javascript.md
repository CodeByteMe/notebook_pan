# 常用方法
```javascript
document.scripts                // 获取当前DOM已有`<script>`标签对象
```

# 一个漂亮的下载单页
样例  
![图片](../images/e17fbd.png)  
源码  

```html
<!DOCTYPE html>
<html lang="zh">
<head>
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1" />
	<title>XX 单页面</title>
	<link rel="apple-touch-icon" href="https://cdn.u1.huluxia.com/g3/M02/19/F5/wKgBOV23guqAIvy8AAAlSkLYaV8350.png">
</head>
<body style="line-height: 1.9;background-image:url(https://ae04.alicdn.com/kf/U754455a2964d4bc2b409d068186665cfy.jpeg);">
	<div style="padding-top:10%;text-align:center;">
		<p>
			<img src="https://cdn.u1.huluxia.com/g3/M02/19/F5/wKgBOV23guqAIvy8AAAlSkLYaV8350.png" width="100">
		</p>
		<h1 style="color:#fff;">
			<strong>名称</strong>
		</h1>
		<p style="font-size:3vh;color:#fff;">
			<b>名称</b>是一个牛 X 的网址 <br>真牛
		</p>
		<p>
			<a style="border-radius:10px;padding:10px 38px;color:#fff;border-style:solid;text-decoration:none;" href="https://cdn.mom1.cn/wordpress/wordpress.zip">立即下载</a>
		<p>
			<span style="color: #bd8787;">最新版本 v1.1.1</span>
			<br>
			<span style="color:#ff0202;background-color:#9734344d;">MD5 值：E10ADC3949BA59ABBE56E057F20F883E</span>
		</p>
		<p style="color:#fff;">你的地址:
		<script src="https://pv.sohu.com/cityjson?ie=utf-8"></script>
		<script>document.write(returnCitySN["cname"])</script>  
		© 
		<a style="text-decoration:none;color:#fff;" href="#">XX</a>.版权所有
		</p>
		<script type="text/javascript" color="255,255,255" opacity='0.7' zIndex="-2" count="200" src="https://cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
	</div>
</body>
</html>
```

[转载](https://blog.kieng.cn/1798.html)

# JavaScript数据存储
两种方式(都`只能存储字符串`)
* `localStorage`:不清除浏览器,则会一直存储在浏览器.持久化存储
* `sessionStorage`:本地会话存储,关闭页面或刷新都会导致失效

存储
```javascript
localStorage.name = 'vanida';
localStorage["name"]='vanida'； 
localStorage.setItem("name", 'vanida'); 
```
获取
```javascript
var name = localStorage.name;
var name = localStorage["name"]； 
var name = localStorage.getItem("name");
```
清除
```javascript
localStorage.removeItem("name");
localStorage.name='';
```
清除所有值
```javascript
localStorage.clear() 
```
> 因为只能存字符串,如果有`JSON`数据  
> 存的时候使用`localStorage.setItem("name", JSON.stringify(jsonData))`  
> 取的时候使用`JSON.parse(localStorage.getItem("name"))`

[参考](https://www.cnblogs.com/wuweb/p/7846752.html)
