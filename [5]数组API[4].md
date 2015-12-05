# [5]数组API[4]
### Iteration Methods
1 这部分将要介绍的API主要是以下几个：
> Array.prototype.forEach()  
> Array.prototype.every()  
> Array.prototype.some()  
> Array.prototype.filter()  
> Array.prototype.map()  
> Array.prototype.reduce()  
> Array.prototype.reduceRight()  

*上面列举的API都是用于遍历数组，这一部分的API比较容易混淆，需要理解清楚。

#### Array.prototype.forEach()
> 数组中的每一个元素都需要经过forEach中的callback函数处理，包括undefined。  
> 语法：arr.forEach(callback, thisArg)  
> callback：就是每个元素都需要经过处理的函数，function(currentValue, index, array){}。callback函数需要传入的参数有三个，分别是currentValue, index, array；    
> currentValue: 是当前处理的元素  
> index: 是当前处理元素在数组中的小标  
> array：是当前被处理的数组  
> thisArg: 当有传入该参数的时候，将该参数赋值给this；如果没有传入该参数，则this赋值为undefined  

```JavaScript
  function logArrayElemnts(element, index, array){
    console.log('a[' + index + ']=' + element);
  }
  //需要注意的是小标为2的元素没有被访问
  [2, 5, , 9].forEach(logArrayElements);	//print: a[0] = 2
											// a[1] = 5
											// a[3] = 9
```
> forEach支持IE9以及以上版本的浏览器，对于低版本的IE则需要使用兼容性代码解决：

```JavaScript
  if(!Array.prototype.forEach){
  	Array.prototype.forEach = function(callback, thisArg){
  		var T, k;
  		if(this == null){
  			throw new TypeError('this is null or not defined');
  		}
  		var O = Object(this);
  		var len = O.length >>> 0;
  		if(typeof callback !== 'function'){
  			throw new TypeError(callback + 'is not a function');
  		}
  		if(arguments.length > 1){
  			T = thisArg;
  		}
  		k = 0;
  		while(k < len){
  			var kValue;
  			if(k in O){
  				kValue = O[k];
  				//callback函数的三个参数kValue, k, O
  				callback.call(T, kValue, k, O);
  			}
  		}
  	}
  }
```

#### Array.prototype.every()
> 当所有的元素经过处理后都返回true，则整体返回true；一旦有一个元素不满足，则直接返回false   
> 语法：arr.every(callback, thisArg)  
> callback: function(currentValue, index, array){}。当一有元素经过callback处理后，返回false，则直接结束后面元素的判断，整体返回false；否则当所有的元素经过callback函数处理之后都是true，则整体返回true    
> callback传入的参数和forEach的含义一样，thisArg也一样  

```JavaScript
  function isBigEnough(element, index, array) {
    return element >= 10;
  }
  [12, 5, 8, 130, 44].every(isBigEnough);   	//print: false
  [12, 54, 18, 130, 44].every(isBigEnough); 	//print: true
```

#### Array.prototype.some()
> 只要数组中任何一个元素返回处理后返回ture，则整体上返回true；否则返回false  
> 语法：arr.some(callback, thisArg)    
> callback传入的参数和forEach的含义一样，thisArg也一样   

```JavaScript
  function isBiggerThan10(element, index, array) {
  	return element > 10;
  }
  [2, 5, 8, 1, 4].some(isBiggerThan10);  	//print: false
  [12, 5, 8, 1, 4].some(isBiggerThan10); 	//print: true
```

#### Array.prototype.filter()
> filter方法会返回一个新的数组，数组中元素的组成规则：根据callback函数返回值来决定该元素是否添加到新数组中，如果callback返回值为true，则添加到新数组中，否则为false，则不添加到新数组中  
> 语法：arr.filter(callback, thisArg)  
> callback传入的参数和forEach的含义一样，thisArg也一样   

```JavaScript
  var arr = [
    { id: 15 },
    { id: -1 },
    { id: 0 },
    { id: 3 },
    { id: 12.2 },
    { },
    { id: null },
    { id: NaN },
    { id: 'undefined' }
  ];
  var invalidEntries = 0;
  function filterByID(obj) {
  	if ('id' in obj && typeof(obj.id) === 'number' && !isNaN(obj.id)) {
    	return true;
  	} else {
    	invalidEntries++;
    	return false;
   	}
  }
  var arrByID = arr.filter(filterByID);
  console.log('Filtered Array\n', arrByID); 	//print: [{ id: 15 }, { id: -1 }, { id: 0 }, { id: 3 }, { id: 12.2 }]
  console.log('Number of Invalid Entries = ', invalidEntries); 	//print: 4
```

####Array.prototype.map()
> map方法会组成一个新的数组，新数组元素的组成则是由每个元素经过callback处理后返回的结果组成。  
> 语法：arr.map(callback, thisArg)  
> callback传入的参数和forEach的含义一样，thisArg也一样   

```JavaScript
  //Example 1
  var kvArray = [{key:1, value:10}, {key:2, value:20}, {key:3, value: 30}];
  var reformattedArray = kvArray.map(function(obj){ 
   var rObj = {};
   rObj[obj.key] = obj.value;
   return rObj;
  });
  console.log(reformattedArray);	//print: [{1: 10}, {2: 20}, {3: 30}]

  //Example 2
  var numbers = [1, 4, 9];
  var roots = numbers.map(Math.sqrt);
  console.log(roots);				//print: [1, 2, 3]
```

####Array.prototype.reduce()
> reduce方法会接收一个函数作为累加器，最后返回一个数值  
> 语法：arr.reduce(callback[, initialValue])  
> callback: function(previousValue, currentValue, currentIndex, array){}  
> 第一次调用callback函数的时候，如果initialValue有值，则第一次调用的时候previousValue为数组中的第一个元素；否则previousValue和currentValue是数组中的前两个元素；  
> 对于一个空数组，而且initialValue为空，则直接返回一个类型错误
> 对于一个空数组，但是initialValue不为空；或者只有一个元素的数组，initialValue为空，则直接返回该元素或initialValue，而不调用callback函数    

```JavaScript
  [0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, currentIndex, array) {
    return previousValue + currentValue;
  });
```
> 上面代码的调用过程如下图：

![reduce调用过程](http://123.56.156.116/Louis/array/ArrayFirst.png)

```JavaScript
  [0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, currentIndex, array) {
  	return previousValue + currentValue;
  }, 10);
```
> 上面代码的调用过程如下图：

![reduce调用过程](http://123.56.156.116/Louis/array/ArraySecond.png)

```JavaScript
  var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
  	return a.concat(b);
  }, []); 			// flattened is [0, 1, 2, 3, 4, 5]
```

####Array.prototype.reduceRight()
> reduceRight方法与reduce一样，唯一的区别就是在对数组元素处理的顺序上，reduceRight的处理顺序是从数组元素的右边往左边方向进行，即从数组的末尾到数组的前面；而reduce则是从数组的前面处理到数组的末尾。  
> 语法：arr.reduceRight(callback[, initialValue])

```JavaScript
  var flattened = [[0, 1], [2, 3], [4, 5]].reduceRight(function(a, b) {
    return a.concat(b);
  }, []);			// flattened is [4, 5, 2, 3, 0, 1]
```
