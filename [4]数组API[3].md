# [4]数组API[3]
### Accessor Methods
1 这部分将要介绍的API主要是以下几个：
> Array.prototype.concat()  
> Array.prototype.join()  
> Array.prototype.slice()  
> Array.prototype.toString()  
> Array.prototype.indexOf()  
> Array.prototype.lastIndexOf()  

*上面列举的API都不会修改原来的数组

#### Array.prototype.concat
> 由原来的数组和提供的参数组成一个新的数组，并返回。  
> 语法：var newArr = oldArr.concat([element1, element2, element3, ..., elementN]);  
> 说明：   
>> 1 concat()函数并不会改变原来数组中的元素，但是会返回一个新的数组。   
>> 2 对于数组中含有对象引用，concat()函数只是进行浅复制(shallow copy)，即新数组和原来数组都是指向同一个对象。也就是说，一旦指向的对象被修改，则原来数组和新数组中相应的元素都会发生变化。   
>> 3 对于字符串和数值则是直接复制到新数组中。 

```JavaScript
  var alpha = ['a', 'b', 'c'],
      numeric = [1, 2, 3];
  var alphaNumeric = alpha.concat(numeric);
  console.log(alphaNumeric); 	//result: ['a', 'b', 'c', 1, 2, 3]

  var num1 = [1, 2, 3],
      num2 = [4, 5, 6],
      num3 = [7, 8, 9];
  var nums = num1.concat(num2, num3);
  console.log(nums); 			//result: [1, 2, 3, 4, 5, 6, 7, 8, 9]

  //指向对象引用
  var obj = {name: "Louis", age: 23};
  var objArr = [obj];
  var newArr = objArr.concat();
  console.log(objArr[0].age);	//result: 23
  console.log(newArr[0].age);	//result: 23

  obj.age = 24;
  console.log(objArr[0].age);	//result: 24
  console.log(newArr[0].age);	//result: 24
```

#### Array.prototype.join()
> 该方法将数组转换成字符串，并返回该字符串。  
> 语法：arr.join([separator=','])  
> separator: 分隔符是可选的，当没有传入分隔符的时候则数组返回的字符串中每个元素使用逗号,  进行分开；如果有传入分隔符，则按照分隔符分开。  

```JavaScript
  var a = ['Wind', 'Rain', 'Fire'];
  var myVar1 = a.join();      // assigns 'Wind,Rain,Fire' to myVar1
  var myVar2 = a.join(', ');  // assigns 'Wind, Rain, Fire' to myVar2
  var myVar3 = a.join(' + '); // assigns 'Wind + Rain + Fire' to myVar3
  var myVar4 = a.join('');    // assigns 'WindRainFire' to myVar4
```

#### Array.prototype.slice()
> 该方法返回一个新的数组，新的数组是原来数组的部分浅复制(shadllow copy)。  
> 语法：arr.slice([startIndex[, endIndex]])  
> startIndex: 当没有传入该参数则默认为0；如果传入的是负数，则加上数组长度；  
> endIndex: 当没有传入该参数则默认是数组的长度(arr.length)；如果传入的是负数，则加上数组的长度  
> 需要注意的是截取的数组中不包括endIndex位置处的元素，即从startIndex到endIndex-1   
> 对于对象引用，跟Array.prototype.concat()函数一样  

```JavaScript
  var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
  var citrus = fruits.slice(1, 3);		// citrus = ['Orange','Lemon']
```

#### Array.prototype.toString()
> 将数组组成一个字符串，并返回字符串。  
> 语法：arr.toString()  
> 与arr.join();得到的字符串一样  

```JavaScript
  var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
  var temp = fruits.toString();		//print: "Banana,Orange,Lemon,Apple,Mango"
  var temp2 = fruits.join();		//print: "Banana,Orange,Lemon,Apple,Mango"
```

#### Array.prototype.indexOf()
> 该方法从数组(由前往后，即从0到arr.length-1顺序)找查找某个元素，如果能找到则返回第一个相同元素在数组中的位置；如果没能找到则返回-1  
> 语法：arr.indexOf(searchElement[, fromIndex = 0])
> searchElement: 要在数组中查找的元素
> fromIndex: 从某个位置开始查找，如果没有该参数则默认为0；如果fromIndex大于或者等于数组的长度，则直接返回-1；如果fromIndex小于0，则加上数组的长度，从结果值的位置开始查找，如果结果值还是小于0，则是整个数组查找。  

```JavaScript
  var array = [2, 5, 9];
  array.indexOf(2);     // 0
  array.indexOf(7);     // -1
  array.indexOf(9, 2);  // 2
  array.indexOf(9, 3);	//-1
  array.indexOf(2, -1); // -1
  array.indexOf(2, -3); // 0
```

```JavaScript
  //找到数组中某个元素的所有位置
  var array = ['a', 'b', 'a', 'c', 'a', 'd'];
  var searchElement = 'a';

  var indices = [];
  var index = array.indexOf(searchElement);
  while(index != -1){
  	indices.push(index);
  	index = array.indexOf(searchElement, index + 1);
  }
  console.log(indices);
```
> indexOf()只在IE9及其以上支持，所以对于低版本的IE需要先实现indexOf()函数，如下：  

```JavaScript
  if(!Array.prototype.indexOf){
  	Array.prototype.indexOf = function(searchElement, fromIndex){
  		var k;
  		//this是指向调用的函数
  		//只有当this === undefined的时候才会 === void 0
  		if(this == null || this === void 0){
  			throw new TypeError('"this" is null or not defined');
  		}

  		//将函数转化成对象
  		var O = Object(this);

  		//>>>的两个作用：1)将非数值型转化成0；2)对于含有小数点的，只取其整数部分
  		var len = O.length >>> 0;

  		if(len === 0){
  			return -1;
  		}

  		//根据是否传入fromIndex进行取值
  		var n = +fromIndex || 0
  		if(Math.abs(n) === Infinity){
  			n = 0;
  		}
  		if(n >= len){
  			return -1;
  		}

  		//k其实是数组下标
  		k = Math.max(n >= 0 ? n : len - Math.abs(n), 0);
  		//逐个数组元素进行比较
  		while( k < len ){
  			if(k in O && O[k] === searchElement){
  				return k;
  			}
  			K++;
  		}
  		return -1;
  	}
  }
```

#### Array.prototype.lastIndexOf()
> 从数组中查找指定的元素，如果存在则返回该元素第一次出现在数组中的位置，否则返回-1。需要注意的是查找的方向是从数组尾部往前，即arr.length到0   
> 语法：arr.lastIndexOf(searchElement[, fromIndex = arr.length - 1])   
> searchElement: 要查找的值  
> fromIndex: 如果没有传入该参数则默认为arr.length数组长度；如果传入的数值大于或等于数组的长度，则对整个数组都进行查找；如果传入的是负数，则加上数组的长度，从结果值处开始查找，如果结果值还是负数则直接返回-1；需要注意的是对于有传入fromIndex，是从fromIndex处开始往前查找  

```JavaScript
  var array = [2, 5, 9, 2];
  array.lastIndexOf(2);     // 3
  array.lastIndexOf(7);     // -1
  array.lastIndexOf(2, 3);  // 3
  array.lastIndexOf(2, 2);  // 0 注意结果不是 3
  array.lastIndexOf(2, -2); // 0
  array.lastIndexOf(2, -1); // 3
```
