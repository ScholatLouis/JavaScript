# [3]数组API[2]
### Mutator Methods
1 这个部分介绍的API主要有以下几个：
> Array.prototype.pop()  
> Array.prototype.push()  
> Array.prototype.shift()  
> Array.prototype.unshift()  
> Array.prototype.reverse()  
> Array.prototype.sort()  
> Array.prototype.splice()  

*上面所列举的数组API都会改变当前操作的数组内容。

#### Array.prototype.pop()
> 该方法删除数组的最后一个元素，并且返回被删除的元素。[在原来的数组上操作，且有返回值]  
> 语法：arr.pop();  

```JavaScript
  var arr = [1,2,3,4];
  var temp = arr.pop();
  console.log(temp);		//print: 4
  console.log(arr);			//print: [1,2,3]

  //对于一个空数组，pop元素会返回undefined
  console.log([].pop());	//print: undefined
```

#### Array.prototype.push()
> 该方法将一个或者多个元素插入到数组的最后位置，并且返回新数组的长度。[在原来的数组上操作，并且有返回值]  
> 语法：arr.push(element1[, element2, ... , elementN]);  

```JavaScript
  var sports = ['soccer', 'baseball'];
  var total = sports.push('football', 'swimming');

  console.log(sports);		//print: ['soccer', 'baseball', 'football', 'swimming']
  console.log(total);		//print: 4

  //合并两个数组比较
  var vegetables = ['parsnip', 'potato'];
  var moreVegs = ['celery', 'beetroot'];
  console.log(vegetables.push(moreVegs));	//print: ['parsnip', 'potato', Array[2]]

  //使用apply合并两个数组
  Array.prototype.push.apply(vegetables, moreVegs);
  console.log(vegetables);					//print: ['parsnip', 'potato', 'celery', 'beetroot']
```

#### Array.prototype.shift()
> 该方法将数组的第一个元素移除，并且返回被移除的元素。[在原来的数组上操作，并且有返回值]  
> 语法：arr.shift();  

```JavaScript
  var myFish = ['angel', 'clown', 'mandarin', 'surgeon'];
  console.log('myFish before: ' + myFish);			//print: myFish before: ["angel", "clown", "mandarin", "surgeon"]
  var shifted = myFish.shift(); 
  console.log('myFish after: ' + myFish); 		 	//print: myFish after: ["clown", "mandarin", "surgeon"]
  console.log('Removed this element: ' + shifted); 	//print: Removed this element: angel

  //对于一个空数组，shift会返回undefined
  console.log([].shift());							//print: Undefined
```

#### Array.prototype.unshift()
> 该方法将一个或者多个元素插入到数组的第一个位置，并且返回新数组的长度。[在原来的数组上操作，并且有返回值]  
> 语法：arr.unshift(element1[, element2, ..., elementN]);  

```JavaScript
  var arr = [1, 2];
  arr.unshift(0); 			// result of call is 3, the new array length; arr is [0, 1, 2]
  arr.unshift(-2, -1); 		// = 5; arr is [-2, -1, 0, 1, 2]
  arr.unshift([-3]);		// arr is [[-3], -2, -1, 0, 1, 2]

  //合并两个数组
  Array.prototype.unshift.apply([1,2], [3,4]);	//the new arr is [1,2,3,4]
```

#### Array.prototype.reverse()
> 该方法将数组中所有元素的位置进行反转。[在原来的数组上操作，并且有返回值]  
> 语法：arr.reverse()  

```JavaScript
  var myArray = ['one', 'two', 'three'];
  myArray.reverse();
  console.log(myArray) 		//print: ['three', 'two', 'one']  
```

#### Array.prototype.sort()
> 该方法对数组中所有的元素都进行排序，如果没有提供比较函数compareFunction，则按照字符串的Unicode码的顺序进行排序。[在原来的数组上操作，并且有返回值]  
> 语法：arr.sort([compareFunction])  

```JavaScript
  //比较函数没有提供的情况下：
  var fruit = ['cherries', 'apples', 'bananas'];
  fruit.sort();		//['apples', 'bananas', 'cherries'];

  var scores = [1, 10, 2, 21];		
  scores.sort();	//[1, 10, 2, 21]	//没有提供比较函数，都是转化成字符串，然后根据字符串的Unicode码进行排序

  var things = ['word', 'Word', '1 Word', '2 Words'];
  things.sort();	//['1 Word', '2 Words', 'Word', 'word']
```
> compareFunction(a, b)比较函数   
> 0. 注意：a，b分别代表数组中比较的两个元素，可以是数值、对象、数组等  
> 1. 如果compareFunction(a, b)返回的数值小于0，则a排在b的前面  
> 2. 如果compareFunction(a, b)返回的数值为0，则不对a和b进行交换位置。需要注意的是ESMAscript并没有对此行为做出保证，不同的浏览器可能会有不同的做法。  
> 3. 如果compareFunction(a, b)返回的数值大于0，则a排在b的后面  
> compareFunction(a, b)常见的形式如下：

```JavaScript
  function compare(a, b){
  	if(a is less than b by some ordering criterion){
  		return -1;
  	}
  	if(a is greater than b by the ordering criterion){
  		return 1;
  	}
  	//a must be equal to b
  	return 0;
  }
```
> Example:

```JavaScirpt
  //按数值大小排序
  var numbers = [4, 2, 5, 1, 3];
  numbers.sort(function(a, b){
  	return a - b;
  });
  console.log(numbers);			//print: [1, 2, 3, 4, 5]

  //对于对象也可以按照其中的属性值进行排序
  var items = [
   { name: 'Edward', value: 21 },
   { name: 'Sharpe', value: 37 },
   { name: 'And', value: 45 },
   { name: 'The', value: -12 },
   { name: 'Magnetic' },
   { name: 'Zeros', value: 37 }
  ];
  items.sort(function(a, b){
  	if(a.value > b.value){
  		return 1;
  	}
  	if(a.value < b.value){
  		return -1;
  	}
  	return 0;
  });
  console.log(items);
  //-12, 21, 37, 45, undefined, 37; 因为itesms[4]中的对象没有value属性，无法进行比较，所以前面的4个是按顺序排好，最后两个还是原来的顺序。
```

> 字符串与数值运算小结：  
> console.log(+true);	//print: 1;  
> console.log(+false);	//print: 0;  
> '90' - 80;			//results: 10;  
> 'a' - 'b';			//results: NaN;  
> +'90';				//results: 90;  
> 数值型字符串会先转换成数值再进行运算；而对于非数值型字符串由于无法转换成数值，所以做任何四则运算结果都是NaN  

#### Array.prototype.splice()
> 该方法可以对数组中已经存在元算进行删除，也可以添加元素到数组中。[在原来的数组上操作，并且有返回值]  
> 语法：arr.splice(startIndex, deleteCount[, element1, element2, ..., elementN])  
> startIndex: 改变数组的起始位置，如果startIndex大于数组的长度，则将数组的长度赋值给startIndex；如果startIndex为负数，则加上数组的长度。  
> deleteCount: 从startIndex开始，要删除元素的个数；如果deleteCount为0，则不删除数组中的元素，但是此时需要提供至少一个新的元素(即第三个参数)；如果deleteCount的数值大于startIndex后面元素的个数，则将从startIndex处开始，将数组后面的元素全部删除。  
> elementN: 要添加到数组中的元素，如果该参数为空则splice()值做删除操作。  
> return：返回值是被删除的元素，如果没有删除操作则返回空数组。 

```JavaScript
  var myFish = ['angel', 'clown', 'mandarin', 'surgeon'];
  var removed = myFish.splice(2, 0, 'drum');		//myFish is ['angel', 'clown', 'drum', 'mandarin', 'surgeon']
  													// removed is [], no elements removed
  removed = myFish.splice(3, 1);					// myFish is ['angel', 'clown', 'drum', 'surgeon']
													// removed is ['mandarin']
  removed = myFish.splice(2, 1, 'trumpet');			// myFish is ['angel', 'clown', 'trumpet', 'surgeon']
													// removed is ['drum']
  removed = myFish.splice(0, 2, 'parrot', 'anemone', 'blue'); 
  													// myFish is ['parrot', 'anemone', 'blue', 'trumpet', 'surgeon']
													// removed is ['angel', 'clown']
  removed = myFish.splice(3, myFish.length);		// myFish is ['parrot', 'anemone', 'blue']
													// removed is ['trumpet', 'surgeon']
```
