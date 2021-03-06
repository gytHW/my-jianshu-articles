大家都知道：*HTTPS = HTTP + SSL/TSL* 也就是，https是在http基础上加了一层加密。
这也由此衍生出一个问题 —— https的加密用的是对称加密还是非对称加密，是否全程都是用的一种加密方式？
这个问题引起了我的兴趣，去查阅了下资料，了解了一下，这里做个简单的总结备忘。

### 1. 什么是对称加密和非对称加密？
这个详细来说有点复杂，这里简单的说下：
> **对称加密**就是加密和解密用的是同一个密钥k。
> **非对称加密**是发送端使用公开的公钥a加密，然后接收端使用私密的私钥b解密。

对称加密快，非对称加密安全。对称加密如**DES**，非对称加密如**RSA**。

***

### 2. 那https的加密是用的对称加密还是非对称加密呢？
答案是——**两者都用了**

这里让我们先看看一个完整的https请求的过程：
1. 首先，浏览器请求一个url，找到服务器，向服务器发起一个请求。服务器将自己的证书(包含服务器公钥S_PuKey)、对称加密算法种类及其他相关信息返回客户端。
2. 浏览器检查CA证书是不是由可以信赖的CA机构颁发的，确认证书有效和此证书是此网站的。如果不是，给客户端发一个警告，询问是否继续访问。
3. 如果是，客户端使用公钥加密了一个随机对称密钥，包括加密的URL一起发送到服务器 
4. 服务器用自己的私匙解密了你发送的钥匙。然后用这把对称加密的钥匙给你请求的URL链接解密。
5. 服务器用你发的对称钥匙给你请求的网页加密。你也有相同的钥匙就可以解密发回来的网页了。

说起来复杂，简单总结起来就是：
> **使用非对称加密传输一个对称密钥K**，让服务器和客户端都得知。然后两边都**使用这个对称密钥K来加密解密收发数据**。因为**传输密钥K是用非对称加密方式**，很难破解比较安全。而**具体传输数据则是用对称加密方式**，加快传输速度。两全其美。

这么想来，https的设计真的是一个非常棒的设计，同时兼顾了安全和速度，可以说是比较完善了。

***
参考资料：
1. [理解SSL（https）中的对称加密与非对称加密](https://www.cnblogs.com/hai-blog/p/8311671.html)
2. [聊聊对称/非对称加密在HTTPS中的应用](https://zhuanlan.zhihu.com/p/34732244)
