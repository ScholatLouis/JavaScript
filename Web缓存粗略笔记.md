### Web缓存
之前有看过web缓存之类的文章，自己也有总结过，现在将总结的知识点粗略记录下，等过几天再回来丰富。  

> 缓存应该说是大家都比较熟悉的东西了，主要是为了用来提高各种速度，减轻压力。至于提高什么速度，减轻什么压力，这跟缓存的类型挂钩的。不同的缓存类型不一样，比如说数据库缓存则是为了提高数据查找的速度，，减轻数据库的压力。

##### 缓存的类型
缓存的类型大体可以分为以下四种:  
1 数据库缓存memcached  
2 服务器端缓存：  
  1) CDN(内容分发网络)缓存  
  2) 代理服务器缓存 squid  
  
3 浏览器缓存  
4 Web应用缓存  
  
##### Web应用缓存
这里我们主要说说Web应用缓存。Web应用缓存的缓存规则有两种，一种是根据http协议进行，另外一种则是非http协议的缓存机制。  
1 非http协议的缓存机制  
很多时候我们可能会在html页面中看到如下的一句代码：  
```HTML
	<meta http-equiv="pragma" content="no-cache">
```
上面的一句html代码指明了浏览器不能对该页面进行缓存。  

2 http协议的缓存机制  
我将http协议的缓存机制归类为两组，一组是用于记录缓存的有效时间；另外一组则是用于记录缓存判断。  
1) 有效时间   
在http头部有两个记录缓存的有效时间，分别是cache-control中的max-age和expires
其中max-age是http1.1协议的，而expires则是http协议1.0的。所以当这两者的事件发生冲突的时候，max-age的优先级高于expires。  

2) 缓存判断类型  
A Last_Modified(response) ==> If_Modified_Since(request)  
B ETag(response) ==> If_None_Match(request)  
> 其中Last_Modified和If_Modified_Since是http1.0协议中的，而ETag和If_None_Match是http1.1协议中的。这两组都可以用于判断缓存是否已经过期。不同的是ETag是根据缓存文本内容生成的hash值，是服务器资源的唯一标识。根据缓存是否过期的判断，http请求的返回码一般是200或者304。  

> 为什么有了Last_Modified还需要ETag字段呢？主要的原因有以下三个：  
> 1) Last_Modified只能精确到秒级，对于经常秒级以下修改的文件，则需要通过ETag判断是否已经发生变化；  
> 2) 很多时候会出现Last_Modified时间发生变化，而实际上文件的文本内容并没有发生变化的情况，这种时候一样需要通过ETag进行判断；  
> 3) 对于部分浏览器没能给资源加上准确的Last_Modified时间   

3 使用web缓存构建站点  
1) 对静态资源(js css 图片)设置一个比较长时间的缓存  
2) 减少对cookie的依赖，因为http每次请求和响应都会带上cookie  
3) 对于静态资源，可以利用ajax缓存  
4) 对于一些安全级别不高的可以减少https协议的请求，因为https协议是不能进行缓存的  
5) 适当增加get请求，post请求不能进行缓存  

###### http put请求和get请求的区别
这里顺便总结下http协议中get请求和put请求之间的区别。根据自己的理解，主要有以下的几点不同：  
1) get请求一般用于获取/查询资源，而post请求一般用于更改数据。  
> 在这里get请求有一个术语叫做安全幂等。其中安全主要指的是没有对资源做出修改；而幂等的意思则是同个url多次请求等到的数据是一样的。  

2) get请求会将数据直接放在http请求的后面，直接表现就是在url后面跟着一串参数，参数和url之间用?隔开，而参数之间则是使用&连接；post请求则是将数据放置在http请求包中。    
3) get请求的数据大小一般都有限制，1024字节；而post请求则没有限制传输的数据大小。  
4) get请求的安全性比post低，因为get请求直接将参数暴露出来。  

##### HTML5Web缓存
html5中提供了三种缓存方法，分别是  
1 离线缓存mainfest  
2 LocalStorage  
3 SessionStorage  
