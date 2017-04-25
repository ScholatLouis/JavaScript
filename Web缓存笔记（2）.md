### Web缓存笔记（2）
之前写过关于web缓存的粗略笔记，现在谈谈cookie和HTML5提供的localStorage & sessionStorage

#### cookie
cookie的大小限制一般为4KB，是网景公司于1993年发明，主要用于记录保存登录信息，存在与浏览器端。

#### localStorage & sessionStorage
localStorage和sessionStorage是HTML5新进入的技术。对于localStorage并不是什么新的技术，在HTML5之前IE就有使用过userData作为本地存储。而sessionStorage与localStorage类似，主要是两者的生存周期不同，localStorage是存储与本地，与HTTP会话无关；而sessionStorage则是一个会话的存储周期。

#### cookie localStorage sessionStorage三者区别
| feature | cookie | localStorage | sessionStroage | 
| --- | --- | --- | ---  |
| 数据的生命周期 | 一般是由服务器生成，可以设置失效时间。| 除非被清除，否则一直存在 | 仅在当前会话下失效，关闭浏览器泽清除  |
| 大小限制 | 4KB左右 | 一般为5MB | 一般为5MB左右 |
| 与服务器通信 | 每次都会携带在HTTP头重，如果使用cookie保存过多数据会带来性能问题 | 仅在客户端中存在，不参与和服务器的通信 | 不参与服务器通信 |

#### 安全性考虑
需要注意的是cookie，localStorage，sessionStorage三者都可以通过前台浏览器console访问，具有XSS注入的风险，所以一般不用来存储系统的敏感信息。

##### 参考文章
[1] http://jerryzou.com/posts/cookie-and-web-storage/  
[2] http://www.admin10000.com/document/7097.html
