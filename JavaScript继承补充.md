###JavaScript继承补充
在之前的文章中有讲过JavaScript继承的使用方法，这次是在之前的基础上做一些补充。在JavaScript中的继承方式有两种，分别是原型式继承和类式继承。之前的文章介绍的都是通过原型式继承的方式，这里主要补充类式继承。

####JavaScript的原型式继承
虽然之前已经介绍过原型式继承，但是在这里我们再次重温下：
```JavaScript
	//声明一个超类
	function Person(name){
		this.name = name;
	}
	//给超类添加getName的方法
	Person.prototype.getName = function(){
		return this.name;
	}
	//实例化超类
	var temp = new Person("Louis");
	console.log(temp.getName());
	//声明类
	function Programmer(name, sex){
		//调用超类Person的构造函数，并将name传递给它
		Person.call(this.name);
		this.sex = sex;
	}
	//通过原型链实现继承
	Programmer.prototype = new Person();
	//因为子类的原型对象等于超类的实例，所以prototype.constructor这个方法也等于超类的构造函数，因此对constructor的指向进行纠正。
	Programmer.prototype.constructor = Programmer;
	//给子类添加getSex方法
	Programmer.prototype.getSex = function(){
		return this.sex;
	}
	//实例化子类
	var _temp = new Programmer("Louis", "male");
	console.log(_temp.getSex());
	console.log(_temp.getName());
```

####JavaScript的类式继承
```JavaScript
	var clone = function(obj){
		var _f = function(){};
		//原型式继承最核心的地方，函数的原型对象为对象字面量
		_f.prototype = obj;
		return new _f;
	}
	//声明一个对象字面量最为超类
	var Person = {
		name : "Louis",
		getName : function(){
			return this.name;
		}
	}
	//不需要定义一个Person的子类，只要执行一次克隆即可
	var Programmer = clone(Person);
	console.log(Programmer.getName());
	Programmer.name = "haloLois";
	console.log(Programmer.getName);
	//声明子类，只需要执行一次克隆即可
	var someOne = clone(Programmer);
```
