#JavaScript的深浅拷贝
##0 基本数据类型和引用类型
　JavaScript的数据类型可以分为基本类型和引用类型。基本数据类型数字、字符串、boolean值三种；引用类型有：Number、String、null、undefined、function、array、Object七种。
##1 深浅拷贝的含义
　【结论1】在基本数据类型中，赋值就是深拷贝，而在引用类型中，赋值仅仅是浅拷贝。<br/>
　【结论2】浅拷贝说到底就是地址的引用，而深拷贝则是数据和对象的复制。
-------------------
  基本数据类型的深拷贝<br/>
　``` JavaScript
　var num = 1;
　var num1 = 2;
　num = 3;
　console.log(num);   //3
　console.log(num1);  //2
　```
##2 数组的深浅拷贝
* 数组的浅拷贝
* 数组的深拷贝
##3 对象的深浅拷贝
* 对象的浅拷贝
* 对象的深拷贝
##4 JQuery的深浅拷贝
##【参考文章】
* [1]
* [2]
* [3]
