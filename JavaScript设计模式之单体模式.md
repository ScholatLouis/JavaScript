### JavaScript设计模式之单体模式
> 单体singleton模式提供了一种将相关属性和方法组成一个逻辑单元的方法；而对于能够实例化的对象，单体模式主要表现在只能创建一个对象。

##### 单体的基本结构
> 单体的基本结构主要适用于划分命名空间，以减少全局变量的数目。最简单的单体模式和对象的字面量创建方法类似：

```JavaScript
	var Singleton = {
		attribute1 : true,
		attribute2 : 10,
		method1 : function(){},
		method2 : function(){}
	};
```

> 这样所有的成员都需要通过Singleton来访问，如：Singleton.attribute1, Singleton.method1()。  
> 这种方式的缺点就是Singleton中的属性和方法对外完全暴露，可以通过外界进行修改，如：Singleton.attribute1 = false  
> 划分命名空间的使用方法跟使用字面量的方式创建对象类似。下面我们说说具有私有变量的单体模式的结构

##### 具有私有变量的单体模式
> 在单体模式中要实现这种具有私有变量有两种方法，分别是采用下划线方法和使用闭包来完成。

1 使用下划线
> 这种方式只是将私有变量的命名方式改为加上下划线_而已，但是实际上并不是严格意义上的私有变量，因为在外界一样可以访问得到这种类型的私有变量。如：

```JavaScript
	GiantCorp = {
		//private methods
		_stripWhiteSpace : function( str ){
			return str.replace(/s+/, "");
		},
		_stringSplit : function( str, delimiter ){
			return str.split(delimiter);
		},
		//public method
		stringToArray : function( str, delimiter, stripWS ){
			if( stripWS ){
				str = this._stripWhiteSpace( str );
			}
			var outputArray = this._stringSplit( str, delimiter );
			return outputArray;
		}
	};
```

> 在上面的例子中，私有函数有：_stripWhiteSpace和\_stringSplit两个方法，但是实际上我们在外部可以通过GiantCorp.\_stripWhiteSpace和GiantCorp.\_stringSplit来访问。所以说这种方式实际上并不能真正地完成变量的私有化，下面我们介绍闭包的方法。

2 使用闭包
> 其实使用闭包的方式来实现私有变量和私有方法，在我的github上之前已经有给出代码：[闭包的使用：实现私有属性的getter和setter](https://github.com/ScholatLouis/JavaScript/blob/master/%E9%97%AD%E5%8C%85%E7%9A%84%E4%BD%BF%E7%94%A8%EF%BC%9A%E5%AE%9E%E7%8E%B0%E7%A7%81%E6%9C%89%E5%B1%9E%E6%80%A7%E7%9A%84getter%E5%92%8Csetter.md)，但是我们这里将的通过闭包实现的私有属性和私有方法和上面的还是不太一样，具体差别在哪里，我们后面分析。

```JavaScript
	GiantCorp = (function(){
		function _stripWhiteSpace( str ){
			return str.replace(/s+/, "");
		};
		function _stringSplit( str, delimiter ){
			return str.split(delimiter);
		};
		return {
			stringToArray : function( str, delimiter, stripWS ){
				if( stripWS ){
					str = _stripWhiteSpace( str );
				}
				var outputArray = _stringSplit( str, delimiter );
				return outputArray;
			}
		}
	})();
```

> 上面是通过立即执行函数来返回得到stringToArray函数的，由于GiantCorp只能创建一个对象，所以对于闭包的使用并不会引起太大的内存消耗，这就是使用单例模式的时候使用闭包实现私有属性方法的好处。

```JavaScript
	function GiantCorp(){
		function _stripWhiteSpace( str ){
			return str.replace(/s+/, "");
		};
		function _stringSplit( str, delimiter ){
			return str.split(delimiter);
		};
		return {
			stringToArray : function( str, delimiter, stripWS ){
				if( stripWS ){
					str = _stripWhiteSpace( str );
				}
				var outputArray = _stringSplit( str, delimiter );
				return outputArray;
			}
		}
	}
```

> 上面的代码没有将GiantCorp使用立即执行函数，因此我们可以通过创建多个GiantCorp来得到多个stringToArray函数，但是这样的话会导致内存消耗的增加，因为闭包函数引用到的两个函数_stripWhiteSpace和\_stringSplit一直存放在内存中，而且每个对象都保存一个副本，并不是公用。

##### 惰性实例化单例模式
> 上面都是使用立即执行函数来创建单例模式，也就是在js加载完成之后就会创建相应的对象，但是如果我们并不需要立即创建对象，那么这种时候就需要使用惰性的方式来实现单例模式了。惰性实例化模式和之前的差别主要在需要通过一个静态函数getInstance()来获取到对象，进而通过这个获取到的对象来引用方法。具体看以下代码

```JavaScript
	GiantCorp = (function(){
		var uniqueInstance = null;
		function constructor(){
			function _stripWhiteSpace( str ){
				return str.replace(/s+/, "");
			};
			function _stringSplit( str, delimiter ){
				return str.split(delimiter);
			};
			return {
				stringToArray : function( str, delimiter, stripWS ){
					if( stripWS ){
						str = _stripWhiteSpace( str );
					}
					var outputArray = _stringSplit( str, delimiter );
					return outputArray;
				}
			}
		};
		return {
			getInstance : function(){
				if( !uniqueInstance ){
					uniqueInstance = constructor();
				}
				return uniqueInstance;
			}
		}
	})();
```

> 对于惰性单例模式，我们的调用方式为:GiantCorp.getInstance().stringToArray();
