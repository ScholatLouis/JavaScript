#原型Prototype
> 在看了不少资料之后，对于原型的理解就是两个字，共享。
  在这一部分，将通过分析原型出现的原因，已经原型的应用-->继承。
  
###原型Prototype的出现
> 在讨论原型的出现，就需要从对象的创建开始说起。
 
  创建对象可以通过new Object()或者直接通过对象字面量{}完成，但是使用这种方法会导致在创建多个对象的时候，产生大量的代码。因此有了工厂模式：  
 1 工厂模式  
  工厂模式就是使用一个函数将需要创建的对象的属性都封装起来，如：  

  ```JavaScript
  	function createPerson(name, age, job){
  		var obj = new Object();
  		obj.name = name;
  		obj.age = age;
  		obj.job = job;
  		obj.alertName = function(){
  			console.log(this.name);
  		}
  		return obj;
  	}

  	var personFirst = createPerson("June", 21, "Student");
  	var PersonSecond = createPerson("Louis", 23, "Student");
  ```
  这样就可以通过工厂模式创建对象，只需要将参数传入到createPerson函数中，函数会返回一个obj初始化后的对象。需要注意的是，使用工厂模式创建的对象，不能解决对象识别问题，即：

  ```JavaScript
  	personFirst instanceof object 	//打印true
  	personFirst instanceof createPerson 	//打印false
  ```

 2 构造函数模式
  ```JavaScript
  	function Person(name, age, job){
  		this.name = name;
  		this.age = age;
  		this.job = job;
  		this.alertName = function(){
  			console.log(this.name);
  		}
  	}

  	var personFirst = new Person("June", 21, "Student");
  	var personSecond = new Person("Louis", 23, "Student");
  ```

> 1.首先构造函数和普通的函数是没有什么不同  
> 2.要把一个函数当成构造函数使用，只需要使用new操作符  
> 3.根据习惯，构造函数的第一个字母使用大写  

* new操作符创建对象的过程
  创建一个新的对象 --> 将构造函数的作用域赋给新对象(即this指向了新对象) --> 对象的初始化 --> 返回新对象

* 构造函数模式的不足

> 使用构造函数存在的最大问题就是每个实例都会将所有的属性创建一次。这个对于数值属性来说可以接受，但是如果函数方法每个实例都要创建一遍，则不合理。所以对于构造函数模式可以将方法转移到构造函数外部，如下：

  ```JavaScript
  	function Person(name, age, job){
  		this.name = name;
  		this.age = age;
  		this.job = job;
  		this.sayName = sayName;
  	}

  	function sayName(){
  		console.log(this.name);
  	}

  	var personFirst = new Person("June", 21, "Student");
  	var personSecond = new Person("Louis", 23, "Student");
  ```
  	
> 但是对于这种方法，如果需要多个函数，则会导致很多个全局函数。对于构造函数模式的这个问题，我们可以通过原型来解决。  

3 原型模式
 * 原型

> 在每个创建的函数中都会有一个原型prototype属性，这个属性是一个指针，指向一个对象。原型指向的这个对象包含可以由该函数创建的对象共享访问的属性和方法。简单的说就是该原型指向的对象中含有可以被共享访问的属性和方法，而只有该函数创建的实例才有权限访问。

  ```JavaScript
  	//构造函数
  	function Person(){}
  	//构造函数prototype指向的原型对象
  	Person.prototype.name = "Louis";
  	Person.prototype.age = 23;
  	Person.prototype.job = "Student";
  	Person.prototype.sayName = function(){
  		console.log(this.name);
  	}
  	//使用构造函数创建的实例
  	var personFirst = new Person();
  	personFirst.sayName();

  	var personSecond = new Person();
  	personSecond.sayName();

  	console.log(personFirst.sayName == personSecond.sayName);	//打印true
  ```
> 我们在Person函数的原型中添加的所有属性和方法都能够被personFrist和personSecond访问，而且需要注意的是共享访问，而不是各自保存一个副本。

 * 原型对象的理解
  1. 在创建了一个函数之后，JavaScript引擎就会为该函数创建一个prototype属性，这个属性指向原型对象。而原型对象中含有一个constructor的属性，这个属性指向含有原型对象所在的函数。就如之前的构造函数Person、Person.prototype、personFirst之间的关系如下图：
  ![三者之间的关系示意图](https://github.com/ScholatLouis/ScholatLouis.github.io/blob/master/BlogImg/prototype/prototypeFirst.png)

> 由上图可以看到，其实实例和构造函数之间并没有直接的联系。每个实例中都含有[[prototype]]属性，这个属性指向Person.prototype这个原型对象，而Person.prototype原型对象中含有constructor属性则指向构造函数Person，构造函数也含有指向Person.prototype原型的属性prototype。  
> 在personFirst中我们可以看到，该对象除了[[prototype]]属性之外，并没有其他的属性。但是我们能够通过personFirst.name得到“Louis"的值。这就是原型和对象实例查找属性的问题：  
>> 一个对象在读取某个属性值的时候，首先是先查找该对象是否包含这个属性，如果有则返回，不再向原型查找；如果没有则向上查找该对象指向的原型是否含有这个属性，如果有则返回原型中该属性的值，如果没有则继续向继承的原型链上查找，如果还是没有则直接返回undefined。  
>> 对于上面的例子，在使用personFirst.name访问取name属性值的时候，首先先找personFirst这个对象上是否含有属性值name，很明显并没有，接着则personFirst的原型链Person.prototype上查找name属性，发现有，而且值为"Louis"，则将该值直接返回。  

 2. 原型属性的屏蔽：当我们在对象实例上创建一个和原型上相同名称的属性时，根据属性的查找顺序，我们可以知道原型上相同名称的属性将会被屏蔽掉。Show the fllowing code!

  ```JavaScript
    //构造函数
  	function Person(){}
  	//原型对象
  	Person.prototype.name = "Louis";
  	Perosn.prototype.age = 23;
  	Person.prototype.job = "Student";
  	Person.prototype.sayName = function(){
  		console.log(this.name);
  	}
  	//实例
  	var personFirst = new Person();
  	personFirst.name = "John";
  	personFirst.sayName()	//打印: John	--> 来自实例

  	var personSecond = new Person();
  	personSecond.sayName()	//打印：Louis	--> 来自原型
  ```

> 当我们为对象添加一个属性的时候，这个属性就会屏蔽原型对象中的同名属性；也就是说如果添加的这个属性只会阻止我们访问原型中的那个对象，但是不会修改原型中的属性。不过，我们可以使用delete操作符来删除对象实例属性，从而恢复访问原型中的对象。

  ```JavaScript
    function Person(){}
    Person.prototype.name = "Louis";
    Person.prototype.age = 23;
    Person.prototype.job = "Student";
    Person.prototype.sayName = function(){
    	console.log(this.name);
    }

    var person1 = new Person();
    var person2 = new Person();

    person1.name = "June";
    console.log(person1.name);	 	//打印： June --> 来自对象
    console.log(person2.name);		//打印： Louis--> 来自原型

    delete person1.name;
    console.log(person1.name); 		//打印： Louis--> 来自原型
  ```
 * in操作符
  1. hasOwnProperty && in 操作符
   使用hasOwnProperty()方法可以检测是一个属性是存在于实例中，还是原型中。只有当给定的属性存在于对象实例中，才会返回true。而对于in操作符，则无论属性是否存在于实例还是原型中，只要能够访问到这个属性，则返回true。

```JavaScript
   	//使用hasOwnProperty()方法
   	function Person(){};
   	Person.prototype.name = "Louis";
    Person.prototype.age = 23;
    Person.prototype.job = "Student";
    Person.prototype.sayName = function(){
    	console.log(this.name);
    }

    var personFirst = new Person();
    var personSecond = new Person();

    console.log(personFirst.hasOwnProperty("name"));	//false	--> 来自原型
    console.log(personSecond.hasOwnProperty("age"));	//false --> 来自原型

    personFirst.name = "June";
    console.log(personFirst.hasOwnProperty("name"));	//true --> 来自对象实例
```

```JavaScript
	//使用in操作符
   	function Person(){};
   	Person.prototype.name = "Louis";
    Person.prototype.age = 23;
    Person.prototype.job = "Student";
    Person.prototype.sayName = function(){
    	console.log(this.name);
    }

    var personFirst = new Person();
    var personSecond = new Person();

    console.log("name" in personFirst);	//true --> 来自原型
    console.log("age" in personSecond);	//true --> 来自原型

    personFirst.name = "June";
    console.log("name" in personFirst);	//true --> 来自实例对象
```   

> 下面的代码用来检测属性是否存在于原型中

```JavaScript
	function hasPrototypeProperty(obj, name){
		return !obj.hasOwnProperty(name) && (name in obj);
	}
```

 * 原型的字面量写法
  1. 上面使用Person.prototype.xxx的方法书写原型，每次需要添加属性的时候都要重复书写前面部分的代码。对于这种情况，我们可以使用原型的字面量来创建原型对象。

```JavaScript
	function Person(){}
	Person.prototype = {
		name: "Louis",
		age : 23,
		job : "Student",
		sayName : function(){console.log(this.name);}
	}
```
> 使用原型字面量有一点需要注意的是，原型对象的constructor属性不再指向原来的构造函数Person();  
> 这是因为使用Person.prototype = {}创建原型，相当于Person.prototype是Object的实例，而实例中存在的[[prototype]]指向的是Object的原型，原型中的constructor属性指向的就是Object对象，所以如果constructor属性很重要，则需要在创建原型的字面量上重新指定，如：  

```JavaScript
	function Person(){}
	Person.prototype = {
		constructor : Person,
		name: "Louis",
		age : 23,
		job : "Student",
		sayName : function(){console.log(this.name);}
	}
```

 * 原型的动态性
  1. 原型的动态性体现在只要对原型对象所做的任何修改都能够立即从实例上得到反映。  

```JavaScript
	var friend = new Person();
	Person.prototype.sayHi = function(){
		console.log("halo Louis");
	}
	friend.sayHi();		//打印： halo Louis
```

> 需要注意的是，给原型添加属性和方法都能够在实例上得到反映，但是如果重新改写原型，则会切断实例和原型之间最初的关系。  
> 需要谨记的是：实例中的指针仅仅指向原型，而不是指向构造函数  
> 重写原型之后构造函数、对象实例、原型之间的关系如下图：  

![重写原型之后构造函数、对象实例、原型之间的关系](https://github.com/ScholatLouis/ScholatLouis.github.io/blob/master/BlogImg/prototype/prototypeSecond.png)


 * 原型的弊端  
  1.原型的弊端也是显而易见的，对于数值型属性而言，每一个属性也都是共享的。对于所创建的每一个实例都具有相同的属性值，其实也不合理。应该是数值型属性为各自所有，而方法属性则可以进行共享，所以一般在创建构造函数的时候都是使用构造函数模式和原型模式相结合进行，如：  

```JavaScript
	//属性私有
	function Person(name, age, job){
		this.name = name;
		this.age = age;
		this.job = job;
	}
	//方法共有
	Person.prototype.sayName = function(){
		console.log(this.name);
	}
	var personFirst = new Person("June", 21, "Student");
	var personSecond = new Person("Louis", 23, "Student");
	personFirst.sayName(); 	//打印： June
	personSecond.sayName();	//打印： Louis
```

由于篇幅过长，关于原型的应用继承放在原型Prototype[2]

###参考文章
> [1]https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain  
> [2]http://yehudakatz.com/2011/08/12/understanding-prototypes-in-javascript/  
> [3]http://blog.vjeux.com/2011/javascript/how-prototypal-inheritance-really-works.html  
> [4]http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html  
> [5]JavaScript高级程序设计(3nd Edition)第六章  
