### 克隆DOM节点  
在[浏览器的渲染过程](https://github.com/ScholatLouis/JavaScript/blob/master/浏览器的渲染过程.md)一文中，有提到离线化处理DOM节点，其中的一种方法就是将DOM节点进行克隆，然后在重新替换。这篇文章主要是说明原生JavaScript和jQuery库下克隆节点的做法。  
##### 原生JavaScript
原生JavaScript提供了一个方法克隆节点，cloneNode(boolean)。接受的参数boolean值表示是否同时克隆子节点，默认是false，即不克隆子节点。  
```HTML
	<div id="divContent">
		<ul>
			<li>First</li>
			<li>Second</li>
			<li>Third</li>
			<li>Forth</li>
			<li>Fifth</li>
		</ul>		
	</div>
```

```JavaScript
	var cloneDivFir = document.getElementById("divContent").cloneNode();
	var cloneDivSec = document.getElementById("divContent").cloneNode(true);
```

> 需要注意的是，克隆一个DOM节点，会拷贝该节点行的所有属性，但是会丧失addEventListener方法和on-属性(即node.onclick = function())，添加在这个节点上的事件回调函数。  

##### jQuery库 
jQuery库同样提供了一个方法用于克隆DOM节点。clone()函数用于克隆当前匹配元素集合的一个副本，并以jQuery对象的形式返回。语法如下：  
jQueryObject.clone(withDataAndEvents [, deepWithDataAndEvents]);  
> 1 withDataAndEvents：是一个boolean值，用于指明是否同时复制元素的附加数据和绑定事件，默认为false  
> 2 deepWithDataAndEvents：是一个boolean值，用于指明是否同时复制元算的所有子元素的附加数据和绑定事件，默认值即为参数withDataAndEvents的值  
> 返回值是一个jQuery对象  
> 需要注意的是jQuery的clone方法默认会克隆节点的所有子节点  

```HTML
	<p id="paragraph">
		<input id="firstButton" type="button" value="FirstButton" />
		<input id="secondButton" type="button" value="SecondButton" />
	</p>
```

```JavaScript
	$("input[value$=Button]").click(function(){
		console.log(this.value);
	});
	var $temp1 = $("#FirstButton");
	$temp1.data("myName", "Louis");
	//没有克隆附加的数据和绑定事件
	var $cloneButton1 = $temp1.clone();
	console.log($cloneButton1.data("myName"));	//-->print: undefined
	$cloneButton1.trigger("click");	//-->没有绑定事件，无响应输出
	//同时克隆附加的数据和绑定事件
	var $cloneButton2 = $temp1.clone(true);
	console.log($cloneButton2.data("myName"));	//-->print: Louis
	$cloneButton2.trigger("click");	//-->触发事件发生，print: FirstButton
```
