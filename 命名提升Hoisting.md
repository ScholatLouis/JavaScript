#命名提升Hoisting
  在这一部分需要说明清楚的有两个内容，一个是JavaScript的作用域问题，另一个就是变量和函数声明提升hoisting问题。

###JavaScript作用域
  在之前学习的语言比如C语言，都是块级作用域，如：

```C
#include <stdio.h>
int main() {
	int x = 1;
	printf("%d, ", x);		//print: 1
	if (1) {
		int x = 2;
		printf("%d, ", x);	//print: 2
	}
	printf("%d\n", x);		//print: 1
}
```
> 上面的代码中一次打印出来的是1，2，1。这个很容易理解，因为一个块就是一个新的作用域，能够屏蔽外面相同名称的变量，即块级作用域(block-level scope)。而在JavaScript中，并不是块级作用域，而是函数级作用域(function-level scope)，块并没有起到C语言中块的隔离变量作用。  

```JavaScript
  var x = 1;
  console.log(x); 		//print: 1
  if (true) {
	var x = 2;
	console.log(x); 	//print: 2
  }
  console.log(x); 		//print: 2
```
> 可以看到在上面的代码中，if块声明的变量x在块外面还能访问到。  
> 知道了JavaScript中是使用函数级作用域之后，对于需要一个临时的新的作用域，可以通过如下的方式来实现：

```JavaScript
  function foo(){
  	var x = 1;

  	(function(){
  	  var x = 2;
  	  //其他处理代码
  	}());

  	//此处x的值仍然为1
  }
```

###命名提升Hoisting
  对于命名提升的理解，先看看下来的代码
```JavaScript
  var foo = 1;
  function bar() {
	if (!foo) {
		var foo = 10;
	}
	console.log(foo);
  }
  bar();				//print: ???

  var a = 1;
  function b() {
  	console.log(a);
  	a = 10;
  	return;
  	function a() {}
  }
  b();					//print: ???
  console.log(a);		//print: ???
``` 

> 1 第一个打印的值是10；第二个打印的是function a(){}；第三个打印的是1。分析放在文章的末尾，先来说说命名提升。

  对于编写后的JavaScript，在执行之前会经过解释器。解释器会将变量声明和函数声明做一个操作，那就是Hoisting。如对于下面的代码：
```JavaScript
  function foo(){
    bar();
	var x = 1;
}
```
  在执行之前，经过解释器解释成：
```JavaScript
  function foo(){
	var x;
	bar();
	x = 1;
  }
```
  可以看到对于变量x，经过解释器解释之后提升到了当前作用域的顶部，即函数foo的顶部。需要注意的是，对于变量的提升只有声明部分，对于赋值部分则没有做提升操作。  
```JavaScript
  //原来的函数
  function foo(){
	if(false){
		var x = 1;
	}
	return ;
	var y = 1;
  }
  //解释之后的函数
  function foo(){
	var x, y;
	if(false){
		x = 1;
	}
	return ;
	y = 1;
  }
```

  对于函数而言，首先函数有两种声明方式，分别是函数命名和函数表达式。这两种方式的处理方式是不同的，主要区别就是是否对函数体进行提升。
> 1) var functionOne = function(){}，对于这种函数表达式的和变量声明是一致的，只提升变量名即functionOne，函数体是不做提升操作。  
> 2) function functionTwo(){}，对于函数命名方式则会将函数名和函数体一起做提升操作。  

```JavaScript
  function test(){
	foo();
	bar();
	var foo = function(){
		alert("foo function");
	}
	function bar(){
		alert("function bar");
	}
  }
  test();
```
  提升之后的函数如下：  

```JavaScript
  function test(){
	foo();	//TypeError--> foo is not a function
	bar();  //function bar
	var foo = function(){	
		//指示将变量名foo进行提升，浏览器并不能知道foo变量是一个指向函数的引用，因此不能使用foo()执行函数。
		console.log("foo function");
	}
	function bar(){		
		//函数名和函数体都提升到当前作用域的顶部，因此能执行函数bar()
		console.log("function bar");
	}
  }
```
  文章开头的例子解析： 
```JavaScript
  //原来的函数
  var foo = 1;
  function bar() {
	if (!foo) {
		var foo = 10;
	}
	console.log(foo);
  }
  bar();
  
  //JavaScript解释器执行结果
  var foo = 1;
  function bar(){
  	var foo;				//此时foo的值为undefined
  	if(!foo){ 				//!undefined的结果为true,进入if语句执行foo = 10
  		foo = 10;
  	}
  	console.log(foo);		
  }
  bar() 					//控制台打印： 10
```

```JavaScript
  var a = 1;
  function b() {
	console.log(a);
	a = 10;
	return;
	function a() {}
  }
  b();
  console.log(a);
 
  //JavaScript解释器结果
  var a = 1;
  function b(){
  	function a(){}
  	console.log(a);	//注意此处弹出的是function a(){}而不是function a()，因此可以证明此处的函数提升是包括函数体
  	a = 10;
  	return;
  	function a(){}
  }
  b();
  console.log(a);	//最后这里弹出的a为1，因此函数b的作用域在此处已经结束，引用的a为全局定义的a。
```

#参考文章
> [1]http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html
