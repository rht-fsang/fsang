---
title: HTTP知识系列
date: 2022-12-4 10:36:15
tags: 学习
categories: 前端
top: 998
typora-copy-images-to: upload
---

### HTTP报文结构

***

HTTP报文结构：起始行+头部+空行+实体

<!--more-->

#### 请求报文

起始行：方法 + 路径 + http版本，GET /home HTTP/1.1

头部

空行：区分头部和实体

实体：请求体（一般POST方法才有）

#### 相应报文

起始行：http版本+状态码+原因，HTTP/1.1 200 ok

头部

空行：区分头部和实体

实体：响应体（服务端返回的数据）

### HTTP的请求方法

***

**方法**

GET：通常用来回去资源

HEAD：获取资源的元信息

POST：上传信息

PUT：修改数据

DELETE：删除资源

CONNECT：建立连接隧道，用于代理服务器

OPTIONS：列出可对资源实行的请求方法，用来跨域请求

TRACE：追踪请求-相应的传输路径

**GET和POST的区别**

- 缓存：GET请求会被浏览器主动缓存下来，留下历史记录，而POST默认不会
- 编码：GET只能进行URL编码，只能接受ASCII字符，而POST没有限制
- 参数：GET一般放在URL中，不安全，POST放在请求体中，更适合传输敏感信息
- 冥等性：GET是冥等的，而POST不是（冥等表示执行相同的操作，结果也是相同的）
- TCP：GET会把请求报文一次性发出去，而POST会分为两个TCP数据包，先发header部分，如果服务器响应再发body部分

### URL

#### URL结构

协议类型+域名类型+端口号+路径+查询参数+锚点

协议类型：HTTP和HTTPS，HTTPS在HTTP的前提上加上了一个安全保护，在数据给tcp之前对数据先进行加密然后再给到tcp连接传输

域名和IP：域名是IP的别称，方便人去记忆。浏览器跨域通过DNS去解析域名获得对应的IP

端口号：一台机器可以提供多种服务，一个服务对应一个端口号port

路径：具体去哪个页面

查询参数：同一页面看到不同的内容

锚点：同一页面同一内容查看某一位置内容

**URL编码**

URL只能使用ASCII，ASCII之外的字符是不支持显示的，而且还有一部分符号是界定符，如果不处理的会导致解析出错。因此URL引入了编码机制，把非ASCII码字符和界定符转为十六进制字节值，然后再签名加个%。如空格转义成%20

### HTTP状态码

#### 1XX

表示目前是协议处理的中间状态，还需要后续的操作

101 Swithing Protocols。再http升级为websocket的时候，如果服务器同意变更，就会发送状态码101

#### 2XX

表示成功状态

200 OK是最常见的成果状态码，通常在响应体中放有数据

204 NO Content含义与200相同，但响应头后面没有数据

206 Partial Content顾名思义，表示部分内容，它的使用场景为HTTP分块下载和断点连续传，当然同时也会带上相应的响应头字段Content-Range

#### 3XX

表示重定向状态，资源位置发生变动，需要重新请求

301 Moved Permanently即永久重定向，对应着302 Found，即临时重定向

304 Not Modified当协商缓存命中时会返回这个状态码

#### 4XX

请求报文有误

400 Bad Request客户端请求的语法错误，服务端无法理解

401 Unauthorized请求需要用户的身份认证

403 Forbidden服务器理解客户端的请求，但拒接执行此请求

404 Not Found资源未找到，表示没有服务器上找到相应的资源

405 Method Not Allowed请求方法不被服务端允许

406 Not Acceptable资源无法满足客户端的条件

408 Request Timeout服务器等待了太长时间

409 Conflict多个请求发生了冲突

413 Request Entity Took Large请求体积的数据过大

414 Request-URL Too Long请求行里的URL太大

429Too Many Request客户端发送的请求过多

431 Request HeaderFields Too Large请求头的字段内容太大

#### 5XX

服务端发送错误

500 Internal Server Error服务器出错

501 Not Gateway服务器自身是正常的，但是访问的时候出错了

503 Service Unavailable表示服务器当前很忙，暂时无法相应服务

### HTTP特点和缺点

#### HTTP特点

灵活可拓展：一个是语义上自由，只规定了基本格式，还有就是传输形式的多样性

可靠传输：因为HTTP使用的TCP传输协议，所以继承了TCP的可靠传输

请求-应答，也就是一发一收、有来有回

无状态，这里的状态是指的通信过程的上下文信息，而每次http请求都说独立、无关的，默认不需要保留状态信息

#### HTTP缺点

无状态在需要长连接的情况下，需要保存大量的上下文信息，以避免传输重复的信息，这个时候无状态就是缺点；但是如果一些应用仅仅只是为了获取一些数据，不需要保存连接上下文信息，无状态减少了网络开销，此时为优点

明文传输，协议里的报文不适用二进制数据，直接使用文本形式

队头阻塞问题，当http开启长连接时，共用同一个TCP连接，同一时刻只能处理一个请求，那么当前请求耗时过长的情况下，其他的请求只能处理阻塞状态

### Accept

对应Accept系列字段的介绍分为四个部分：数据格式、压缩方式、支持语言和字符集

#### 数据格式

对于发送端利用Content-Type字段限制数据类型，接受到利用Accept字段限制接受的数据类型。以下是具体的取值

text：text/html，text/plain，text/css等

image：image/gif，image/jpeg，image/png等

application：application/json, application/javascript, application/pdf, application/octet-stream

#### 压缩方式

发送端使用Conten-Encoding表明压缩方式，接受端使用Accept-Encoding限制压缩方式。以下是具体的取值

gzip：目前比较流行的压缩格式

deflate：另外一种有名的压缩格式

br：一种专门为HTTP发明的压缩格式

#### 支持语言

发送端使用Content-Language字段表明语言，接收端使用Accept-Language字段表明语言

#### 字符集

发送端直接放在Content-Type中以charset指定字符集，接受端使用Accept-Charset指定可以接受的字符集

// 发送端

Content-Type: text/html; charset=utf-8

// 接收端

Accept-Charset: charset=utf-8

### HTTP传输定长和不定长的数据

#### 定长包体

对于定长包体来说，发送端在传输的时候一般会带上Conten-Length字段指明包体的长度

#### 不定长包体

对于不定长包体，需要配置Transfer-Encoding：chunked，设置了这个字段后会自动产生两个效果，第一个是会忽略Content-Length，第二个就是基于长连接持续推送动态内容

### HTTP处理大文件

对于一些大文件来说，想要一下传输完是不现实的，所以HTTP采取了范围请求的解决方案允许客户端仅仅请求一个资源的一部分

#### 如何支持

前提是服务器支持范围请求，要支持这个功能，响应头上就需要加上Accept-Ranges:none来告诉客户端这边是支持范围请求

#### Range字段拆解

对于客户端，通过Range字段来指定请求那一部分，格式为bytes=x-y

0-499表示从开始到第499个字节

500-表示从第500字节到文件终点

-100表示文件的最后一个100个字节

服务器收到请求之后，首先验证范围是否合法，如果越界了那么返回416错误码，否则读取相应片段，返回206状态码

#### 对于单段数据和多端数据，服务器的响应数据是不一样的

单段数据 Range：bytes=0-9

多段数据 Range：bytes=0-9，30-39

**单段数据**

Content-Range字段，0-9表示请求的返回，100表示资源的总大小

```javascript
HTTP/1.1 206 Partial Content
Content-Length: 10
Accept-Ranges: bytes
Content-Range: bytes 0-9/10
i am xxxxx
```

**多段数据**

Content0Type：multiparty/byteranges；boundary=00000010101表示请求一定是多段数据请求，响应体中的分隔符是00000010101

```javascript
HTTP/1.1 206 Partial Content
Content-Type: multipart/byteranges; boundary=00000010101
Content-Length: 189
Connection: keep-alive
Accept-Ranges: bytes
--00000010101
Content-Type: text/plain
Content-Range: bytes 0-9/96
i am xxxxx
--00000010101
Content-Type: text/plain
Content-Range: bytes 20-29/96
eex jspy 
e--00000010101--
```

### HTTP处理表单数据

在HTTP中，有两种主要的表单提交的方式，体现在两种不同的Content-Type取值，application/x-www-form-urlencoded和multiparty/form-data

#### application/x-www-form-urlencoded

对于application/x-www-form-urlencoded格式的表单内容，有以下特点：数据会被编码成以&分隔的键值对，字符以URL编码方法编码

#### multipart/form-data

请求头中的Content-Type字段会包含boundary。且boundary的值有浏览器默认指定。例：Content-Type：multipart=- - - WebkitFormBoundaryRRJKeWfHPGrS4LKe

数据会分为多个部分，每两个部分之间通过分隔符来分隔，每部分表示均有HTTP头部描述子包体

### HTTP处理队头阻塞

HTTP传输是基于请求-应答的模式进行的，报文必须是一发一收，但值得注意的是，里面的任务被放在一个任务队列中串行执行，一旦队首的请求处理太慢，就会阻塞后面请求的处理

#### 并发连接

对于一个域名允许分配多个长连接，那么相当于增加了任务队列，不至于一个队伍的任务阻塞其他所有任务。在RFC2616规定过客户端最多并发2个连接，不过事实上在现在的浏览器标准中，这个上限要多很多，Chrome是6个

#### 域名分片

一个域名表示可以并发6个长连接，那就多分一些二级域名，在一个baidu.com域名下可以分出很多二级域名，如content.baidu.com,text.baidu.com等二级域名。这样一台服务器能并发的连接数就更多了

### Cookie

#### 生存周期

Cookie的有效期可以通过Expires和Max-Age两个属性，也就是HTTP的强缓存。Expires表示资源过期时间，Max-Age用的是一段时间间隔，单位是秒，从浏览器收到报文开始计算

#### 作用域

关于作用域有两个属性Domain和path，给Cookie绑定了域名和路径，在发送请求之前，发现域名或则路径不匹配的话，就不会带上该Cookie。对于路径来说，/表示域名下的任何路径都允许使用Cookie

#### 安全相关

如果带上Secure，说明只能通过HTTPS传输cookie

如果带上HttpOnly，那么说明只能通过HTTP协议传输，不能通过JS访问，这也是预防XSS攻击的重要的手段

如果带上SameSite属性则可以预防CSRF攻击，SameSite有三个值可以选择：

- 在Strict模式下，浏览器完全禁止第三方请求携带Cookie
- 在Lax模式，只能在get方法提交表单或则a标签发送get请求的情况下可以携带Cookie
- 在None模式下，也就是默认模式，请求都会自动带上Cookie

#### 缺点

容量缺陷：Cookie的体积上限只有4KB。

性能缺陷：Cookie紧跟域名，不管域名下面的某一个地址需不需要这个Cookie，请求都会带上完整的Cookie，这样随着请求数的增多，会造成巨大的性能浪费。但可以通过Domain和Path指定作用域来解决

安全缺陷：由于Cookie以纯文本的形式在浏览器和服务端中传递，这样很容易被非法用户截获，然后进行一系列的篡改。重新发送给服务器。在Http为fasle的情况下，Cookie信息能直接通过JS脚本来读取

### HTTP代理

代理服务器是介于客户端和服务端的一个中间角色，对于客户端来说，表现为服务器进行响应，对于服务器来说，表现为客户端发起请求。具有双重身份

#### 功能

负载均衡：客户端发送的请求会先到达代理服务器，后面有多少源服务器，IP地址客户端都是不知道的

保障安全：利用心跳机制监控后台的服务器，一旦发现故障机就将其踏出集群。并且对上下行的数据进行过滤，对非法IP限流

缓存代理：将内容缓存到代理服务器，使得客户端可以直接从代理服务器获得缓存

#### 相关头部字段

Via，代理服务器需要表明自己的身份，可以通过Via字段来记录。Via中代理的顺序就是HTTP传输中报文传达的顺序

X-Forwarded-For，记录请求方的IP地址

X-Real-IP，获取真实用户的IP地址，X-Forwarded-Host记录客户端的域名，X-Forwarded-Proto记录客户端的协议名

### HTTP缓存

#### 缓存的好处

减少网络带宽的消耗，对网站运营者来说，可以减少网络流量，降低运营成本

降低服务器压力，对于服务器来说，重复使用浏览器本地的缓存数据，可以减少对服务器的请求，降低对服务器的压力

减少网络延迟，加快网络渲染，对客户端来说，使用浏览器本地的缓存比请求服务器的数据更快，提高页面的渲染效率

#### 强缓存（不需要发送http请求。注意：当Expires和Cache-Control同时存在时，Cache-Control会被优先考虑）

**HTTP/1.0版本中使用的Cache-Control**

Expires代表缓存过期时间，存在于服务端返回的响应头中，用来告诉浏览器在这个时间之前都可以直接从缓存中获取数据。如 Expires: Wed, 22 Nov 2019 08:41:00 GMT

**HTTP/1.1版本中使用的Cache-Contral**

Cache-Control采用的是过期时长来控制缓存，对应的字段是max-age。如 Cache-Control：max-age=3600（代表该资源在3600秒内可以直接使用缓存）

Cache-Control还有一些其他的属性，如下：

Public：客户端和代理端都可以对数据进行缓存

private：只能客户端缓存，中间的代理服务器不能缓存

no-cache：不能进行任何形式的缓存

s-naxage：代理服务器的缓存过期时间

#### 协商缓存（强缓存失效后，浏览器在请求头上携带相应的缓存tag香服务器发送请求，由服务器根据tag决定是否使用缓存）

**Last-Modified**

Last-Modified表示最后修改时间。在浏览器第一次给服务器发送请求后，服务器会在响应头中加上这个字段，浏览器接收到后，再第二次请求中会携带If-Modified-Since字段，

这个字段的值是服务器传来的最后修改时间。服务器拿到请求头中的If-Modified-Since字段后，会和服务器中该资源的最后时间修改时间对比，如果请求头中的值小于最后修改时间，

返回新资源。反之则返回304，告诉浏览器直接用缓存

**Etag**

ETag是服务器根据当前文件的内容，给文件生成的唯一标识，只要内容有改动，这个值就会变化。服务器通过响应头把这个值给浏览器。浏览器收到这个值会在

下次请求的时候把这个值作为If-None-Match这个字段的内容，放入请求头中给服务器，服务器收到后与Etag对比，不一致返回新资源，一致则返回403，告诉浏览器直接使用缓存

**对比**

在精度上，ETag由于Last-Modified

在性能上，Last-Modified由于ETag，运维ETag在对每个文件生成唯一标识会损耗性能

#### 缓存位置

浏览器其中的缓存位置按照优先级从高到低排列分别是：Service Worker；Memory Cache；Disk Cache；Push Cache。（比较大的文件会直接丢进磁盘中，反之进入内存 ，当内存使用率很高的时候，也会优先储存在磁盘中）

Service Worker借鉴了Web Worker的思路，既让JS运行在主线程之外，由于它脱离了浏览器的窗体，因此无法访问DOM。它可以实现离线缓存，消息推送和网络代理等功能

Memory Cache指的是内存缓存，从效率上讲它是最快的。但是存活时间也是最短的，当渲染进程结束后，内存缓存也就在不存在了

Disk Cache是储存在磁盘中的缓存，存取效率比内存缓存低，但是存储容量和存储时长都很不错

Push Cache是推送缓存，这是浏览器缓存的最后一道防线，是HTTP/2中的内容

#### 刷新对于强缓存和协商缓存的影响

当ctrl+f5强制刷新网页时，直接重服务器加载，跳过强缓存和协商缓存

当f5刷新网页时，跳过强缓存，但是会检查协商缓存

浏览器地址栏中写入URL，回车 浏览器先去找缓存

### 缓存代理

对于源服务器来说也是有缓存的，但是对于HTTP缓存来说，如果每次客户端缓存失效都要到源服务器获取，那样服务器的压力会也别大，所以让代理服务器接管一部分服务端的HTTP缓存，客户端缓存过期就去最近的代理服务器获取，代理服务端过期了的话就去最近原服务器获取

#### 源服务器的缓存控制

private和public，在源服务器的响应头中，会加上Cache-Control这个字段进行缓存控制字段，那么它的值当中可以加入private或则public表示是否允许代理服务器缓存

proxy-revalidate，must-revalidate的意思s 客户端缓存过期就去源服务器获取，而proxy-revalidate则表示代理服务器的缓存过期后到源服务器获取

s-maxage，s是share的意思，限定了缓存代理服务器中可以放多久，和限制客户端缓存时间max-age并不冲突

#### 客户端的缓存控制

max-stale和min-fresh，在客户端的请求头上，可以加入这两个字段，对代理服务器上的缓存进行宽容和限制操作，max-stale：5表示过期时间在5秒之内可以从代理中获取，min-fresh：5表示等缓存时间过期之前5秒拿代理中的缓存

only-if-cached，这个字段加上后表示客户端只会接受代理缓存，而不会接受源服务器的响应，如果代理缓存无效，则返回504

### 跨域

#### 什么是跨域

浏览器遵循同源政策（协议、IP地址和端口都相同则为永源），非同源站点有以下限制：

- 不能读取和修改对方的DOM

- 不读访问对方的Cookie、IndexDB和LocalStorage

- 限制XMLHttpRequest请求

当浏览器香目标URL发Ajax请求时，只要当前URL和目标URL不同源，则产生跨域，被称为跨域请求，跨域请求的响应一般会被浏览器拦截。在服务端处理完数据后，将响应返回，主进程检查到跨域，且没有cors响应头，将响应体全部丢掉，并不会发送给渲染进程

#### CORS

CORS是W3C的一个标准，全称是跨域资源共享。它需要浏览器和服务器的共同支持，具体来说，非IE和IE1.0以上支持CORS，服务器需要附加特定的响应头

浏览器根据请求方法和请求头的特定字段，将请求做了一下分类，具体来说规则是这样，凡是满足下面条件的属于简单请求：

- 请求方法为GET、POST或则HEAD
- 请求去头的取值范围：Accept、Accept-Language、Content-Language、Content-Type（只限于三个值application/x-form-urlencoded、multipart/form-data、text/plain）

**简单请求**

请求发出去之前，浏览器会在请求头中添加一个Origin字段，用来说明请求来自哪个源。服务器拿到请求之后，在回应时添加Access-Content-Allow-Origin字段，如果Origin不在这个字段的范围中，那么浏览器就会将相应拦截，因此Access-Content-Allow-Origin字段是服务器用来决定浏览器是否拦截这个响应，是必须的一个字段

Access-Control-Allow-Credentials字段是一个布尔值，表示允许发送Cookie，对于跨域请求，浏览器对这个字段默认值为false，而如果需要拿到浏览器的Cookie，需要添加这个响应头并设为true，并且在前端也需要设置withCredentials属性

```javascript
let xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```

Access-Control-Expose-Headers字段是给XMLHttpRequest对象赋能让它不仅可以拿到基本的6个响应头字段（Cache-Control、Content-Language、Content-Type、Expires、Last-Modified和Pragma）, 还能拿到这个字段声明的响应头字段

```javascript
Access-Control-Expose-Headers: aaa
XMLHttpRequest.getResponseHeader('aaa')
```

**非简单请求**

非简单请求相对而言会有些不同，主要有以下两个方面：预检请求和响应字段

以PUT方法为例

```javascript
var url = 'http://xxx.com';
var xhr = new XMLHttpRequest();
xhr.open('PUT', url, true);
xhr.setRequestHeader('X-Custom-Header', 'xxx');
xhr.send();
```

当这段代码执行后，会发送预检请求，预检请求的请求行和请求体是下面的格式

```javascript
OPTIONS / HTTP/1.1
Origin: 当前地址
Host: xxx.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
```

预检请求的方法是OPTIONS，同时会加上Origin源地址和Host目标地址。同时加上以下两个字段

Access-Control-Request-Method，列出CORS请求用到哪个HTTP方法

Accesss-Control-Request-Headers,指定CORS请求将要加上什么请求头

同时响应字段也会分为两部分，一部分是对于预检请求的响应，一部分是对于CORS请求的响应

预检请求的响应

```javascript
HTTP/1.1 200 OK
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 1728000
Content-Type: text/html; charset=utf-8
Content-Encoding: gzip
Content-Length: 0
```

在预检响应字段中有几个关键的字段

Access-Control-Allow-Origin：表示跨域允许请求的源，可以填具体的源名，也可以填*表示任意源

Access-Control-Allow-Methods：表示允许的请求方法列表

Access-Control-Allow-Headers：简单请求中已经介绍

Access-Control-Allow-Credentials：表示允许发送的请求字段

Access-Control-Max-Age：表示预检请求的有效期，在此期间，不用发出另一条预检请求

在预检请求的响应返回后，如果请求不满足响应头的条件，则触发XMLHttpRequest的onerror方法，当然后面真正的CORS请求也不会发出来了

#### JSONP

虽然XMLHttpRequest对象遵循同源政策，但是script标签不一样，它可以通过src填上目标地址从而发出GET请求，实现跨域请求并拿到响应。

和CORS相比，JSONP最大的优势在于兼容性好，IE低版本不能使用CORS但可以使用JSONP，缺点也很明显，请求方法单一，只支持GET请求。

以下是封装的一个JSONP

```javascript
const jsonp = ({ url, params, callbackName }) => {
  const generateURL = () => {
    let dataStr = '';
 for(let key in params) {
      dataStr += `${key}=${params[key]}&`;
    }
 dataStr += `callback=${callbackName}`;
    return `${url}?${dataStr}`;
  };
 return new Promise((resolve, reject) => {
    // 初始化回调函数名称
    callbackName = callbackName || Math.random().toString.replace(',', ''); 
// 创建 script 元素并加入到当前文档中
    let scriptEle = document.createElement('script');
scriptEle.src = generateURL();
    document.body.appendChild(scriptEle);
  // 绑定到 window 上，为了后面调用
    window[callbackName] = (data) => {
resolve(data);
      // script 执行完了，成为无用元素，需要清除
      document.body.removeChild(scriptEle);
     }
   });
}
```

#### Nginx

Nginx是一个高性能的反向代理服务器，可以用来轻松解决跨域问题

正向代理帮助客户端访问客户端自己访问不到的服务器，然后将结果返回客户端

反向代理拿到客户端的请求，将请求转发给其他的服务器，主要场景是维持服务器集群的负载均衡，换句话说，反向代理帮其他的服务器拿到请求，然后选择一个合适的服务器，将请求转交给它

因此，正向代理服务器是帮客户端做事情，反向代理服务器是帮其他的服务器做事情

Nginx相当于一个跳板机，这个跳板机的域名也是client.com，让客户端访问client.com/api，这当然没有跨域，然后Nginx服务器作为反向代理，将请求转发给server.com，当响应应返回时又将响应给到客户端

### TLs1.2握手过程

前面我们已经知道HTTPS = HTTP + SSL/TLS来保证数据的安全

SSL是安全套接层，在OSI七层模型中处于会话层。之前SSL出过三个大版本，当它发展到第三个大版本的时候才被标准化，成为TLS（传输层安全），并被当做TLS1.0。也就是说TLS1.0 = SSL3.0

现在主流的版本是TLS/1.2，之前的TLS1.0、TLS1.0和TLS1.1都被认为是不安全的，在不久的将来会被完全淘汰。因此我们接下来主要讨论的是TLS1.2，当然在2018年推出了更加优秀的TLS1.3，大大优化了TLS握手过程

#### TLs1.2握手过程

**Client Hello**

首先，浏览器发送client_random、TLS版本、加密套件列表

client_random是用来最终确定secret的一个参数

在TLS握手过程中，使用ECDHE算法生成pre_random,128位的AES算法进行对称加密，在对称加密的过程中使用主流的GCM分组模式，因为对称加密中很重要的一个问题就是如何分组。最后一个是哈希摘要算法，采用SHA256算法

哈希摘要算法解释实例

服务端给客户端发信息，客户端并不知道此时的信息是服务端发的还是中间伪造的信息，这个时候就要引入哈希摘要算法，将服务端的证书信息通过这个算法生成一个摘要，用来标识这个服务端的身份，用私钥加密后把加密后的标识和自己的公钥传给客户端。客户端拿到这个公钥来解密，生成零一分摘要。两个摘要进行对比，如果相同则能确认服务端的身份

**Server Hello**

服务器给客户端回复非常多的内容

server_random也是最后生成secret的一个参数，同时确认TLS版本、需要使用的加密套件和自己的证书

**Client验证证书，生成secret**

客户端验证服务端传来的证书和签名是否通过，如果验证通过，则传递client_params这个参数给服务器

客户端通过EECDHE算法计算出pre_random,其中传入两个参数：server_params和slient_params

客户端现在拥有了client_random、server_random和pre_random，接下来这三个数通过一个伪随机数函数来计算最终的secret

**Server生成secret**

接收到客户端传的client_params，服务端使用ECDHE算法生成pre_random，接着用和客户端的伪随机数函数生成最后的secret

#### RAS和ECDHE握手过程的区别

ECDHE握手，也就是主流的TLS1.2握手中，使用ECDHE实现pre_random的加密解密，没有用到RSA

使用ECDHE还有一个特点，就是客户端发完首位信息后跨域提前抢跑，直接发送HTTP报文，节省了一个RTT，不必等到收尾信息到达服务器，然后等服务器返回收尾消息给自己，直接开始发请求

### TLS1.3改进

TLS1.3对TLS1.2做了一系列的改进，主要分为着几个部分：强化安全、提高性能

#### 强化安全

在TLS1.3中废除了非常多的加密算法，最后只保留五个加密套件。同时叶删除了非对称加密算法

- TLS_AES_128_GCM_SHA256
- TLS_AES_256_GCM_SHA384
- TLS_CHACHA20_POLY1305_SHA256
- TLS_AES_128_GCM_SHA256
- TLS_AES_128_GCM_8_SHA256

#### 提高性能

**握手改进**

大体的方式和TLS1.2差不多，不过和TLS1.2相比少了一个RTT，服务器不必等待对方验证证书之后才拿到client_params，而是直接在第一次握手的时候就能够拿到，拿到之后立即计算secret，节省了之前不必要的等待时间。同时，这也以为这在第一次握手的时候客户端需要传送更多的信息，一口气传完

**会话复用**

会话复用有两种方式：Sessio ID和Session Ticket

Session ID是在客户端和服务端首次连接后各自保存会话的ID，并存储会话密钥，当再次连接时，客户端发送ID过来，服务器查找这个ID是否存在，如果找到了就直接复用之前的会话状态，会话密钥不用重新生成，直接用来的那份。当客户端特别的多的时候，服务器的压力会特别大。

Session Ticket 当服务端的压力大的时候，就把压力分摊给客户端。双方连接成功后，服务器加密会话信息，用Session Ticket消息发给客户端，让客户端保存下来。下次重连的时候，就把这个Ticket进行解密，验证没过期那就直接恢复之前的会话状态。这种方式减少了服务端的压力，但是带来了安全问题，即每次用一个固定的密钥来解密Ticket数据，一旦黑客拿到了这个密钥，之前的所有的历史记录也被破解了。因此密钥需要定期进行更换

**PSK**

前面说的都是1-RTT情况下的优化，PSK就是使用0-RTT在Session Ticket的同时带上应用数据，不用等到服务端确认。这样会方服务器被攻击的风险很大

### HTTP/2改进

由于HTTPS在安全方面已经做的非常好了，HTTP改进的关注点放在了性能方面。头部压缩和多路复用，还有颠覆性的功能实现设置请求优先级和服务器推送

#### 头部压缩

HPACK算法是专门为HTTP/2服务的，它有以下两个优点：

- 在服务器和客户端之间建立哈希表，将用到的字段存放在这张表中，那么在传输的时候对于之前出现过的值，只需要把索引传给对方即可，对方拿到索引查表就好了
- 对于整数和字符串进行哈夫曼编码，哈夫曼编码的原理就是先将所有出现的字符建立一张索引表，然后让出现次数多的字符对应的索引尽可能短，传输的时候也是传输这样的索引序列，可以达到非常高的压缩率

#### 多路复用

TCP的对头阻塞是在数据包层面，单位是数据包，前一个报文密钥收到便不会将后面收到的报文上传给HTTP，而HTTP队头阻塞是在HTTP的队头阻塞在HTTP请求-响应层面，前一个请求密钥请求完，后面的请求就要阻塞住

HTTP/2认为明文传给机器而言太麻烦了，不方便计算机的解析，因为对于文本而言会有多义性的字符，比如回车换行到底是内容还是分隔符，在内部需要用到状态机去识别。于是HTTP/2干脆把报文全部换成二进制格式，全部01传输，方便了机器的解析

通信双方都可以给对方发送二进制帧，这种二进制帧的双向传输的序列也叫做流。HTTP/2用流来在一个TCP连接上进行多个数据帧的通信，这就是多路复用的概念

#### 服务器推送

在HTTP/2中，服务器已经不再完全的被动的接受请求，响应请求，它也能新建stream来给客户端发送消息，当TCP连接建立之后，比如浏览器请求一个HTML文件，服务器就可以在返回HTML的基础上，将HTML中引用到的其他资源文件一起返回给客户端，减少客户端的等待

### HTTP/2中的二进制

HTTP/2中传输的帧结构如下图所示：

![image-20221214143815969](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221214143815969.png)

每个帧分为帧头和帧体。先是三个字节的帧长度，这个长度表示的是帧体的长度然后是帧类型，大概可以分为数据帧和控制帧两种。数据帧用来存放HTTP报文，控制帧用来管理流的传输。

接下来的一个字节是帧标志，黎曼一共8个标志位，常用的有END_HEADERS表示数据结束，END_STRAEAM表示单方向数据发送结束

后四个字节是Stream ID，也就是流标识符，有了它接收方就能从乱序的二进制帧中选择出ID相同的帧，按顺序组装成请求/响应报文

#### 流的状态变化

![image-20221214143852729](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221214143852729.png)

最开始两者都是空闲状态，当客户端发送Headers帧后，开始分配Stream ID，此时客户端的流打开，服务端接受之后服务端的流也打开，两端的流都打开之后，就可以互相传递数据帧和控制帧

当客户端要关闭时，向服务端发送END_SITREAM帧，进入半关闭状态，不过此时服务端的情况是只能发送数据，而不能接受数据。随后服务端也想客户端发送END_STREAM帧，表示数据发送完毕，双方进入关闭状态

如果下次需要开启新的流，流ID需要自增，直到上限为止，到达上限后开一个新的TCP连接重头开始计数。由于流ID字段长度为四个字节，最高位又被保留，因此范围是0~2的31次方，大约21亿个

#### 流的特性

- 并发性，一个HTTP/2连接上可以同时发多个帧，这一点和HTTP/1不同
- 自增性，流ID是不可重用的，而是会按顺序递增，达到上限后又新开TCP连接从头开始
- 双向性，客户端和服务端都可以创建流，互不干扰，双方都可以作为发送发或接收方
- 可设置优先级，可以设置数据帧的优先级，让服务器优先处理重要资源，优化用户体验
