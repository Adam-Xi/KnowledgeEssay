1. **“GET使用URL或Cookie传参，而POST将数据放在BODY中”，这个是因为HTTP标准对HTTP协议用法的约定**

2. **“GET方式提交的数据有长度限制，则POST的数据则可以非常大”，这个是因为它们使用的操作系统和浏览器设置的不同引起的区别**

   使用GET通过URL提交数据，则GET可以提交的数据量就和URL的长度有直接关系。而实际上，URL不存在参数上限的问题，HTTP规范并没有对URL的长度进行限制。这个限制是特定的浏览器即服务器对它的限制(整个URL长度，并不仅仅是参数值数据长度)

   对服务器来说，URL长了，对服务器处理也是一种负担，防止有的恶意构造的URL，所以多数服务器会给URL长度加限制(针对所有HTTP请求的，与GET、POST没关系)

3. **“POST比GET私密(安全)，因为数据在地址栏上不可见”，这个说法没毛病**

   通过GET提交数据，用户名和密码将明文出现在URL上，因为登录页面有可能被浏览器缓存，其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了，除此之外，使用GET提交数据还可能会造成Cross-site request forgery攻击 



GET和POST最大的区别：（好像和幂等性有关系）

+ **GET产生一个TCP数据包**：对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）
+ **POST产生两个TCP数据包**：对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）

