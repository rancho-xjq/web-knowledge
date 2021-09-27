1. DNS的作用有哪些？（选项是域名解析、防火墙、负载均衡、控制流量还有一个我不记得了）

2. 以下关于POST请求说法正确的是？（POST请求只能通过body带参数、服务器一定能够收到POST请求发送的数据、POST请求发送了两个数据包、POST请求可以被缓存）

3. [h]浏览器为什么会跨域？怎么解决跨域？
参考： [彻底理解浏览器的跨域](https://juejin.cn/post/6844903816060469262)
       [10种跨域解决方案（附终极大招）](https://juejin.cn/post/6844904126246027278)

4. 如何实现ajax请求
通过实例化一个XMLHttpRequest对象得到一个实例，调用实例的open方法为这次 ajax请求设定相应的http方法、相应的地址和以及是否异步，当然大多数情况下我们都是选异步， 以异步为例，之后调用send方法ajax请求，这个方法可以设定需要发送的报文主体，然后通过 监听readystatechange事件，通过这个实例的readyState属性来判断这个ajax请求的状态，其中分为0,1,2,3,4这四种 状态，当状态为4的时候也就是接收数据完成的时候，这时候可以通过实例的status属性判断这个请求是否成功

``` html
var xhr = new XMLHttpRequest();
xhr.open('get', 'aabb.php', true);
xhr.send(null);
xhr.onreadystatechange = function() {
  if(xhr.readyState==4) {
    if(xhr.status==200) {
      console.log(xhr.responseText);
    }
  }
}
```

4. [h]同源策略是什么？
同源策略是指只有具有相同源的页面才能够共享数据，比如cookie，同源是指页面具有相同的协议、域名、端口号，有一项不同就不是同源。
有同源策略能够保证web网页的安全性。

参考：

[前端必备HTTP技能之同源策略详解](http://www.jianshu.com/p/beb059c43a8b)

[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)

[浏览器的同源策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)

5. 请说一下实现jsonp的实现思路？
jsonp的原理是使用script标签来实现跨域，因为script标签的的src属性是不受同源策略的影响的，因此可以使用其来跨域。一个最简单的jsonp就是创建一个script标签，设置src为相应的url，在url之后添加相应的callback，格式类似于
url?callback=xxx，服务端根据我们的callback来返回相应的数据，类似于res.send(req.query.callback + '('+ data + ')')，这样就实现了一个最简单的jsonp

参考：

[动手实现一个JSONP]()

[jsonp的原理与实现](https://segmentfault.com/a/1190000007665361)

[fetch-jsonp源码](https://github.com/camsong/fetch-jsonp/blob/master/src/fetch-jsonp.js)

5. 页面从输入URL到展现发生了什么


参考：

[浏览器内核、JS 引擎、页面呈现原理及其优化](https://www.zybuluo.com/yangfch3/note/671516)

[HTTP权威指南](https://book.douban.com/subject/10746113/)

[说一说从输入URL到页面呈现发生了什么？——网络篇](https://juejin.cn/post/6844904021308735502#heading-24)

7. TCP三次握手的过程，为什么是三次而不是两次或者四次？
第一次握手：客户端发送一个syn（同步）包（syn=x）给服务器，进入SYN_SEND状态，等待服务器确认
第二次握手：服务端收到客户端发送的同步包，确认客户端的同步请求（ack=x+1）,同时也发送一个同步包， 也就是一个ACK包+SYN包服务器进入SYN_RECV状态
第三次握手：客户端收到服务器的SYN+ACK包，向服务器发送一个确认包，此包发送完毕，客户端和服务器进入 ESTABLISHED状态，完成三次握手
不是两次是为了防止已经失效的连接请求报文段突然又传送到了服务端，因而产生错误，比如有一个因网络延迟的请求 发送到了服务端，服务端收到这个同步报文之后进行确认，如果此时是两次握手，那么此时连接建立，但是客户端并没有发出 建立连接的请求，服务端却一直等待客户端发送数据，这样服务端的资源就白白浪费了。
不是四次的话是因为完全没有必要，三次已经足够了

参考：

[计算机网络之面试常考](https://www.nowcoder.com/discuss/1937)

[tcp为什么要三次握手，而不能二次握手？](http://blog.csdn.net/xumin330774233/article/details/14448715)

[TCP 为什么是三次握手，为什么不是两次或四次？](https://www.zhihu.com/question/24853633)

8. TCP的四次挥手
第一次：主动关闭方发送一个FIN包，用来关闭主动关闭方到被动关闭方的数据传送，也就是告诉另一方我不再发送数据了，但此时仍可以接收数据

第二次：被动关闭方收到FIN包之后，发送一个确认（ACK）包给对方

第三次：被动关闭方发送一个FIN包，告诉对方不带发送数据

第四次：主动关闭方收到FIN包之后，发送一个ACK包给对方，至此完成四次挥手

9. HTTP报文的格式，传输中以何种方式传输
HTTP报文分为三个部分，起始行、首部和主体，其中起始行和首部以一个回车和换行符分隔，首部和主体以一个空行分隔，其中起始行是对这次HTTP请求或者响应 的描述，请求报文的起始行包括使用的HTTP方法、请求的url地址、HTTP版本，响应报文的起始行包括HTTP的版本，HTTP状态码，http状态码的描述，首部也 就是常说的HTTP头部，如Date、Cookie、Content-Type等，主体是这次请求或响应的数据，传输中以明文传输

10. 常见的HTTP头部
可以将HTTP首部分为通用首部、请求首部、响应首部、实体首部，通用首部表示一些通用信息，如Date表示报文创建时间，请求首部就是请求报文中 独有的，如cookie、和缓存相关的If-Modified-Since，响应首部就是响应报文中独有的，如set-cookie和重定向有关的location，实体首部用来 描述实体部分，如Allow用来描述可执行的请求方法，Content-Type描述主体类型，Content-Encoding描述主体的编码方式

11. HTTP状态的简要分类
可以按照HTTP状态码的第一个数字分类，1xx表示信息，2xx表示成功，3xx表示重定向，这里需要注意的是304，表示未修改， 4xx表示客户端错误，最常见的是404，5xx表示服务端错误

12. HTTP状态码101、200、301、302、304的具体含义
101：切换协议 200：正常，OK，301：永久重定向，302：临时重定向，304：未修改

13. 简要介绍一次302的过程
用户请求一个url，服务器处理这个url，设置这个url需要重定向，返回用户一个302响应，并在http头部设置location字段为新的地址， 浏览器得到这个响应，根据location中新的地址重新发起一次请求。

14. 用户登陆过程的简要说明，如何判断用户是否登录？
用户输入用户名和密码，通过post请求将密码和用户名发送给服务器，服务器比对收到的用户名、密码和数据库中的数据进行比对，不一致则做出响应， 反馈信息给客户端，如果比对一致则服务端生成一个session，这个session可以存储在内存、文件、数据库中，同时生成一个与之一一对应的sessionID 作为cookie发送给客户端，比对成功之后反馈信息，这时一般会进行一次重定向，重定向至登陆之后的默认页面。判断用户登录则是根据这个sessionID，每次请求 会先检查有没有这次类似sessionID的cookie发送过来，没有则认为没有登录，有则是否有相应的session，这个session是否过期等，来判断用户是否登录， 登录是否过期。

15. udp的阻塞机制，如何处理

16. get和post有什么区别？什么时候用get，什么时候用post
get用来请求数据，post用来提交数据，form表单使用get时，数据会以querystring形式存在url中，因而不够安全也存在数据大小限制，而post不会，post将数据 存放在HTTP报文体中，获取数据应该使用get，提交数据应该使用post。

参考：

[get和post区别？](https://www.zhihu.com/question/28586791)

[浅谈HTTP中Get与Post的区别](http://www.cnblogs.com/hyddd/archive/2009/03/31/1426026.html)

[GET和POST有什么区别？及为什么网上的多数答案都是错的。](http://www.cnblogs.com/nankezhishi/archive/2012/06/09/getandpost.html)

[也谈 GET 和 POST 的区别](http://www.cnblogs.com/ldp615/archive/2012/07/27/http-get-post.html)

17. 介绍一下HTTPS的连接过程

18. 什么是正向代理？什么是反向代理？
正向代理就是客户端向代理服务器发送请求，并且指定目标服务器，之后代理向目标服务器转交并且将获得的内容返回给客户端。比如翻墙
反向代理的话代理会判断请求走向何处，并将请求转交给客户端，客户端只会觉得这个代理是一个真正的服务器。如负载均衡。

[正向代理和反向代理的区别](https://zhuanlan.zhihu.com/p/25423394)


20. 介绍一下HTTPS的连接过程


21. 和缓存有关的请求头有哪些？优先级是怎样的？
和缓存有关的请求头有Cache-Control、If-Match、If-None-Match、If-Modified-Since、If-Unmodified-Since，在缓存中
总体来说是Cache-Control优先于Expires，Cache-Control中会需要检测Cache-Control是否过期，过期的话检验会优先检测Etag，
也就是If-Match、If-None-Match,不一致则验证Last-Modify请求头也就是If-Modified-Since、If-Unmodified-Since。

参考：

[HTTP缓存控制小结](http://www.imweb.io/topic/5795dcb6fb312541492eda8c)

22. 介绍一下DNS的查找过程？


23. http连接性能优化，长连接，keep-alive

24. https的详细过程，使用的加密算法，是对称加密算法还是非对称加密算法。md5、SHA、AES分别是对称加密的还是非对称加密的

25. https没有大规模应用的原因

26. http2有哪些新特性？

27. https 实现原理（越详细越好）

28. 缓存有哪些，区别是什么

29. CDN原理讲一下

30. 用户输入url到页面展示经历了哪些步骤

31. 刚刚提到 TCP 的三次握手，其中 https（s是什么？在 tcp 层的起了什么作用）

32. 为什么canvas的图片为什么有跨域问题?
29. 什么是跨域？都有哪些方式可以进行跨域？
跨域就是不同域名下的通信来往，跨域方式：

- jsonp 请求
- HTML5新规范的CORS功能,只要目标服务器返回请求头部Access-Control-Allow-Origin: * 即可
- 通过内部服务器代理，实现跨域
- <img>,<script>,<link>,<iframe>，通过src，href属性设置为目标url,实现跨域请求

30. HTTPS的连接是什么样的？

31. 比如说百度的一个服务不想让阿里使用，如果识别到是阿里的请求，然后跳转到404或者拒绝服务之类的

32. 304缓存，有了Last-Modified，为什么还要用ETag？有了Etag，为什么还要用Last-Modified？Etag一般怎么生成？

33. 如何劫持https的请求，提供思路
很多人在google上搜索“前端面试 + https详解”，把答案倒背如流，但是问到如何劫持https请求的时候就一脸懵逼，是因为还是停留在https理论性阶段。

想告诉大家的是，就算是https，也不是绝对的安全，以下提供一个本地劫持https请求的简单思路。

模拟中间人攻击，以百度为例

先用OpenSSL查看下证书，直接调用openssl库识别目标服务器支持的SSL/TLS cipher suite

openssl s_client -connect www.baidu.com:443
用sslcan识别ssl配置错误，过期协议，过时cipher suite和hash算法

sslscan -tlsall www.baidu.com:443
分析证书详细数据

 sslscan -show-certificate --no-ciphersuites www.baidu.com:443
生成一个证书

openssl req -new -x509 -days 1096 -key ca.key -out ca.crt
开启路由功能

sysctl -w net.ipv4.ip_forward=1
写转发规则，将80、443端口进行转发给8080和8443端口

iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080 
iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-ports 8443
最后使用arpspoof进行arp欺骗

34.  jsonp和CORS那个更安全？jsonp有什么安全问题？为什么有这些安全问题？

35. OSI七层协议？

36. https使用上有什么注意点？

37. https和http性能有什么区别？

38. ajax如何实现、readyState五中状态的含义

39. jsonp如何实现
基本原理：主要就是利用 script 标签的src属性没有跨域的限制，通过指向一个需要访问的地址，由服务端返回一个预先定义好的 Javascript 函数的调用，并且将服务器数据以该函数参数的形式传递过来，此方法需要前后端配合完成。
执行过程：

前端定义一个解析函数(如: jsonpCallback = function (res) {})
通过params的形式包装script标签的请求参数，并且声明执行函数(如cb=jsonpCallback)
后端获取到前端声明的执行函数(jsonpCallback)，并以带上参数且调用执行函数的方式传递给前端
前端在script标签返回资源的时候就会去执行jsonpCallback并通过回调函数的方式拿到数据了。

缺点：

只能进行GET请求

优点：

兼容性好，在一些古老的浏览器中都可以运行

代码实现：
（具体可以参考这篇文章：[JSONP原理及实现](https://github.com/LinDaiDai/niubility-coding-js/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/%E8%B7%A8%E5%9F%9F/JSONP%E5%8E%9F%E7%90%86%E5%8F%8A%E5%AE%9E%E7%8E%B0.md)）
``` js
<script>
    function JSONP({
        url,
        params = {},
        callbackKey = 'cb',
        callback
    }) {
        // 定义本地的唯一callbackId，若是没有的话则初始化为1
        JSONP.callbackId = JSONP.callbackId || 1;
        let callbackId = JSONP.callbackId;
        // 把要执行的回调加入到JSON对象中，避免污染window
        JSONP.callbacks = JSONP.callbacks || [];
        JSONP.callbacks[callbackId] = callback;
        // 把这个名称加入到参数中: 'cb=JSONP.callbacks[1]'
        params[callbackKey] = `JSONP.callbacks[${callbackId}]`;
        // 得到'id=1&cb=JSONP.callbacks[1]'
        const paramString = Object.keys(params).map(key => {
            return `${key}=${encodeURIComponent(params[key])}`
        }).join('&')
        // 创建 script 标签
        const script = document.createElement('script');
        script.setAttribute('src', `${url}?${paramString}`);
        document.body.appendChild(script);
        // id自增，保证唯一
        JSONP.callbackId++;

    }
    JSONP({
        url: 'http://localhost:8080/api/jsonps',
        params: {
            a: '2&b=3',
            b: '4'
        },
        callbackKey: 'cb',
        callback (res) {
            console.log(res)
        }
    })
    JSONP({
        url: 'http://localhost:8080/api/jsonp',
        params: {
            id: 1
        },
        callbackKey: 'cb',
        callback (res) {
            console.log(res)
        }
    })
</script>
```

40. 怎么处理跨域

41. restful的method解释

42.  Http请求中的keep-alive有了解吗。
答案一：
在http早期，每个http请求都要求打开一个tpc socket连接，并且使用一次之后就断开这个tcp连接。 使用keep-alive可以改善这种状态，即在一次TCP连接中可以持续发送多份数据而不会断开连接。通过使用keep-alive机制，可以减少tcp连接建立次数，也意味着可以减少TIME_WAIT状态连接，以此提高性能和提高httpd服务器的吞吐率(更少的tcp连接意味着更少的系统内核调用,socket的accept()和close()调用)。 但是，keep-alive并不是免费的午餐,长时间的tcp连接容易导致系统资源无效占用。配置不当的keep-alive，有时比重复利用连接带来的损失还更大。所以，正确地设置keep-alive timeout时间非常重要。
答案二：
Keep-Alive是HTTP的一个头部字段Connection中的一个值，它是保证我们的HTTP请求能建立一个持久连接。也就是说建立一次TCP连接即可进行多次请求和响应的交互。它的特点就是只要有一方没有明确的提出断开连接，则保持TCP连接状态，减少了TCP连接和断开造成的额外开销。
另外，在HTTP/1.1中所有的连接默认都是持久连接的，但是HTTP/1.0并未标准化。

43. fetch取消

44. https获取加密密钥的过程

45. http的方法有哪几种,每种方法的有用途

46. http握手原理

47. http请求幂等性

48. 前端会话机制

49. https的过程？

50. http请求的报文头部是什么？

51. https和http的区别
HTTPS和HTTP的区别主要如下：
1、https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。

2、http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。

3、http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。

4、HTTP在传输过程中使用的是明文 传输，内容可能被窃取，而且无法验证通信方的身份，还有可能遭遇身份伪装，HTTPS协议是由SSL+HTTP协议构建的可进行加密传输，HTTPS在应用层和传输层之间增加了ssl协议用来加密 内容，因此通过证书验证来验证身份，即使数据被窃取也无法解密，数据的传输更加安全。

52. https的缺点
虽然说HTTPS有很大的优势，但其相对来说，还是存在不足之处的：

（1）HTTPS协议握手阶段比较费时，会使页面的加载时间延长近50%，增加10%到20%的耗电；

（2）HTTPS连接缓存不如HTTP高效，会增加数据开销和功耗，甚至已有的安全措施也会因此而受到影响；

（3）SSL证书需要钱，功能越强大的证书费用越高，个人网站、小网站没有必要一般不会用。

（4）SSL证书通常需要绑定IP，不能在同一IP上绑定多个域名，IPv4资源不可能支撑这个消耗。

（5）HTTPS协议的加密范围也比较有限，在黑客攻击、拒绝服务攻击、服务器劫持等方面几乎起不到什么作用。最关键的，SSL证书的信用链体系并不安全，特别是在某些国家可以控制CA根证书的情况下，中间人攻击一样可行。

53. http option的作用

1、获取服务器支持的HTTP请求方法；

2、用来检查服务器的性能。

其实在正式跨域之前，浏览器会根据需要发起一次预检（也就是option请求），用来让服务端返回允许的方法（如get、post），被跨域访问的Origin（来源或者域），还有是否需要Credentials(认证信息)等。

54. TCP与UDP的区别
1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接

2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付 3、TCP面向字节流，实际上是TCP把数据看成一连串无结构的字节流;UDP是面向报文的 UDP没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低（对实时应用很有用，如IP电话，实时视频会议等）4、每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信 5、TCP首部开销20字节;UDP的首部开销小，只有8个字节

6. TCP的逻辑通信信道是全双工的可靠信道，UDP则是不可靠信道

7. http在哪一层，tcp呢

8. 怎么与服务端保持连接

9. cookie有哪些属性

10. 怎么禁止js访问cookie

11. 文件上传如何做断点续传

12. 表单可以跨域吗

13. 搜索请求中文如何请求

14. Hhttp1.1时如何复用Tcp连接

15. Cookie放哪里，Cookie能做的事情和存在的价值

16. TCP属于哪一层（1 物理层 -> 2 数据链路层 -> 3 网络层(IP)-> 4 传输层(TCP) -> 5 应用层(Http)）

17. Jsonp方案需要服务端怎么配合

18. Ajax发生跨域要设置什么（前端）

19. 加上CORS之后从发起到请求正式成功的过程

20. Access-Control-Allow-Origin在服务端哪里配置

21. Jsonp为什么不支持Post方法

22. 网络的五层模型

23. HTTPS的加密过程

24. 介绍SSL和TLS

25. 介绍DNS解析

26. formData和原生的Ajax有什么区别

27. 介绍下表单提交，和FormData有什么关系

28. HTTP和TCP的不同
HTTP的责任是去定义数据，在两台计算机相互传递信息时，HTTP规定了每段数据以什么形式表达才是能够被另外一台计算机理解。

而TCP所要规定的是数据应该怎么传输才能稳定且高效的传递与计算机之间。

29. CORS预请求OPTIONS就一定是安全的吗

30. 在项目中如何把http的请求换成https
由于我在项目中是会对axios做一层封装，所以每次请求的域名也是写在配置文件中，有一个baseURL字段专门用于存储它，所以只要改这个字段就可以达到替换所有请求http为https了。
当然后面我也有了解到：
利用meta标签把http请求换为https:

<meta http-equiv ="Content-Security-Policy" content="upgrade-insecure-requests">

31. http请求可以怎么拦截
在浏览器和服务器进行传输的时候，可以被nginx代理所拦截，也可以被网关拦截。

32. https的加密方式
HTTPS主要是采用对称密钥加密和非对称密钥加密组合而成的混合加密机制进行传输。
也就是发送密文的一方用"对方的公钥"进行加密处理"对称的密钥"，然后对方在收到之后使用自己的私钥进行解密得到"对称的密钥"，这在确保双发交换的密钥是安全的前提下使用对称密钥方式进行通信。

33. 混合加密的好处
对称密钥加密和非对称密钥加密都有它们各种的优缺点，而混合加密机制就是将两者结合利用它们各自的优点来进行加密传输。
比如既然对称密钥的优点是加解密效率快，那么在客户端与服务端确定了连接之后就可以用它来进行加密传输。不过前提是得解决双方都能安全的拿到这把对称密钥。这时候就可以利用非对称密钥加密来传输这把对称密钥，因为我们知道非对称密钥加密的优点就是能保证传输的内容是安全的。
所以它的好处是即保证了对称密钥能在双方之间安全的传输，又能使用对称加密方式进行通信，这比单纯的使用非对称加密通信快了很多。以此来解决了HTTP中内容可能被窃听的问题。
其它HTTP相关的问题：
如：


HTTPS的工作流程


混合加密机制的好处


数字签名


ECDHE握手和RSA握手


向前安全性

参考： [HTTPS面试问答](https://github.com/LinDaiDai/niubility-coding-js/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BD%91%E7%BB%9C/HTTPS%E9%9D%A2%E8%AF%95%E9%97%AE%E7%AD%94.md)

34. HTTP 的请求方式场景
Get 方法：获取数据通常(查看数据)-查看
POST 方法：向服务器提交数据通常(创建数据)-create
PUT 方法：向服务器提交数据通常(更新数据)-update，与POST方法很像，也是提交数据，但PUT制定了资源在服务器上的位置，常用在修改数据
HEAD 方法：只请求页面的首部信息
DELETE 方法：删除服务器上的资源
OPTIONS 方法：用于获取当前URL支持的请求方式
TRACE 方法：用于激活一个远程的应用层请求消息回路
CONNECT 方法：把请求链接转换到透明的TCP/IP的通道

35. HTTP状态码

1XX ：信息状态码

100 continue 继续，一般在发送 post 请求时，已发送了 http header 之后服务端将返回此信息，表示确认，之后发送具体参数信息


2XX ：成功状态码

200 ok 正常返回信息
201 created  请求成功并且服务器创建了新资源
202 accepted 服务器已经接收请求，但尚未处理


3XX ：重定向

301 move per 请求的网页已经永久重定向
302 found 临时重定向
303 see other 临时冲重定向，且总是使用get请求新的url
304 not modified 自从上次请求后，请求的网页未修改过


4XX ：客户端错误

400 bad request 服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求
401 unauthorized 请求未授权
403 forbidden 禁止访问


404 not found 找不到如何与url匹配的资源
5XX ：服务器错误

500 internal server error 最常见的服务器端的错误
503 service unacailable 服务器端暂时无法处理请求（可能是过载活维护）
