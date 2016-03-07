```JavaScript
function person(){
	//私有属性
	var age = 10;
	var getAge = function(){
		console.log("halo: " + age);
	}
	var setAge = function(temp){
		age = temp;
		console.log(age);
	}

	return O = {
		getAge: getAge,
		setAge: setAge
	}
}

var temp = person();
temp.getAge();     //--->打印：10
temp.setAge(12);   //--->打印：12
temp.getAge();     //--->打印：12

var temp2 = person();
temp2.getAge();   //--->打印：10
```

> 在控制台中执行以上代码，可以通过getAge方法获取person的年龄，也可以通过setAge获取年龄
