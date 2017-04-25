### require学习笔记[1]
##### AMD与CommonJs
AMD和CommonJs都是模块化编程的规范，不同的是AMD规范主要用于浏览器，而CommonJs主要用于后台。
AMD是Asynchronous Module Definition的缩写，意思就是异步模块定义。它采用异步的方式加载模块，模块的加载不会影响到它后面语句的运行，也就不会出现网页假死的状态。require.js采用的就是AMD规范的异步加载库。

##### require.js
require.js的两个主要功能：
1）实现js的异步加载
2）管理库之间的依赖关系

我们先来看看平时脚本的加载：
```HTML
	<!DOCTYPE html>
	<html>
	    <head>
	        <script type="text/javascript" src="index.js"></script>
	    </head>
	    <body>
	      <div>halo Louis</div>
	    </body>
	</html>
```
  然后在index.js中加上如下的代码
```JavaScript
	(function(){
		alert("halo Louis");
	})()
```
> 可以看到，当浏览器弹出"halo Louis"的时候，页面还是处于空白的状态，没有显示<div>halo Louis</div>的内容。这是因为脚本的加载默认是阻塞加载状态，所以只有等脚本加载之后才会显示html页面的内容。

现在我们通过加入require库来改变脚本的加载
```HTML
	<!DOCTYPE html>
	<html>
	    <head>
	        <script type="text/javascript" src="require.js" data-main="main"></script>
	    </head>
	    <body>
	      <div>halo Louis</div>
	    </body>
	</html>
```
	下面是main.js的代码
```JavaScript
	require.config({
		//配置库的路径
		paths:{
			"jquery": [],
			"temp" : "temp"
		}
	});
	//配置库之间的依赖关系
	require(["jquery", "temp"], function($){
		console.log($().jquery);
	});
```
	在temp.js中的代码如下：
```JavaScript
	//定义模块
	define(function(){
		(function(){
			alert("halo Louis");
		})()
	})
```
> 通过引入require之后，可以看到在脚本加载的时候页面也有显示。这是因为require的引入使得脚本采用异步的方式进行加载，因此不会阻塞到浏览器对页面的渲染。
