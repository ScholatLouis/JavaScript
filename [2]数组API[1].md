#[2]数组API[1]
####Array.prototype
 > Array.prototype属性代表了数组Array构造器的原型链。如果需要自行拓展数组函数，需要将函数指向Array.prototype链上。
 > 使用Array.isArray(Array.prototype)返回的结果为true

####Array.prototype.length
 > 获取数组的长度：arr.length；数组的最大长度为2的32次方，即Math.pow(2,32)

 ```JavaScript
  var temp = new Array(5);
  console.log(temp.length);		//打印：5
 ```
 > 常用于遍历数组的时候，提供退出条件
 
 ```JavaScript
  var numbers = [1,2,3,4,5];
  for(var i = 0; i < numbers.length; ++i){
  	numbers[i] *= 2;
  }
 ```
####Array.isArray()
 > 用于判断一个对象是否为数组，如果是则返回true，否则返回false。
 > 语法：Array.isArray(obj)

 ```JavaScript
  //以下都是返回true
  Array.isArray([]);
  Array.isArray(Array.prototype);
  Array.isArray(new Array());

  //以下都是返回false
  Array.isArray();
  Array.isArray({});
  Array.isArray(null);
  Array.isArray(undefined);
 ```
 > Array.isArray()只在IE9及其之上版本才支持，对于需要在IE9以下的版本，这需要使用以下兼容性代码：

 ```JavaScript
  if(!Array.isArray){
  	Array.isArray = function(arg){
  		return Object.prototype.toString.call(arg) === '[object Array]';
  	}
  }
 ```

###typeof VS. instanceof
1  typeof操作符

> 语法：typeof operand  
> 参数：operand是一个表达式，表示对象或原始值
> 常见的类型及其返回值，如下：

![常见类型及其返回值](http://123.56.156.116/Louis/typeof/typeofFirst.png)
```JavaScript
  //返回"number"
  typeof 37
  typeof 3.14
  typeof Math.LN2
  typeof Infinity
  typeof NaN			//NaN是"Not-A-Number"的缩写，意思是不是一个数字，其返回结果为："number"
  typeof Number(1)		//不建议这么使用

  //返回"string"
  typeof ""
  typeof "bla"
  typeof (typeof 1)
  typeof String("Louis")	//不建议这么使用

  //返回"boolean"
  typeof true
  typeof false
  typeof Boolean(true)		//不建议这么使用

  //返回"undefined"
  typeof undefined
  typeof blabla				//对于没有定义的变量，自然是Undefined类型；对于一个定义了，但是没有赋值的变量也为Undefined

  //返回"object"
  typeof {a: 1}
  typeof new Date()
  //下面的三种用法不建议，容易和上面的发生混淆
  typeof new String("Louis")
  typeof new Number(21)
  typeof new Boolean(true)

  //返回"function"
  typeof function(){}
  typeof Math.sin
```

2  instanceof操作符

> 语法：object instanceof constructor  
> 参数：object需要检测的对象; constructor构造函数  
> instanceof是用于检测对象object是否在构造函数constructor原型属性(prototype property)的原型链(prototype chain)上。

```JavaScript
  //定义构造函数
  function C(){}
  function D(){}

  var objC = new C();
  objC instanceof C; 				//true
  objD instanceof D;				//false
  objC instanceof Object; 			//true, 通过继承
  C.prototype insanceof Object;		//true 	

  C.prototype = {};
  var objC2 = new C();
  objC2 instanceof C;				//true
  objC instanceof C;				//true,因为原型已经改写，切断了原来实例与原型最初的关系
```
