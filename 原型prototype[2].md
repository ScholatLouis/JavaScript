# 原型prototype[2]
 前面说了介绍了原型，这一部分说说原型的一大作用：继承。在面向对象中，继承有两种方法，分别是接口继承和实现继承。但是在JavaScript中，函数不能只签名，所以JavaScript中只有实现继承。
> JavaScript继承的基本思路就是让一个引用类型继承另一个引用类型的属性和方法。    

```JavaScript
  function SuperType(){
    this.property = true;
  };
  SuperType.prototype.getSuperValue = function(){
  	return this.property;
  };
  function SubType(){
    this.subproperty = false;
  };
  //继承SuperType
  SubType.prototype = new SuperType();
  SubType.prototype.getSubValue = function(){
    return this.subproperty;
  };
  var instance = new SubType();
  console.log(instance.getSupterValue());	//print: true
```
> 首先我们先来看下上面函数之间的关系，如下图：  

![上面代码的原型关系图](https://github.com/ScholatLouis/ScholatLouis.github.io/blob/master/BlogImg/prototype/prototypeThird.png)  

  1 由上面的图我们可以看到，继承其实就是通过改写原型实现的。我们将SuperType的一个实例赋值给SubType.prototype，使得SubType.prototoype含有指向SuperType.prototype的指针，这样就将原型之间的关系联系起来，构成了原型链(prootype chain)。  
  2 注意观察，可以发现在SubType.prototype原型中含有SuperType构造函数中的属性property。这是因为SubType.prototype是作为SuperType的一个实例，所以自然保存有SuperType实例中的属性，但是对于getSuperValue方法则还是保存在SuperType.prototype中。  
  3 SubType.prototype中含有的属性都是共享的，所以对于property属性，如果SubType有多个实例则是共享该属性值。  
  4 原型继承之后的原型搜索，还是和之前一样，先搜索实例对象的属性，然后在沿着原型链逐步向上搜索，如果最后还是没有找到则直接返回undefined。  
  5 在JavaScript中，所有的对象都是继承自Object，所以所有函数的默认原型都是Object的实例，因此默认原型都会有一个内部指针，指向Object.prototype。这也是为什么每一个函数都会有toString(), valueOf()等默认方法的基本原因。
  6 对于使用原型继承的时候，不能使用字面量的形式来重写原型，如：

 ```JavaScript
   function SuperType(){
   	this.property = true;
   };
   SuperType.prototype.getSuperValue = function(){
   	return this.property;
   };
   function SubType(){
   	this.subproperty = false;
   };
   //继承SuperType
   SubType.prototype = new SuperType();
   //使用字面量添加新方法，相当于重写了原型，会上面一行代码无效
   SubType.prototype = {
   	getSubValue: function(){
   		return this.subproperty;
   	},
   	someOtherMethod: function(){
   		return false;
   	}
   };
   var instance = new SubType();
   console.log(instance.getSuperValue());	//print: error!找不到该方法
 ```
> 1 上面的代码首先将SubType.prototype赋值为SuperType的实例，这个时候是建立了继承关系；但是在后面又重写了SubType.prototype，这时候SubType.prototype变成了Object对象的一个实例，因此SubType和SuperType之间的关系已经被切断，两者之间没有了关系。 

### 原型链存在的问题
```JavaScript
  function SuperType(){
    this.color = ['red', 'blue', 'green'];
  };

  function SubType(){}

  //继承了SuperType
  SubType.prototype = new SuperType();

  var instanceFirst = new SubType();
  instanceFirst.color.push('black');
  console.log(instanceFirst.color);			//print: ['red', 'blue', 'green', 'black']

  var instanceSecond = new SubType();
  console.log(instanceSecond.color);		//print: ['red', 'blue', 'green', 'black']
```
> 首先还是来看下函数原型之间的关系，如下图：  

![上面代码的原型关系图](https://github.com/ScholatLouis/ScholatLouis.github.io/blob/master/BlogImg/prototype/prototypeForth.png)  

> 1 由上面的图可以看到，由于SubType.prototype是作为SuperType的实例，所以在SubType.prototype中保存有一份SuperType实例对象都具有的属性，即color属性。这就会导致SubType的所有实例都共享一份color；  
> 2 又因为在原型中所有属性都是共享的，所以在SubType的实例中都能引用到color的属性值。因此我们可以看到在instanceFirst和instanceSecond都是共享同一个color数组，这也就是为什么instanceFirst改变了color的值之后，instanceSecond的color数组也跟着改变的原因。    
> 3 原型链继承存在的问题：通过原型继承，原型实际上是变成另一个类型的实例，于是原先的实例属性也就顺理成章的变成了现在原型的属性。   

### 原型链问题的解决方法
1 对于从SuperType构造函数继承下来的属性到原型中的问题，可以通过在SubType的构造函数中调用SuperType构造函数。可以通过call和apply来实现。
```JavaScript
  function SuperType(){
  	this.colors = ['red', 'blue', 'green'];
  };
  function SubType(){
  	//继承SuperType
  	//在this的作用域上调用SuperType构造函数
  	SuperType.call(this);
  };

  var instanceFirst = new SubType();
  instanceFirst.colors.push('black');
  console.log(instanceFirst.colors);		//print: ['red', 'blue', 'green', 'black']

  var instanceSecond = new SubType();
  console.log(instanceSecond.colors);		//print: ['red', 'blue', 'green']
```
> 上面的这种方法通常称之为经典继承方法，但是该方法还是存在构造函数模式的缺点，即所有的方法都在构造函数中定义，因此函数的复用就无从谈起。因此很多情况下都是使用下面的一种方法：组合继承。  

2 组合继承即在上面的方法中添加如原型链的形式。
```JavaScript
  function SuperType(name){
  	this.name = name;
  	this.colors = ['red', 'blue', 'green'];
  };
  SuperType.prototype.sayName = function(){
  	console.log(this.name);
  };
  function SubType(name, age){
  	//继承SuperType属性
  	SuperType.call(this, name);
  	this.age = age;
  };
  SubType.prototype = new SuperType();
  SubType.prototype.sayAge(){
  	console.log(this.age);
  };

  var instanceFirst = new SubType("Louis", 23);
  instanceFirst.colors.push('black');
  console.log(instanceFirst.colors);		//print: ['red', 'blue', 'green', 'black']
  console.log(instanceFirst.sayName());		//print: Louis
  console.log(instanceFirst.sayAge());		//print: 23

  var instanceSecond = new SubType("June", 21);
  console.log(instanceSecond.colors);		//print: ['red', 'blue', 'green']
  console.log(instanceSecond.sayName());	//print: June
  console.log(instanceSecond.sayAge());		//print：21
```
> 这种方法避免了原型链和经典继承的弊端，因此较为常用。
