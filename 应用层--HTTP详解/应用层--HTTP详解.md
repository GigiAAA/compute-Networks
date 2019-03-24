#### **应用层**：我们程序员写的一个个解决我们实际问题，满足我们日常需求的网络程序，都在应用层。

**成熟的应用层协议**

1.HTTP协议：

2.HTTPS协议：

3.FTP协议：用于文件传输

4.DNS协议：用于域名解析

5.SSH协议：用于远程登陆

.......

**应用层除了成熟的应用层协议之外，还存在大量的用户自定制的协议。**

#### **用户自定制协议方法：**

举例：假如此时要实现一个网络版本的计算器。

![1](C:\Users\14665\Desktop\网络基础二（重点）\1.png)

#### HTTP协议（超文本传输协议）（面试最重要）

#### URL（完整的URL必须以https://开头）

平时我们所说的“网址”其实就是说的URL

例如：https://www.baidu.com/

但www.baidu.com不是URL，因为它缺少了前面的https://。

完整的URL格式

![2](C:\Users\14665\Desktop\网络基础二（重点）\2.png)

#### urlencode（转义）和urldecode（解码 可通过百度转义工具进行转换）

键值对中特殊字符和汉字等都需要转义。

#### HTTP协议格式

fiddler工具 抓包

![3](C:\Users\14665\Desktop\网络基础二（重点）\3.png)

抓包后用记事本打开，若能看懂则为文本文档（ASCII码），看不懂为二进制文档（二进制数据）。

#### HTTP为文本协议

#### **HTTP请求报文格式**

```
//fiddler抓包举例
//首行
GET http://stdl.qq.com/stdl/daohang/daily/website-ads-2019-3-21.json?time=1553164544656 HTTP/1.1
//协议头部header
Host: stdl.qq.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.25 Safari/537.36 Core/1.70.3650.400 QQBrowser/10.4.3341.400
Accept: */*
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: pgv_pvid=3300322480; RK=r5xBUCb9Vv; ptcz=cf4fb94c6b4b41b4f7aa0198290808d7cc7ab1f6f08feae6ef935471504687ed; pgv_info=ssid=s9720453277
//空行
```

1.首行（请求行）

a)方法（GET/POST）

b)URL

c)版本号(HTTP/1.0 1.1 2.0)

2.协议头部header**（具体有多少行不确定，故用空行表示header结束）**（请求报头）

每一行是一个键值对，建和值之间使用“： ”来分割

3.空行

协议头的结束标记

4.正文body

通常情况下 GET方法对应的请求没有body，POST方法对应的请求有body。（HTTP请求协议主要为GET和POST方法。其中大部分为GET，小部分为POST）

#### body部分格式可能和URL中的参数部分是一样的。（若Content-Type: application/x-www-form-urlencoded，则格式一样    Content-Length: 28   body长度）

```
//POST方法
POST http://123.207.58.25/login/userLogin HTTP/1.1
Accept: */*
Content-Type: application/x-www-form-urlencoded
Referer: http://123.207.58.25///当前页面是从哪个页面跳转过来的
Accept-Language: zh-CN
Accept-Encoding: gzip, deflate
Content-Length: 28//body长度
Host: 123.207.58.25//本机域名IP地址
Connection: Keep-Alive
Pragma: no-cache
//声明用户的操作系统和浏览器版本信息
User-Agent: Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko Core/1.70.3650.400 QQBrowser/10.4.3341.400
//用于在客户端存储少量信息，通常是先会话功能（服务器返回给浏览器的字符串）
Cookie: PHPSESSID=h0s9gsnl8jcnogsk3k7korg621//用户身份信息

name=1546&passwd=18291740176//body（用户名密码）
```

#### **HTTP响应报文格式**

#### 状态码常见：200   404(无法访问)  403(没有权限访问)

```
HTTP/1.1//版本号 200//状态码 OK//状态码描述
Server: nginx/1.12.2
Date: Thu, 21 Mar 2019 11:35:03 GMT
Content-Type: text/html; charset=UTF-8
Transfer-Encoding: chunked
Connection: keep-alive
X-Powered-By: PHP/5.4.16
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Pragma: no-cache

7410

```

1.首行（响应行）

a)版本号

b)状态码(200 404 403)

c)状态码描述信息

2.协议头部header部分（响应报头）

每一行是一个键值对

每个键值对的建和值之间用“： ”分割

3.空行

header部分结束

4.正文 body(Content-Type: text/html; charset=UTF-8)长度一般情况下存在，特定情况下没有

html

### 协议基本功能（为了更好的封装、分用）

（1）任何一层协议都必须能够将自己的协议报头与有效载荷进行分离；

**HTTP是如何做到将协议报头跟有效载荷分离开来？**

**区分协议报头和有效载荷唯一符号为空行。**报头长度可通过空行知道（因空行是报头结束的标志）。

那么若此协议有有正文，空行读取结束后读取的就为正文，那么怎么**确定正文的长度**呢？

**可通过报头中的Content-Length来确定正文的长度（字节）。**

**可通过报头中的Content-Type来确定正文类型。**

（2）任何一层协议必须要能够决定将自己的有效载荷上交给哪一个协议。



### HTTP常见方法

最常用方法为GET和POST方法。

#### 故极大概率会出现面试题：解释GET方法与POST方法的区别？

回答：取决于用户将自定义数据放置在URL中还是body中，若在URL中即为GET方法，body中即为POST方法。（最明显区别）除此之外GET与POST方法没有区别，即GET可实现的方法POST方法均能实现，POST方法可实现的方法GET方法也可以实现，只是有些场景有些细微区别，比如网页中登陆页面，推荐使用POST方法，因若使用GET方法的话，用户名密码将会出现在网址上，此种情况下其实使用GET方法或者POST方法（用户名密码可通过抓包获取）安全性基本相同，只不过使用POST方法会提高用户体验度。

### HTTP的状态码

最常见状态码，比如200(ok),404(Not Found),403(Forbidden),302(Redirect 重定向)

**重定向：类似于呼叫转移（从老电话号码自动转接到新电话号码）**

**永久性重定向**：第一次访问时从访问页面跳转至新页面，以后访问时就直接访问新页面即可。（应用场景：以前服务器承载人数有限，用户人数增多后需要新的服务器）

**临时性重定向**：每次访问都需要从访问页面跳转至新的页面。（应用场景：某个服务器在非停机维护时，需要暂时将此页面跳转至新页面，维护结束后结束重定向，直接访问该页面即可，不需要跳转）

**那么为什么要使用重定向功能而非直接使用新的服务器呢？**

原因是因为为了照顾老用户的使用，有了重定向后相当于明确告诉老用户原来访问页面跳转至新页面，但若直接使用新的服务器会导致老用户极大可能找不到原有访问页面，体验感较差。

**重定向时需要知道因素：**

**location：后面跟新的URL（新页面网址）**

**404错误是客户端错误，是因为用户请求的信息客户端上不存在。**

##### 303状态码协议头部最后一行Location行为新的地址。

![4](C:\Users\14665\Desktop\网络基础二（重点）\4.png)

![5](C:\Users\14665\Desktop\网络基础二（重点）\5.png)

![6](C:\Users\14665\Desktop\网络基础二（重点）\6.png)

​                                                                               4xx:客户端错误 

![7](C:\Users\14665\Desktop\网络基础二（重点）\7.png)

​                                              ![8](C:\Users\14665\Desktop\网络基础二（重点）\8.png)

​                                                                              5xx:服务器错误 

![9](C:\Users\14665\Desktop\网络基础二（重点）\9.png)

#### HTTP常见Header

1.Content-Type:数据类型（text/html等）

2.Content-Length：Body的长度

3.Host:客户端告知服务器，所请求的资源是在哪个主机的哪个端口上

4.User-Agent:声明用户的操作系统和浏览器版本信息

5.referer:当前页面是从哪个页面跳转过来的

6.location:搭配3XX状态码使用，告诉客户端接下来要去哪里访问（重定向）

#### 7.Cookie：用于在客户端存储少量信息。通常用于实现会话(session)的功能（服务器返回给浏览器的字符串）（重点掌握）

服务器借助Cookie识别用户信息，当用户新安装一个浏览器时，第一次访问浏览器即向服务器发送请求，服务器响应返回一个Set-Cookie字符串（其中包含用户身份标识），以后每次用户使用浏览器访问（即每次向服务器发送请求）时均会携带Cookie字符串，服务器以此来识别用户。

##### Cookie特点

1.Cookie字段根据每个域名会保存一份（例如百度和淘宝同一用户会有两个身份标识，但在同一家互联网公司旗下的所有产品中为一个用户同一身份标识）。

2.Cookie长度有限（一般只保留用户ID），有些流浪其长度上限为4K，更加复杂的信息（如浏览记录等）要根据用户ID到服务器中查询。

3.Cookie保存的用户标识一般都有时间期限，过期就要求用户重新登陆，提高安全性。

查询Java中如何实现一个登录功能。

![10](C:\Users\14665\Desktop\网络基础二（重点）\10.png)

### HTTPS就是在HTTP协议上进行了加密。

