###require学习笔记[2]
1 require的加载  

> <script type="text/javascript" data-main="main" src="js/require.js"></script>  
其中src的属性值用来指定require库的位置，而data-main用来指定主模块，通常命名为main.js，由于require知道加载的脚本为.js格式，因此可以省略掉.js直接写成main

2 主模块main的配置

```JavaScript
	require(["moduleA", "moduleB", "moduleC"],function(moduleA, moduleB, moduleC){
		//...
	});
```
> 在require中第一个参数为数组，用于指定所有依赖的模块，模块的加载采用异步的方式进行。第二个参数是回调函数，所有的模块加载完成之后就会调用该函数，函数的参数则是各个依赖模块的输出值。如：  

```JavaScript
	require(["jquery", "underscore", "backbone"], function($, _, backbone){
		//...
	});
```
> require.js会依次加载jquery,underscore,backbone，然后调用回调函数。

3 主模块main的加载配置

> 在主模块main中，所有依赖的模块都需要指定路径才能加载成功，而路径的配置方式如下：

```JavaScript
	require.config({
		paths:{
			"jquery" : "jquery.min",
			"underscore" : "underscore.min",
			"backbone" : "backbone.min"
		}
	});
```
> 上面的配置是和main.js同个目录下的js，如果不在不同一目录下则需要加上各自的目录。假设三个库都是处于js/lib目录下，则可以直接使用基目录baseUrl指定。

```JavaScript
	require.config({
		baseUrl : "js/lib",
		paths : {
			"jquery" : "jquery.min",
			"underscore" : "underscore.min",
			"backbone" : "backbone.min"
		}
	});
```

4 非规范模块的加载

> 由于require采用的是AMD的规范，所以所有加载的模块都需要是采用AMD规范进行。所以对于非采用AMD规范的模块，需要转换成AMD模块再进行加载。  
对于非AMD规范的模块可以采用shim进行转换，具体配置如下(underscore和backbone两个库都不是AMD规范)：

```JavaScript
	require.config({
		shim: {
			"underscore" : {
				//输出变量
				exports: "_"
			},
			"backbone" : {
				//deps数组用于指定该模块的依赖性
				deps: ["underscore", "jquery"],
				exports: "backbone"
			},
			"jquery.scroll" : {
				deps: ["jquery"],
				exports: "jQuery.fn.scroll"
			}
		}
	});
```
