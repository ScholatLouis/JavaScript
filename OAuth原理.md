### OAuth原理
##### OAuth
OAuth: Open Authorization开放授权，允许用户通过授权让第三方应用获取用户的个人信息。在没有OAuth协议之前，如果需要在A应用中获取用户在B应用中的个人信息（如好友、头像、昵称等），则需要用户在A应用中输入用户在B应用的个人账号和密码，以此来获取B应用中用户的信息。这种方式存在用户账户密码安全性的问题。有了OAuth协议之后，同样的场景只需要用户在B应用中对A应用进行授权，即可让A应用获取用户在B应用中个人信息。简单的流程图如下：
![OAuth简易流程](http://jbcdn2.b0.upaiyun.com/2013/10/oauth_developer_1.jpg)
> 1，2：用户访问第三方网站，第三方网站会引导用户到授权方授权页面进行登录（如：QQ、WeChat）  
> 3：用户登录授权方，并点击确认授权  
> 4：第三方web获取用户在授权方的个人信息  

##### 获取微信OAuth2.0登录授权
OAuth2.0有四种授权方式，分别是：
> 授权码模式(authorization code)  
> 简化模式(implicit)  
> 密码模式(resource owner password credentials)  
> 客户端模式(client credentials)  

微信OAuth2.0授权使用的是授权码模式，因此这里主要了解微信授权的流程。主要过程有四步，如下：
> 1 引导用户进入授权页面授权，获取code  
 
授权页面的链接为：https://open.weixin.qq.com/connect/oauth2/authorize?appid=APPID&redirect_uri=REDIRECT_URI&response_type=code&scope=SCOPE&state=STATE#wechat_redirect
其中appid是第三方web应用申请的id  
redirect_uri是授权成功之后的跳转页面  
response_code这里的值是code，因为微信采用的是授权码模式  
scope则是应用授权作用域，有两个值分别时snsapi_base和snsapi_userinfo。其中snsapi_base是不弹出授权页面，直接跳转到redirect_uri，并获取用户的openid；而snsapi_userinfo会弹出授权框，用户授权之后能获取到用户的昵称、头像等信息。授权弹框如下：    
![OAuth授权弹框](https://github.com/ScholatLouis/BlogImg/blob/master/OAuth_getUserInfo.png)  
\#wechat_redirect是必须带上的参数  

> 2 通过code获取网页授权access_token  
> 3 如果需要可以通过刷新网页授权access_token避免过期  
> 4 通过网页授权access_token和openid获取用户的基本信息  

#### 参考文章
[1]http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html#.E7.AC.AC.E4.B8.80.E6.AD.A5.EF.BC.9A.E7.94.A8.E6.88.B7.E5.90.8C.E6.84.8F.E6.8E.88.E6.9D.83.EF.BC.8C.E8.8E.B7.E5.8F.96code  
[2]https://github.com/navyxie/oauth2-wechat-develop  
[3]https://www.ibm.com/developerworks/cn/java/j-lo-oauth/  
[4]http://www.jikexueyuan.com/course/501.html  
[5]http://blog.jobbole.com/49211/  
