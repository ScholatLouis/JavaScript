###Grunt插件值jshint的使用
> jshint插件是用来检查js的语法，同样通过例子说明，首先看下目录结构：
>> ->grunt_test  
   --->src  
   ----->demo.js  
   --->dest  
   --->Gruntfile.js  
   --->package.json  
   --->jshitrc.json  

首先我们一样先把Gruntfile.js文件配置起来，配置内容如下：
```JavaScript
	model.exports = function(grunt){
		grunt.initConfig({
			pkg: grunt.file.readJSON("package.json"),
			jshint: {
				build: ["src/demo.js"],
				options: {
					jshintrc: "jshintrc.json"
				}
			}
		});
		grunt.loadNpmTasks("grunt-contrib-jshint");
		grunt.registerTask("default", ["jshit"]);
	}
```
> package.json文件的配置内容和之前的配置一样，不再说明。我们主要来看下jshitrc.json文件的内容

```JSON
	//各个配置信息的具体含义：http://jshint.com/docs/options/
	options
	{
		"boss" : false,
		"curly" : true,
		"eqeqeq" : true,
		"eqnull" : true,
		"expr" : true,
		"immed" : true,
		"newcap" : true,
		"noempty" : true,
		"noarg" : true,
		"undef" : true,
		"regexp" : true,
		"browser" : true,
		"devel" : true,
		"node" : true
	}
```
> 这个文件的配置主要是说明对那些语法进行检查，而对于其中各个属性的具体含义，可以通过http://jshint.com/docs/options/ 进行查看。
