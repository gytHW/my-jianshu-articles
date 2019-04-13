今天工作中碰到一个很神奇的bug，花了很多时间，最后才找到问题，特此记录下防止后人踩坑。

### 问题描述
在处理一个接口的报错，定位到问题是根据简历id(```tob_resume_id```)获取简历详情的时候没能获取到数据，这是很不正常的，因为其他地方都能正常获取。经过比对，发现是传的tob_resume_id不对，为何不对，因为返回简历id的接口返回的简历id就是错的。
![接口返回的tob_resume_id](https://upload-images.jianshu.io/upload_images/1038472-ecd992ed616c51e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
于是去看代码，发现```tob_resume_id```是从表中查出来的，用sql一查，发现数据库查出来的id是正确的！
![mysql查出来的tob_resume_id](https://upload-images.jianshu.io/upload_images/1038472-0f6fd0de606c1115.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就很奇怪了，于是去看代码逻辑，发现并没有对这个id进行过处理，最后直接return返回了。也就是说理论上前端拿到的应该就是数据库查到的1227结尾的id。ssh到服务器上，打断点进行调试，发现直到最后return前的最后一步，data中的tob_resume_id还是1227，那为何结果到了页面上的接口返回值中就变成了1200呢？百思不得其解。

### 解决过程
考虑过是否有缓存，有中间件进行处理，经过考虑之后一一否决。最终只能想到和其他也返回tob_resume_id的接口进行比对，看看到底是哪里有不同。然后发现了一个很细微的不同——
![tob_resume_id是字符串！](https://upload-images.jianshu.io/upload_images/1038472-b2506b64ad200fc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**此接口返回的```tob_resume_id```是字符串，而上一个接口返回的是整型！**

抱着这个怀疑，我询问了一名老司机同事，在他的确认下我才知道：

##### JSON的数字类型是有最大值的！
##### JS在处理超出最大值的数字时会进行截断！所以此时需要把数字转成字符串才能正确显示！
这真的是我以前不知道的知识点了，原来json还有这种特性。
在使用类型转换在return之前将```tob_resume_id```转成字符串之后，问题解决了，接口开始返回正确的值了。



