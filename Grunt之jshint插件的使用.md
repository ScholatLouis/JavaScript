###Grunt插件之uglify的使用
> uglify插件是用来压缩js文件，具体的使用我们通过例子说明，首先看下目录结构  
> ->grunt_test  
  --->src  
  ------>jquery.js  
  ------>underscore.js  
  ------>backbone.js  
  --->dest  
  --->Gruntfile.js  
  --->package.json  

现在我们要做的就是把src目录下的所有js文件都进行压缩，然后放到dest目录下。  
首先我们需要配置Gruntfile.js文件，具体配置如下;
```JavaScript
	model.exports = function(grunt){
		grunt.initConfig({
			pkg: grunt.file.readJSON("package.json"),
			jshint: {
				options: {
					//stripBanners主要用于清除/**/这类的注释信息
					stripBanners: true,
					//banner则是在压缩文件的头部添加相应的信息，其中pkg.name是从package.json文件中获取的信息。
					banner: "/* !<%=pkg.name%><%=pkg.version%> <%grunt.template.today('yyyy-mm-dd')%>*/\n"
				},
				build: {
					expand: true,
					cwd: "src/",
					src: "*.js",
					dest: "dest/"
				}
			}
		});
		//加载jshint插件
		grunt.loadNpmTasks("grunt-contrib-jshint");
		//注册事件
		grunt.registerTask("default", ["jshint"]);
	}
```

> 接下来看看packgae.json文件的配置  

```JSON
	{
		"name" : "grunt_test",
		"version": "0.0.1",
		"devDependencies": {}
	}
```
> 在package.json文件中，devDependencies是需要依赖的插件，我们在创建package为你吉安的时候可以不写，在执行npm install grunt-contrib-jshint --save-dev命令之后，devDependencies中的信息自动会添加上去。  
> 配置完成之后，我们执行grunt则可以完成js文件的压缩，同时压缩之后的文件都存放在dest目录下。
