#[1]DOM Level 0 与 DOM Level 2事件
1. DOM Level 0事件
 * 一开始浏览器处理事件的方式是将事件处理程序被设置为js代码串，切作为html的属性值（DOM Level 0的事件直接绑定到HTML代码中），如：
	HTML
	<span id="test" onclick="alert('event inline');">Click me!</span>
 * DOM Level 0对某一元素不能同时绑定同一事件，因为后面的事件会覆盖前面同样的事件绑定
	```HTML
	<span id="test" onclick="alert('event inline');">Click me!</span>
	```
	
	```JavaScript
	var test = document.getElementById("test");
	test.onclick = function(){
		alert("event first");
	};

	test.onclick = function(){
		alert("event second");
	}
	```
	[Example点击查看](http://jsfiddle.net/Louis_Tsang/8sb5cys2/ "Example")
  最后弹框出现的只有event second，event first绑定的事件被覆盖，inline的事件也被覆盖
 * DOM Level 0绑定事件的两种方法如上：  
  1.将事件作为html的属性直接写在html代码中。  
  2.获取元素，在通过元素的.onEvent = function(){}进行绑定。  

2. DOM Level 2事件
 * DOM Level 2规定了事件的传播方式，主要有三个阶段，分别是捕获Capturing Phase、目标Target Phase和冒泡Bubble Phase。这个知识点详细内容请看目录[2][事件传播与冒泡]()
 * DOM Level 2的事件则是通过addEventListener(IE浏览器使用attachEvent)函数对元素设置监听函数。  
    + addEventListener(String, function(event), Boolean)函数介绍  
      1. 第一个参数String，指的是事件类型。如绑定点击事件则是click，需要注意的是没有on前缀，不是onclick  
      2. 第二个参数是事件处理函数，JS在调用的时候会传入事件event，关于事件event的详细内容请看目录[3][Event对象]()  
      3. 第三个参数是一个boolean值，取true或者false。当为true的时候则表示在事件Capturing Phase阶段执行处理函数；为false的时候则表示在事件Bubble Phase阶段执行处理函数。  
 * 通过DOM Level 2事件绑定，同个元素可以绑定多个相同事件，各个事件根据绑定的顺序依次执行。
  ```HTML
  <span id="test" onclick="alert('event inline');">Click me!</span>
  ```
  ```JavaScript
  var test = document.getElementById("test");
  test.addEventListener("click", function(){
  	alert("event first");
  });
    
  test.addEventListener("click", function(){
  	alert("event second");
  });
  ```
	[Example点击查看](http://jsfiddle.net/Louis_Tsang/t2LsqmLx/ "Example")
3. DOM Level 0 与 DOM Level 2的比较  
  1. 事件的绑定方式不同  
  2. DOM Level 2将事件分为三个阶段，而DOM Level 0则没有，默认事件在相当于DOM Level 2事件Bubble Phase处理(即如果用0级DOM的2个方法赋值的事件监听函数不能在capturing阶段捕捉到事件)。  
  3. DOM Level 0事件浏览器支持比较统一，而如果使用DOM Level 2则在IE和非IE情况下需要做区分。具体的处理方法请看目录[5][跨浏览器中事件]() 
