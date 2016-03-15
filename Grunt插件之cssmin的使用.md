###Grunt插件之cssmin的使用
> cssmin插件是用于压缩css文件的，同样直接上例子。目录结构如下：
>> -->grunt_test  
   ---->src  
   ------->todo.css  
   ---->build  
   ---->Gruntfile.js  
   ---->package.json  

> 首先还是配置Gruntfile.js文件，具体配置如下：

```JavaScript
	model.exports = function(grunt){
		grunt.initConfig({
			pkg: grunt.file.readJSON("package.json"),
			cssmin: {
				build: {
					expand: true,
					cwd: "src/",
					src: "*.css",
					dest: "build/"
				}
			}
		});
		//加载cssmin插件
		grunt.loadNpmTasks("grunt-contrib-cssmin");
		//注册cssmin插件
		grunt.registerTask("default", ["cssmin"]);
	}
```

> package.json和之前的都一样，我们不再说明。
