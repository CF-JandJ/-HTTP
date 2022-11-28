# 1.理解Web和网络基础

客户端：发出请求

Web使用HTTP（超文本传输协议）的协议作为规范，故可以说Web是建立在HTTP协议上的通信

通常使用的网络是在TCP/IP协议族的基础上运作的，HTTP属于它内部的一个子集

协议:相互通信的规则

## TCP/IP协议
TCP/IP协议：把与互联网相关联的协议集合起来的总称

TCP/TP的分层管理：TCP/IP协议族按层次分别为：应用层、传输层、网络层和数据链路层
层次化的好处：如果某个机房需要改变设计，不需要把所有部分整体替换掉，只需要把变动的层替换掉即可

**各层的作用**
应用层：
应用层决定了向用户提供应用服务时通信的活动
TCP/IP协议族内预存了各类通用的应用服务。比如：FTP（文件传输协议）和DNS（域名系统）
HTTP协议也在该层

传输层：
传输层对上层应用层提供处在网络连接中的两台计算机之间的数据传输
在传输层有两个性质不同的协议：TCP（传输控制协议）和UDP（用户数据报协议）

网络层：
网络层用来处理在网络上流动的数据包。数据包是网络传输的最小数据单位。该层规定了通过怎么样的路径到达对面计算机，并且把数据包传送给对方。
与对方计算机之间通过多台计算机或网络设备进行传输时，网络层所起的作用就是在众多的选项内选择一条传输路线

链路层：
用来处理链接网络的硬件部分。

![](https://note.youdao.com/yws/res/0/WEBRESOURCE27234308b8aa3cfcbccf7659b9717d63)

利用TCP/IP协议族进行网络通信，会通过分层顺序与对方进行通信。发送端从应用层往下走，接收端则往应用层往上走
以HTTP为例，首先作为发送端的客户端在应用层发出一个HTTP请求，为了传输方便，在传输层把从应用层处收到的数据进行分割，并且各个报文上标记序号以及端口号后转发给网络层，在网络层增加作为通信目的地的MAC地址后转发给链路层。接收端的服务器在链路层接收到数据，按序往上层发送。

![](https://note.youdao.com/yws/res/0/WEBRESOURCE6ab4b20b21f9c50c817929320c470a21)

发送端在层和层之间传输数据时，每经过一层时，必须会被打上一个该层所属的首部信息，反之，接收端在层与层传输数据时，每经过一层时会把对应的首部消去。这种把信息包装起来的做法叫做封装

## 与HTTP关系密切的三个协议：IP、TCP和DNS

IP：
按层次，IP位于网络层。
IP协议的作用就是把各类数据包传送给对方。而要确保传送到对方那里，则需要满足各类条件。其中两个重要条件就是IP地址和MAC地址

IP地址指明了节点被分配到的地址，MAC地址是指网卡所属的固定地址。IP地址可疑和MAC地址进行配对。IP地址可变换，但是MAC地址基本上不会更改

ARP协议
使用ARP协议凭借MAC地址进行通信
IP间的通信依赖MAC地址。在进行中转时，利用下一站中转设备的MAC地址来搜索下一个中转目标。APR是一种用以解析地址的协议，根据通信方的IP地址就可以反查出对应的MAC地址

TCP协议：
TCP位于传输层，提供可靠的字节流服务
字节流服务：为了方便传输，将大块数据分割成以报文段为单位的数据包进行管理。可靠的传输服务指能够把数据准确可靠的传给对方。

如何确保数据能够到达目标？
三次握手策略
TCP协议把数据包送出去后，TCP不会对传送后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了TCP的标志（SYN和ACK）

发送端发送一个带SYN标志的数据包给对方，接收端收到后，回传一个带有SYN/ACK标志的数据包传达确认信息，最后发送端再回传一个带ACK标志的数据包，代表握手结束

![](https://note.youdao.com/yws/res/0/WEBRESOURCE5f4b84ed0599faf077aabfaf740f6cfb)

## 负责域名解析的DNS服务

DNS服务是和HTTP协议一样位于应用层的协议，它提供域名到IP地址之间的解析服务

DNS协议提供通过域名查找IP地址，或逆向从IP地址反查域名的服务

![](https://note.youdao.com/yws/res/0/WEBRESOURCEa3c1289a93d4a105c3b4163bfd1ed83c)

## 各自发挥的作用

![](https://note.youdao.com/yws/res/0/WEBRESOURCE769a944f6623e0fb327d0c72e29e9076)

URI（统一资源标识符），URL（统一资源定位符）
URI用字符串标识某一互联网资源，而URL表示资源的地点，URL是URI的子集
![](https://note.youdao.com/yws/res/0/WEBRESOURCE76558d7c6c65057bde9fa24d62c3638d)

协议方案名：http: 或 https:
登录信息：指定用户名和密码作为从服务器端获取资源时必要的登录信息，可选
服务器地址：使用绝对URI必须指定待访问的服务器地址，地址可以是NDS可解析名称，也可以是IPv4地址名，也可以是IPv6地址名
服务器端口号：指定服务器链接的网络端口号，可选
带层次的文件路径：指定服务器上的文件路径来定位特指的资源
查询字符串：针对已指定的文件路径内的资源，可以使用查询字符串传入任意参数，可选
片段标识符：使用片段标识符通常可以标记处已获取资源中的子资源，可选

# 简单的HTTP协议

请求访问文本或图片等资源的一端称为客户端，而提供资源响应的一端称为服务端
注：应用HTTP协议时，必定是一端担任客户端的角色，另一端担任服务器端的角色

![](https://note.youdao.com/yws/res/0/WEBRESOURCE92a8087e9b5e56f9d868e2e8f51945b3)


请求：
GET表示请求访问服务器类型，随后的字符串/index.htm指明了请求访问的资源对象，也叫URI，最后HTTP/1.1为HTTP的版本号
内容意思为：请求访问某台HTTP服务器上的/index.htm页面资源

请求报文组成：请求方法、请求URI、协议版本、可选的请求首部字段和内容实体构成

![](https://note.youdao.com/yws/res/0/WEBRESOURCE6124c49f8a6a18cae5828abd3c940aa0)

响应：
开头HTTP/1.1表示服务器对应的HTTP版本
200 OK表示请求的处理结果的状态码和原因短语
下一行显示了创建响应的日期时间，是首部字段内的一个属性
接着以一空行分隔，之后的内容称为资源实体的主体
响应报文基本上由：协议版本、状态码、用以解释状态码的原因短语、可选的响应首部字段以及实体主体构成

![](https://note.youdao.com/yws/res/0/WEBRESOURCE897000a42145eadb411e73d848ee666c)

**HTTP是不保存状态的协议**
在HTTP这个级别，协议对于发送过的请求或响应都不做持久化处理
虽然HTTP/1.1是无状态协议，但为了实现期望的保持状态功能，引入了Cookie技术，有了Cookie再用HTTP协议通信，就可以管理状态了

## 请求URI定位资源
当客户端请求访问资源而发送请求时，URI需要将作为请求报文中的请求URI包含在内。
指定请求URI的方式有很多。

![](https://note.youdao.com/yws/res/0/WEBRESOURCE6fe2e23118db11b0e0d25c10cceef04c)

除此之外，如果不是访问特定资源而是对服务器本身发起请求，可以用*来代替请求URI
例如：

`OPTIONS * HTTP/1.1 `

## 告知服务器意图的HTTP方法

**GET：获取资源**
GET方法用来请求访问已被URI识别的资源。指定的资源经服务器端解析后返回响应内容。
如果请求的资源是文本->保持原样返回
如果像CGI（通用网关接口）那样的程序，则返回经过执行后的输出结果

![](https://note.youdao.com/yws/res/0/WEBRESOURCEabb513bd4251cd64f67d2d3ca71d62ed)

**POST:传输实体主体**

POST方法用来传输实体的主体
POST功能和GET相似，但POST主要目的不是获取响应的主体内容

注意：响应的意思是请求执行成功了，但无数据返回

![](https://note.youdao.com/yws/res/0/WEBRESOURCE330fd4d25241a5e049571680b1074234)

**PUT:传输文件**

PUT方法用来传输文件。像FTP协议的文件上传一样，要求在请求报文的主体中包含文件内容，然后保存到请求URI指定的位置。
但是PUT方法自身不带验证机制，任何人都可以上传文件，存在安全问题，一般不使用该方法。

![](https://note.youdao.com/yws/res/0/WEBRESOURCE0973ab49e884b9962af14b43fd13ce24)

**HEAD:获得报文首部**
HEAD方法和GET方法一样，只是不返回报文主体部分。用于确认URI的有效性及资源更新的日期时间等

![](https://note.youdao.com/yws/res/0/WEBRESOURCE1e6feb04e78748a77c690ebfc62e7579)

**DELETE:删除文件**
DELETE方法用来删除文件，与PUT相反的方法。DELETE方法按请求URI删除指定的资源。
但是DELETE方法本身和PUT方法一样不带验证机制，所以一般Web网站不使用DELETE方法

![](https://note.youdao.com/yws/res/0/WEBRESOURCEa7e6df4ae23b8a6e8d296d519730eda3)

**OPTIONS:询问支持的方法**
OPTIONS方法用来查询针对请求URI指定的资源支持的方法

![](https://note.youdao.com/yws/res/0/WEBRESOURCEda6b53cc36f62fd016fe2a969c142602)

**TRACE:追踪路径**

TRACE方法是让Web服务器端将之前的请求通信环回给客户端的方法
客户端通过TRACE方法可以查询发送出去的请求是怎样被加工修改的。
但是TRACE方法不常用加上容易引发XST攻击，通常更不会用到

![](https://note.youdao.com/yws/res/0/WEBRESOURCE2f0079b5d557e403642eaed9776034ae)

**CONNECT:要求用隧道协议连接代理**
CONNECT方法要求在与代理服务器通信时建立隧道，实现用隧道协议进行TCP通信。
主要使用SSL（安全套接层）和TLS（传输层安全）协议把通信内容加密后经网络隧道传输。
CONNECT方法的格式如下：
`CONNECT 代理服务器名 ：端口号 HTTP版本`

![](https://note.youdao.com/yws/res/0/WEBRESOURCE454c9f7d863d4d9aeeb864e4131326ef)

## 使用方法下达命令

向请求URI指定的资源发放请求报文时，采用称为方法的命令。
方法的作用在于，可以指定请求的资源按期望产生某种行为。

![](https://note.youdao.com/yws/res/0/WEBRESOURCE7547e0df8e72a8faddc408fb8de0aff0)

![](https://note.youdao.com/yws/res/0/WEBRESOURCEd7783bb09233e3963402b108330e7d64)

## 持久连接节省通信量

HTTP协议的初始版本中，每进行一次HTTP通信就要断开一次TCP连接

![](https://note.youdao.com/yws/res/0/WEBRESOURCE9bddb9601bcd0b33bd1987f257d1e237)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE18c905c395a5b41968cf2e1c89818e29)

为了解决上述TCP连接的问题，提出持久连接的方法。持久连接的特点是：只要任意一端没有明确提出断开连接，则保持TCP连接状态

![](https://note.youdao.com/yws/res/0/WEBRESOURCE9d60b95c312535d69905cd344baf3de0)

## 管线化

持久连接使得多数请求以管线化方法发送成为可能。
这样能够做到同时并行发送多个请求，而不需要一个接一个地等待响应。

![](https://note.youdao.com/yws/res/0/WEBRESOURCEa649c8dd8e8ac2679bee29b51cf9229a)

## 使用Cookie的状态管理

HTTP是无状态协议，它不对之前发生过的请求和响应的状态进行管理。也就是说无法根据之前的状态进行本次的请求处理

Cookie技术通过在请求和响应报文中写入Cookie信息来控制客户端的状态

Cookie会根据从服务器端发送的响应报文内的Set-Cookie的首部字段信息，通知客户端保持Cookie，当下次客户端再往服务器发送请求时，客户端会自动在请求报文中加入Cookie值后发送出去

服务器端发现客户端发送过来的Cookie后，会去检查是从哪一个客户端发来的连接请求，然后对比服务器上的记录，最后得到之前的状态信息

![](https://note.youdao.com/yws/res/0/WEBRESOURCEcf927fbcf0004fac530da109774000b9)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE4d2b6029dcd7ca07a8e86a2e73650005)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE3b500a6b80734d648cf4b9362291a6d2)

# HTTP报文内的HTTP信息

HTTP通信过程包括从客户端发往服务器端的请求及从服务器端返回客户端的响应

# HTTP报文

用于HTTP协议交互的信息被称为HTTP报文。
请求端（客户端）的HTTP报文叫做请求报文
响应端（服务器端）的叫做响应报文
HTTP报文本身是由多行数据构成的字符串文本

HTTP报文大致分为报文首部和报文主体两块
两者由空行（CR+LF）来划分，通常并不一定要有报文主体

![](https://note.youdao.com/yws/res/0/WEBRESOURCE1c74d467750c837a2dd2a2b2ddbe174b)

## 请求报文及响应报文的结构

![](https://note.youdao.com/yws/res/0/WEBRESOURCE095e42df368eada473969a5d22b10ffa)

![](https://note.youdao.com/yws/res/0/WEBRESOURCEad24a04b5c9a91b5002e09b72cb7b3ad)

请求行：包含用于请求的方法，请求URI和HTTP版本

状态行：包含表明响应结果的状态码，原因短语和HTTP版本

首部字段：包含表示请求和响应的各种条件和属性的各类首部
一般有四种首部，分别是：通用首部、请求首部、响应首部和实体首部

其他：可能包含HTTP的RFC里未定义的首部（Cookie等）

## 报文实体和实体主体的差异

报文：是HTTP通信中的基本单位，由8位组字节流组成，通过HTTP通信传输

实体：作为请求或响应的有效载荷数据被传输，其内容由实体首部和实体主体组成

HTTP报文的主体用于传输请求或响应的实体主体

通常报文主体等于实体主体，只有当传输中进行编码操作时，实体主体的内容发生变化，才导致它和报文主体产生差异

## 压缩传输的内容编码

向待发送邮件内增加附件时，为了使邮件容量变小，我们会先用ZIP压缩文件之后再添加附件发送。HTTP协议中有一种被称为内容编码的功能也能进行类似的操作。

内容编码指明应用在实体内容上的编码格式，并保持实体信息原样压缩。内容编码后的实体由客户端接受并负责解码

![](https://note.youdao.com/yws/res/0/WEBRESOURCEc8273edacb530cd7cd362031a0d955eb)

## 分割发送的分块传输编码

在HTTP通信过程中，请求的编码实体资源未全部传输完成之前，浏览器无法显示请求页面。
在传输大容量数据时，通过把数据分割成多块，可以让浏览器逐步显示页面。

这种把实体主体分块的功能称为分块传输编码

![](https://note.youdao.com/yws/res/0/WEBRESOURCE5481f6142517032a246530135b557c80)

分块传输编码会将实体主体分成多个部分，每一块都会用十六进制来标记块的大小，而实体主体的最后一块会使用“0（CR+LF）”来标记

使用分块传输编码的实体主体会由接受的客户端负责解码，恢复到编码前的实体主体

## 发送多种数据的多部分对象集合

发送邮件时，我们可以在邮件里写入文字并添加多份邮件，因为采用了MIME（多用途因特网邮件扩展）机制，它允许邮件处理文本、图片、视频等多个不同类型的数据

MIME扩展中会使用一种成为多部分对象集合的方法，来容纳多分不同类型的数据
相应地，HTTP协议中也采纳了多部分对象集合，发送的一份报文主体内可含有多类型实体

![](https://note.youdao.com/yws/res/0/WEBRESOURCE5a7a386d44239d6947f39dd29dcdd47e)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE0d2198f97036aff22e1ba10261eb5766)

一个是上传，一个是响应

在HTTP报文中，使用多部分对象集合时，需要在首部字段加上Content-type
使用boundary字符串来划分多部分对象集合指明的各类实体
在boundary字符串指定的各个实体的起始行之前插入“--”标记，而在大部分对象集合对应的字符串的最后插入“--”标记作为结束

## 获取部分内容的范围请求

恢复：能从之前下载中断处恢复下载，要实现该功能，需要指定下载的实体范围，这种请求被称为范围请求

![](https://note.youdao.com/yws/res/0/WEBRESOURCE011f5b1a9b69a5f6ff004a6ca93d44a1)

执行范围请求时，会用到首部字段Range来指定资源的byte范围

byte范围指定形式如下：

![](https://note.youdao.com/yws/res/0/WEBRESOURCE3e59974d7871c916eef2f79f9afcf95c)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE19b35991fea66c47e43c8b4b3fd5c9da)

针对范围请求，响应会返回状态码为206 Partial Content的响应报文
对于多重范围的范围请求，响应会在首部字段表明 multipart/byteranges后返回响应报文
如果服务器端无法响应范围请求，则会返回状态码200 OK和完整的实体内容

## 内容协商返回最合适的内容

同一个Web网站有可能存在着多份相同内容的页面，内容虽相同，但是语言却不同。
当浏览器默认语言为英文或中文，访问相同URI的Web页面时，会显示对应的英文版或中文版的Web页面，这种机制被称为内容协商

![](https://note.youdao.com/yws/res/0/WEBRESOURCE8bce834da6db233163b06f8c15221f2f)


内容协商机制是指客户端和服务器端就响应的资源内容进行交涉，然后提供给客户端最合适的资源。内容协商会以响应资源的语言、字符集、编码方式等作为判断基准

![](https://note.youdao.com/yws/res/0/WEBRESOURCE3c88d2b8365e86f578ef8eb59847cb42)

内容协商技术有三种类型

服务器驱动协商：
由服务器端进行内容协商。以请求的首部字段为参考，在服务器端自动处理。

客户端驱动协商：
由客户端进行内容协商的方式。用户从浏览器显示的可选列表中手动选择，也可以利用JavaScript脚本在Web页面上自动进行上述选择。

透明协商：
是服务器驱动和客户端驱动的结合体，是由服务器端和客户端各自进行内容协商的一种方法。

# 返回结果的HTTP状态码

HTTP状态码负责表示客户端HTTP请求的返回结果、标记服务器端的处理是否正常、通知出现的错误等工作。


## 状态码告知从服务器端返回的请求结果

状态码的职责是当客户端向服务端发送请求时，描述返回的请求结果
借助状态码，用户可以知道服务器端是否正常处理了请求

![](https://note.youdao.com/yws/res/0/WEBRESOURCE918d63e7eeb3f788c84ed4296b86e357)

状态码如200 OK，以3位数字和原因短句组成

数字中的第一位指定了响应类别，后两位无分类。

响应类型分别有以下5种

![](https://note.youdao.com/yws/res/0/WEBRESOURCEad49bf7795714a80749c4f3df1ad2d2b)

代表性的14个状态码

## 2XX 成功

2XX的响应结果表明请求被正常处理了

**200 OK：**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE7c8fb7a3f0fb7374579ff2200967a0f5)

表示从客户端发来的请求在服务器端被正常处理了

在响应报文内，随状态码一起返回的信息会因为方法的不同而发生改变
比如：
GET方法时，对应请求资源的实体会作为响应返回；
HEAD方法时，对应请求资源的实体首部不随报文主体作为响应返回
      
**204 No Content**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE93509c4a9f98ae97f319a3637f3ee461)

该状态码代表服务器接受的请求已成功处理，但在返回的响应报文中不含实体的主体部分。另外也不允许返回任何实体的主体。

比如当从浏览器发出请求后，返回204响应，则浏览器显示的页面不发生更新

情况：只需要从客户端往服务器发送信息，而对客户端不需要发送新信息内容的情况下使用

**206 Partial Content**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEc6e8a4816db7541d2894dbd325ade805)

该状态码表示客户端进行了范围请求，而服务器成功执行了这部分的GET请求。
响应报文中包含由Content-Range指定范围的实体内容

## 3XX 重定向

3XX响应结果表明浏览器需要执行某些特殊的处理以正确处理请求

**301 Moverd Permanently**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE051ff44aabe237e9b3c4f7eb6bf69578)

永久性重定向。
该状态码表示请求的资源已被分配了新的URI，以后应使用资源现在所知的URI。
如果已经把资源对应的URI保持为书签了，这时应该按Location首部字段提示的URI重新保存

像下方给出的请求URI，当指定资源路径的最后忘记添加斜杠“/”，就会产生301状态码
`http://example.com/sample`
注意：加斜杠，默认当做目录处理，不加默认当做文件处理

**302 Found**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE59f6f371643e1652c86801550b927710)

临时性重定向。该状态码表示请求的资源已被分配了新的URI，希望用户能使用新的URI访问

和301 Moved Permanently状态码相似，但302状态码代表的资源不是被永久移动，只是临时性质的，换句话说，已移动的资源对应的URI将来还可能发生变换。
比如用户把URI保存成书签，但不会像301状态码出现时那样去更新书签，而是仍然保留返回302状态码的页面对应的URI

**303 See Other**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEbf754c9e016fa8ebdcb3359afcfa2634)

该状态码表示由于请求对应的资源存在着另一个URI，应使用GET方法定向获取请求的资源

303状态码和302 Found状态码有着相同的功能，但303状态码明确表示客户端应当采用GET方法获取资源。

虽然 RFC1945 和RFC 2068 规范不允许客户端在重定向时改变请求的方法，但是很多现存的浏览器将302响应视为303响应，并且使用GET方法访问在Location中规定的URI，而无视原先的方法，所以303是最理想的

**304 Not Modified**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE63f87cf2f0d295f25ee6dbcdf2271b5c)

该状态码表示客户端发送附带条件的请求时，服务器端允许请求访问资源，但未满足条件的情况。
304状态码返回时，不包含任何响应的主体部分。304虽然被划分在3XX类别中，但是和重定向无关

附带条件的请求指采用GET方法的请求报文中包含If-Match,If-Modified-Since,If-None-Match,If-Range,If-Unmodified-Since中任一首部

**307 Temporary Redirect**

临时重定向。该状态码与302 Found有着相同的含义
307会按照浏览器的标准，不会从POST变成GET，但是对于处理响应时的行为，每种浏览器可能出现不同情况。

## 4XX 客户端错误

4XX的响应结果表明客户端是发生错误的原因所在

**400 Bad Request**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEc80fc685674ffd817bf00ffb10a92a5f)

该状态码表示请求报文中存在语法错误。
当错误发生时，需要修改请求内容后再次发送请求

**401 Unauthorized**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEa72dc033d197a960adaed4767c9d9f76)

该状态码表示发送的请求需要有通过HTTP认证的认证信息，另外若之前已进行过1次请求，则表示用户认证失败

返回含有401的响应必须包含一个适用于被请求资源的WWW-Authenticate首部用以质询（challenge）用户信息，当浏览器初次接受到401响应，会弹出认证用的对话窗口。

**403 Forbidden**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE5e6075d0d282793ff8565d9c443f4f4f)

该状态码表明对请求资源的访问被服务器拒绝了，服务器必要时给出拒绝的理由，如果想作说明，可以在实体的主体部分对原因进行描述，这样用户就可以看到

**404 Not Found**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEe03e75856830af01b4f48dd5e636ed1e)

该状态码表明服务器上无法找到请求的资源，除此之外，也可以在服务器拒绝请求且不想说明理由时使用。

## 5XX 服务器错误

5XX的响应结果表明服务器本身发生错误

**500 Internal Server Error**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE05bed78286eee11675d61a9eadb53e90)

该状态码表明服务器端在执行请求时发生了错误。也有可能是Web应用存在的Bug或某些临时的故障

**503 Service Unavailable**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE267737dfdb8488bb755914c81d833005)

该状态码表明服务器暂时处在超负荷或正在进行停机维护，现在无法处理请求
如果事先得知解除以上状况需要的时间，最好写入Retry-After首部字段再返回给客户端

# 与HTTP协作的Web服务器

## 用单台虚拟主机实现多个域名

允许一台HTTP服务器搭建多个Web站点
即使物理层面只有一台服务器，但只要使用虚拟主机的功能，则可以假想已具有多台服务器

![](https://note.youdao.com/yws/res/0/WEBRESOURCEf273c9e190baff7e8967ccbabacc5da2)

域名通过DNS服务映射到IP地址（域名解析）之后访问目标网站，可见，当请求发送到服务器时，已经是IP地址形式访问了

所以如果一台服务器内托管了多个域名，当收到请求时就需要弄清究竟要访问哪个域名

![](https://note.youdao.com/yws/res/0/WEBRESOURCE5ad2adfb6f3e89a48866cbf3dcc423b5)

在相同的IP地址下，由于虚拟主机可以寄存多个不同主机名和域名的Web网站，所以发送HTTP请求时，必须在Host首部内完整指定主机名或域名的URI

## 通信数据转发程序 ： 代理、网关、隧道

代理：
代理是一种有转发功能的应用程序，它扮演了位于服务器和客户端中间人的角色，接受由客户端发送的请求并转发给服务器，同时也接受服务器返回的响应并转发给客户端

网关：
网关是转发其他服务器通信数据的服务器，接受从客户端发送来的请求时，它就像自己拥有资源的源服务器一样对请求进行处理

隧道：
隧道是在相隔甚远的客户端和服务器两者之间进行中转，并保持双方通信连接的应用程序

### 代理
![](https://note.youdao.com/yws/res/0/WEBRESOURCE6ac2fb802a18433c972e254078be591e)

代理服务器的基本行为就是接受客户端发送的请求后转发给其他服务器，代理不改变请求URI，会直接发送给前方持有资源的目标服务器

持有资源实体的服务器被称为源服务器。从源服务器返回的响应经过代理服务器后再传递给客户端

![](https://note.youdao.com/yws/res/0/WEBRESOURCEb1b96870fa75576f0863955598eb4c83)

在HTTP通信过程中，可级联多台代理服务器。请求和响应的转发会经过数台类似锁链一样连接起来的代理服务器。转发时，需要附加Via首部字段以标记出经过的主机信息

![](https://note.youdao.com/yws/res/0/WEBRESOURCE57c5eaa9258139fda67c74dfc0525db9)

代理有多种方法，按两种基准分类，一种是是否使用缓存，另一种是是否会修改报文

**缓存代理：**
代理转发响应时，缓存代理会预先把资源的副本保持在代理服务器上，当代理再次接受到对相同资源的请求时，就可以不从源服务器那里获取资源，而是将之前缓存的资源作为响应返回

**透明代理**
转发请求或响应时，不对报文做任何加工的代理类型被称为透明代理，反之对报文内容进行加工的代理被称为非透明代理

### 网关

![](https://note.youdao.com/yws/res/0/WEBRESOURCEe893f2567a4f66bd3e3d49024c5f6c4a)

网关的工作机制和代理相似，而网关能使通信线路上的服务器提供非HTTP协议服务
利用网关可以提高通信的安全性，因为可以在客户端和网关之间的通信线路上加密以确保连接的安全
比如网关可以连接数据库，使用SQL语句查询数据。在Web购物网站上进行信用卡结算时，网关可以和信用卡结算系统联动

### 隧道

隧道可按要求建立一条与其他服务器的通信线路，届时使用SSL等加密手段进行通信，隧道的目的是确保客户端能与服务器进行安全的通信

隧道本身不会去解析HTTP请求，请求保持原样中转给之后的服务器，隧道会在通信双方断开连接时结束

## 保持资源的缓存

缓存是指代理服务器或客户端本地磁盘内保持的资源副本。利用缓存可减少对源服务器的访问，因此节省了通信流量和通信时间。
缓存服务器是代理服务器的一种，并归类在缓存代理类型中，当代理转发从服务器返回的响应时，代理服务器将会保持一份资源的副本

![](https://note.youdao.com/yws/res/0/WEBRESOURCEc007d0d6ed0e1054d357df98a649cd9a)

## 缓存的有效期限

即使存在缓存，也会因为客户端的要求、缓存的有效期等因素，向源服务器确认资源的有效性，若判断缓存失效，缓存服务器将会再次从源服务器上获取“新”资源

![](https://note.youdao.com/yws/res/0/WEBRESOURCEd719160ab1cd6d2aaa6be14a31b89d25)

## 客户端的缓存
缓存不仅可以存在于缓存服务器内，还可以存在客户端浏览器中。

和服务器缓存相似
浏览器缓存如果有效，就不必再向服务器请求相同的资源，可以直接从本地磁盘内读取
当判定缓存过期后，会向源服务器确认资源的有效性，若判断浏览器缓存失效，浏览器会再次请求新资源

![](https://note.youdao.com/yws/res/0/WEBRESOURCE5f13960b78b1d9ea9e658d5dd8c70ce2)

# HTTP首部

![](https://note.youdao.com/yws/res/0/WEBRESOURCE9c1c4fe48bec99051a3e05fa631f3073)

HTTP协议的请求和响应报文中必定包含HTTP首部
首部内容为客户端和服务器分别处理请求和响应提供所需要的信息

报文首部由几个字段构成：
## HTTP请求报文

在请求中，HTTP报文由方法、URI、HTTP版本、HTTP首部字段等部分构成

![](https://note.youdao.com/yws/res/0/WEBRESOURCE21e83e024615ccf096c45cc1549bf2e4)

实例：访问http://hackr.jp时，请求报文的首部信息

![](https://note.youdao.com/yws/res/0/WEBRESOURCE339db5ed2e0b6b1d31c2ebe6d12da781)
![](https://note.youdao.com/yws/res/0/WEBRESOURCEa46c04e30fe371cecf22c8078838358f)

## HTTP响应报文

在响应中，HTTP报文由HTTP版本、状态码（数字和原因短语）、HTTP首部字段3部分构成

![](https://note.youdao.com/yws/res/0/WEBRESOURCE7fb3e454ac3d50c4512196da3d0fdf5e)

实例：访问http://hackr.jp/时，返回的响应报文的首部信息

![](https://note.youdao.com/yws/res/0/WEBRESOURCEf4707df99d65cbd641d142643941810f)

## HTTP首部字段

HTTP首部字段构成HTTP报文的要素之一，在客户端与服务器之间以HTTP协议进行通信的过程中，无论是请求还是响应都会使用首部字段，它能起到传递额外重要信息的作用

使用首部字段是为了给浏览器和服务器提供报文主体大小、所使用的语言、认证信息等内容

![](https://note.youdao.com/yws/res/0/WEBRESOURCE1c8ef689f2cb2752c8b7f866e4c517c5)

### HTTP首部字段结构

HTTP首部字段是由首部字段名和字段值构成的，中间用冒号“：”分隔

`首部字段名：字段值`

例如，在HTTP首部中以Content-Type 这个字段来表示报文主体的对象类型

`Content-Type: text/html`

首部字段名：Content-Type, 字符串：text/html

另外字段值对应单个HTTP首部字段可以有多个值
`Keep-Alive: timeout=15, max=100`

## 4中HTTP首部字段类型

HTTP首部字段根据实际用途被分为4种类型

**通用首部字段**
请求报文和响应报文两方都会使用的首部

**请求首部字段**
从客户端向服务器端发送请求报文时使用的首部。
补充了请求的附加内容、客户端信息、响应内容相关优先级等信息

**响应首部字段**
从服务器端向客户端返回响应报文时使用的首部。
补充了响应的附加内容，也会要求客户端附加额外的内容信息

**实体首部字段**
针对请求报文和响应报文的实体部分使用的首部。
补充了资源内容更新实际等与实体相关的信息

## HTTP/1.1首部字段一览

HTTP/1.1规范定义了如下47种首部字段

![](https://note.youdao.com/yws/res/0/WEBRESOURCE2e0952d76723a8d32d0e64630dd08c79)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE536f237a049477e55e2aa3ba10ffb4e4)

![](https://note.youdao.com/yws/res/0/WEBRESOURCEbde64f6e1d090d0f41b08bd1d345970b)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE7a164cb14316212310939446bebf83b0)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE5f3f5b6ebfe18e713eeb05e7c690d75e)

## 非HTTP/1.1首部字段

在HTTP协议通信交互中使用到的首部字段，不限于RFC2616定义的47种首部字段，还有Cookie、Set-Cookie和Content-Disposition等在其他RFC中定义的首部字段

**End-to-end首部和Hop-by-hop首部**
HTTP首部字段将定义成缓存代替和非缓存代理的行为，分为两种类型

端到端首部：
分在此类别中的首部会转发给请求/响应对应的最终接受目标，且必须保存在由缓存生成的响应中，另外规定它必须被转发

逐跳首部：
分在此类别中的首部只对单次转发有效，会因通过缓存或代理而不再转发
如果要使用该首部，需提供Connection首部字段

以下为HTTP/1.1中的逐跳首部字段。除这8个首部字段之外，其他所有字段都属于端到端首部

![](https://note.youdao.com/yws/res/0/WEBRESOURCE790884cbf99a1ad549120201af16dcae)

## 通用首部字段
通用首部字段是指，请求报文和响应报文双方都会使用的首部

## **Cache-Control**
通过指定首部字段Cache-Control的指令，就能操作缓存的工作机制

![](https://note.youdao.com/yws/res/0/WEBRESOURCEdfd2ba4cbe667e231d115080356df9b5)

指令的参数是可选的，多个指令之间通过“，”分隔。首部字段Cache-Control的指令可用于请求及响应时

`Cache-Control: private, max-age=0, no-cache`

Cache-Control指令一览
可用的指令按请求和响应分类为：

![](https://note.youdao.com/yws/res/0/WEBRESOURCEcfffc69b4af1500e059cf7de9afd16dd)

![](https://note.youdao.com/yws/res/0/WEBRESOURCE67a0c8f8842557b0f04ce3fe06f241ec)

### 表示是否能缓存的指令

**public指令**
`Cache-Control: public`

当指定使用public指令时，则明确表明其他用户也可利用缓存

**private指令**
![](https://note.youdao.com/yws/res/0/WEBRESOURCEda8225752206d66b75a1c56651a28047)

当指定private指定后，响应只以特定的用户作为对象，和public指令的行为相反
缓存服务器会对该特定用户提供资源缓存的服务，对其他用户发来的请求，代理服务器则不会返回缓存

**no-cache指令**
![](https://note.youdao.com/yws/res/0/WEBRESOURCEe29cf84f1316a04c4ca6d2763a45533a)

使用no-cache指令的目的是为了防止从缓存中返回过期的资源

客户端发送的请求中如果包含no-cache指令，则表示客户端将不会接受缓存过的响应，于是，“中间”的缓存服务器必须把客户端请求转发给源服务器

如果服务器返回的响应中包含no-cache指令，那么缓存服务器不能对资源进行缓存，源服务器以后也将不再对缓存服务器请求中提出的资源有效性进行确认，且禁止对其响应资源进行缓存操作

`Cache-Control: no-cache=Location`

由服务器返回的响应中，若报文首部字段Cache-Control中对no-cache字段名具体指定参数值，那么客户端在接收到这个被指定参数值的首部字段对应的响应报文后，就不能使用缓存，换言之，无参数值的首部字段可以使用缓存。
注意：**只能在响应指令中指定该参数**


### 控制可执行缓存的对象的指令


**no-store指令**

`Cache-Control: no-store`

当使用no-store指令时，暗示请求（和对应的响应）或响应中包含机密信息

因此该指令规定缓存不能在本地存储请求或响应的任一部分


### 指定缓存期限和认证的指令

**s-maxage指令**

`Cache-Control: s-maxage=604800(单位：秒)`

s-maxage指令的功能和max-age指令相同，它们的不同点是s-maxage指令只适用于供多位用户使用的公共缓存服务器。也就是说，对于向同一用户重复返回响应的服务器来说，这个指令没有任何作用
另外使用s-maxage指令后，直接忽略对Expires首部字段及max-age指令的处理

**max-age指令**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE758355f4a2939b27f640177852895298)

当客户端发送的请求中包含max-age指令时，如果判定缓存资源的缓存时间数值比指定时间的数值小，那么客户端就接受缓存的资源。
当指定max-age值为0，那么缓存服务器通常需要将请求转发给源服务器

当服务器返回的响应中包含max-age指令时，缓存服务器将不对资源的有效性再作确定，而max-age数值代表资源保存为缓存的最长时间

HTTP/1.1版本的缓存服务器遇到同时存在Expires首部字段的情况时，会优先处理max-age指令，而忽略掉Expires首部字段，而HTTP/1.0版本的缓存服务器情况相反，max-age指令会被忽略掉

**min-fresh指令**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE34c6ed4418a4a10d567f9f6a62f69790)

min-fresh指令要求缓存服务器返回至少还未过指定时间的缓存资源
比如当指定min-fresh为60秒后，过了60秒的资源都无法作为响应返回了

**max-stale指令**

使用max-stale可指示缓存资源，即使过期也找出接收
如果指令未指定参数值，那么无论经过多久，客户端都会接受响应
如果指令中指定了具体数值，那么即使过期，只要仍处于max-stale指定的时间内，仍旧会被客户端接受

**only-if-cached指令**

`Cache-Control: only-if-cached`

使用only-if-cached指令表示客户端仅在缓存服务器本地缓存目标资源的情况下才会要求其返回，换言之，该指令要求缓存服务器不重新加载响应，也不会再次确认资源有效性
若发送请求缓存服务器的本地缓存无响应，则返回状态码504Gateway Timeout

**must-revalidate指令**
`Cache-Control: must-revalidate`

使用must-revalidate指令，代理会向源服务器再次验证即将返回的响应缓存目前是否仍然有效。
若代理无法连通源服务器再次获取有效资源的话，缓存必须给客户端一条504（Gateway Timeout状态码）

另外，使用must-revalidate指令会忽略请求的max-stale指令（即使已经在首部使用了max-stale，也不再有效果）

**proxy-revalidate指令**

`Cache-Control: proxy-revalidate`

proxy-revalidate 指令要求所有的缓存服务器在接收到客户端带有该指令的请求返回响应之前，必须再次验证缓存的有效性

**no-transform指令**

`Cache-Control: no-transform`

使用 no-transform 指令规定无论是在请求还是响应中，缓存都不能改变实体主体的媒体类型，这样做可以防止缓存或代理压缩图片等类似操作

**cache-extension token**

`Cache-Control: private, community="UCI"`

通过cache-extension标记（token），可以扩展Cache-Control首部字段内的指令

例如上例，Cache-Control 首部字段本身没有community这个指令，借助extension tokens实现了该指令的添加。
如果缓存服务器不能理解community这个指令，就会直接忽略，因此，extension tokens仅对能理解它的缓存服务器来说是有意义的

## **Connection**

Connection首部字段具备如下两个作用：
1. 控制不再转发给代理的首部字段
2. 管理持久连接

1:
![](https://note.youdao.com/yws/res/0/WEBRESOURCE40da1874881724fcbdf6ee73518444f2)

在客户端发送请求和服务器返回响应内，使用Connection首部字段，可控制不再转发给代理的首部字段

2：
![](https://note.youdao.com/yws/res/0/WEBRESOURCEcf0e4cdf7ad2977ee39a10c5116544a4)

HTTP/1.1版本的默认连接都是持久连接，为此客户端会在持久连接上连续发送请求，当服务器端想明确断开连接时，则指定Connection首部字段的值为Close

![](https://note.youdao.com/yws/res/0/WEBRESOURCE69bced78132c8cf69a4b683c986ad9fe)

HTTP/1.1之前的HTTP版本的默认连接都是非持久连接，为此如果想要旧版本的HTTP协议上维持持续连接，则需要指定Connection首部字段的值为Keep-Alive

## **Date**

首部字段Date表明创建HTTP报文的日期和时间

![](https://note.youdao.com/yws/res/0/WEBRESOURCEe82597032424c904f6c0304bb23d2f18)

HTTP/1.1协议使用在RFC1123中规定的日期时间的格式：

`Date: Tue, 03 Jul 2012 04:40:59 GMT`

之前的HTTP协议版本中使用在RFC850中定义的格式：

`Date: Tue, 03-Jul-12 04:40:59 GMT`

除此之外，还有一种格式，它与C标准库内的asctime()函数的输出格式一致：

`Date: Tue Jul 03 04:40:59 2021`

## **Pragma**

Pragma 是HTPP/1.1之前版本的历史遗留字段，仅作为与HTTP/1.0的向后兼容而定义

规范定义的形式唯一，如图：

`Pragma: no-cache`

该首部字段属于通用首部字段，但只用在客户端发送的请求中，客户端会要求所有的中间服务器不返回缓存的资源

![](https://note.youdao.com/yws/res/0/WEBRESOURCE14b385c90a6c32e6b8f865d99ad3bee7)

采用 Cache-Control:no-cache指定缓存的处理方式最为理想
发送的请求可能会同时含有下面两个首部字段：
```
Cache-Control: no-cache
Pragma: no-cache
```

## **Trailer**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE79ec9ac1710e23e0cf87bcaee6ba71c5)

首部字段Trailer会事先说明在报文主体后记录了哪些首部字段。
该首部字段可应用在分块传输编码时

![](https://note.youdao.com/yws/res/0/WEBRESOURCEa09762c3b44e7ec9496bee75696771d5)
![](https://note.youdao.com/yws/res/0/WEBRESOURCE91cd04d2a65e45c83fd553c6ee962170)

以上例子，指定首部字段Trailer的值为Expires，在报文主体之后（分块长度0之后）出现了首部字段Expires

## **Transfer-Encoding**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE7167549d95e9d977834db5beb855ace7)

首部字段 Transfer-Encoding 规定了传输报文主体时，采用的编码方式

HTTP/1.1的传输编码方式仅对分块传输编码有效

![](https://note.youdao.com/yws/res/0/WEBRESOURCE335c31c6802b66f2c8fc1fa1ccaf39d7)
![](https://note.youdao.com/yws/res/0/WEBRESOURCEa59fef81d89dc864cc919f0a77ba5eeb)

以上用例中，正如在首部字段 Transfer-Encoding 中指定的那样，有效使用分块传输编码，切分别被分成3312字节和914字节大小的分块数据

## **Upgrade**

首部字段 Upgrade 用于检测HTTP协议及其他协议是否可使用更高版本进行通信，其参数值可以用来指定一个完全不同的通信协议

![](https://note.youdao.com/yws/res/0/WEBRESOURCE551fa4e89750149cfdc2e7af41409352)

以上用例，首部字段Upgrade指定的值为TLS/1.0
注意两个字段首部字段的对应关系，Connection的值被指定为Upgrade
Upgrade首部字段产生作用的Upgrade对象仅限于客户端和邻接服务器之间

对于附有首部字段Upgrade的请求，服务器可用101 Switching Protocols状态码作为响应返回

## **Via**

使用首部字段 Via 是为了追踪客户端与服务器之间的 .请求和响应报文的. 传输路径
报文经过代理或网关时，会先在首部字段 Via 中附加该服务器的信息，然后再进行转发
首部字段 Via 不仅用于追踪报文的转发，还可避免请求回环的发生，所以必须在经过代理时附加该首部字段内容

![](https://note.youdao.com/yws/res/0/WEBRESOURCE4ec6974932d365aadbd2894ed3129903)

上例：在经过代理服务器A时，Via 首部附加了“1.0 gw.hackr.ip(Squid/3.1)”这样的字符串值。
行头的1.0是指接受请求的服务器上应用的HTTP协议版本
接下来经过代理服务器B时也是这样，在 Via 首部附加服务器信息，也可增加1个新的Via首部写入服务器信息

Via首部是为了追踪传输路径，所以经常会和TRACE方法一起使用，比如：
代理服务器接受到由TRACE方法发送过来的请求（期终Max-Forwards:0）时，代理服务器就不能再转发该请求了，这种情况下，代理服务器会将自身的信息附加到Via首部后，返回该请求的响应


## **Warning**

HTTP/1.1的Warning首部是从HTTP/1.0的响应首部（Retry-After）演变过来的
该首部通常会告知用户一些与缓存相关的问题和警告

![](https://note.youdao.com/yws/res/0/WEBRESOURCEb33d214f0870bbec8dfb7f4b680f6f77)

Warning首部的格式如下：（最后的日期时间部分可以忽略）

![](https://note.youdao.com/yws/res/0/WEBRESOURCEd31cc0e41e2801f5558644290f4bb5ec)

HTTP/1.1中定义了7种警告（警告码具备扩展性，今后可能会追加新的）

![](https://note.youdao.com/yws/res/0/WEBRESOURCEb844010dd6bd11bfe6bfe5e966c854c1)
![](https://note.youdao.com/yws/res/0/WEBRESOURCEcc057fb35cd64c5a8c677de89655f006)

## 请求首部字段

请求首部字段是从客户端往服务器端发送请求报文中所使用的字段
用于补充请求的附加信息、客户端信息、对响应内容相关的优先级等内容

![](https://note.youdao.com/yws/res/0/WEBRESOURCEc226234dffa5aad92ced05031d165752)

## **Accept**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE72cbf54c000f60f6addb539323d893c2)

Accept首部字段可通知服务器，用户代理能够处理的媒体类型及媒体类型的相对优先级，可以使用type/subtype这种形式，一次指定多种媒体类型

例如：
![](https://note.youdao.com/yws/res/0/WEBRESOURCE3bcd560a7799f8035218c84fe5c4cb45)

例：如果浏览器不支持PNG图片的显示，那Accept就不指定image/png，而指定可处理的image/gif,image/jpeg等图片类型

如果想给显示的媒体类型增加优先级，则使用q=来额外表示权重值，用分号（；）进行分隔，权重值范围0-,1最大，不指定q时默认权重为q=1.0

## **Accept-Charset**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE489d887602a0de080296f337ada2f526)

Accept-Charset首部字段可用来通知服务器用户代理支持的字符集及字符集的相对优先顺序，可一次性指定多种字符集，与首部字段Accept相同的是可用权重q值来表示相对优先级

该首部字段应用于内容协商机制的服务器驱动协商

## **Accept-Encoding**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE921dc95070a90026e37c6daaf515d411)

Accept-Encoding 首部字段用来告知服务器用户代理支持的内容编码及内容编码的优先级

例：
![](https://note.youdao.com/yws/res/0/WEBRESOURCE60d319b4912c4b3ee67562974258ec70)
![](https://note.youdao.com/yws/res/0/WEBRESOURCE5cd3e8a98eb367a7732b1cf5e7c52bcd)

采用权重q值来表示相对优先级，也可以使用星号（*）作为通配符，指定任意的编码格式

## **Accept-Language**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE8752518a70f40532def74a105dca2683)

首部字段Accept-Language 用来告知服务器用户代理能够处理的自然语言集，以及自然语言集的相对优先级。可一次指定多种自然语言集

权重值q表示相对优先级

## **Authorization**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE951d508565792fb507b173bb614739cb)

首部字段Authorization 是用来告知服务器，用户代理的认证信息。
通常，想要通过服务器认证的用户代理会在接收到返回的401状态码响应后，把首部字段Authorization加入请求中

## **Expect**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEe86a9c7c5e8b2845a4dba2fa7691b9a7)

客户端使用首部字段Expect来告知服务器，期望出现的某种特定行为
因服务器无法理解客户端的期望作出回应而发生错误时，会返回状态码417Expectaion Failed


## **From**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE6bcd8a7d49ddb4ae0f195e1e22161222)

首部字段From 用来告知服务器使用用户代理的用户的电子邮箱地址
通常，其使用目的是为了显示搜索引擎等用户代理的负责人的电子邮件联系方式，使用代理时，尽可能包含From首部字段（但可能会因为代理不同，将电子邮件地址记录在User-Agent首部字段内）

## **Host**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEc6200568343ac186959130e2c0419e97)
![](https://note.youdao.com/yws/res/0/WEBRESOURCE60ec6ebbbf213e50848dbe8428323bb5)

首部字段Host会告知服务器，请求的资源所处的互联网主机名和端口号
**Host首部字段在HTTP/1.1规范内是唯一一个必须被包含在请求内的首部字段**

首部字段Host和以单台服务器分配多个域名的虚拟主机的工作机制有很密切的关联，这是首部字段Host必须存在的意义

请求被发送至服务器时，请求中的主机名会用IP地址直接替换解决，但如果这时，相同的IP地址下部署运行着多个域名，那么服务器就会无法理解究竟是哪个域名对应的请求，因此，就需要使用首部字段Host来明确指出请求的主机名，如果服务器未设定主机名，那直接发送一个空值即可

## **If-Match**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE238105d32c0317a5d2e2896e5bcde9d5)

形如If-xxx这种样式的请求首部字段，都可称为条件请求。
服务器接受到附带条件的请求后，只有判断条件为真，才会执行请求

![](https://note.youdao.com/yws/res/0/WEBRESOURCE99965f829a862418aa725c18bec22859)

首部字段If-Match ，属附带条件之一，它会告知服务器匹配资源所用的实体标记值，这时的服务器无法使用弱ETag值
服务器会对比If-Match 的字段值和资源的 ETag值，仅当两者一致时，才会执行请求。反之，则返回状态码412 Precondition Failed的响应

还可以使用星号（*）指定If-Match 的字段值。对于这种情况，服务器会忽略ETag 的值，只要资源存在就处理请求

## **If-Modified-Since**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE71410122a9996a6eb36186e877e944f3)

首部字段If-Modified-Since，属附带条件之一，它会告知服务器若If-Modified-Since 字段值早于资源的更新时间，则希望能处理该请求，而在指定If-Modified-Since 字段值的日期时间之后，如果请求的资源都没有过更新，则返回状态码304 Not Modified 的响应

If-Modified-Since 用于确认代理或客户端拥有的本地资源的有效性
可通过确认首部字段 **Last-Modified**来确定

## **If-None-Match**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEd1e2a0f41fe376fff773e40a5655b7fc)

首部字段 If-None-Match 属于附带条件之一。它和首部字段 If-Match 作用相反。
用于指定 If-None-Match 字段值的实体标记（ETag）值与请求资源的 ETag 不一致时，它就告知服务器处理该请求

在 GET 或 HEAD 方法中使用首部字段 If-None-Match 可获取最新的资源，因此，这与使用首部字段 If-Modified-Since 时有些类似

## **If-Range**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEf0142fbbeee88e7f8f311ac60bc7ae47)

首部字段 If-Range 属于附带条件一个，它告知服务器若指定的 If-Range 字段值（ETag值或时间）和请求资源的ETag 值或时间相一致时，则作为范围请求处理，反之，则返回全体资源

![](https://note.youdao.com/yws/res/0/WEBRESOURCEa5b383aa58480c068182b55587b8721e)

如果不适用首部字段If-Range发送请求的情况。服务器端的资源如果更新，那客户端持有资源中的一部分也随之失效，当然，范围请求作为前提是无效的。这时，服务器会暂且以状态码412 Precondition Failed 作为响应返回，其目的是催促客户端再次发送请求。这样一来，与使用首部字段 If-Range 相比，就需要两倍功夫

## **If-Unmodified-Since**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEccb99542489f4826b9c06dad8767d1f6)

首部字段 If-Unmodified-Since 和首部字段 If-Modified-Since 的作用相反。
它的作用是告知服务器，指定的请求资源只有在字段值内指定的日期时间，未发生更新的情况下，才能处理请求。如果在指定日期时间之后发生了更新，则以状态码412 Precondition Failed 作为响应返回

## **Max-Forwards**

![](https://note.youdao.com/yws/res/0/WEBRESOURCE0d601c55063f9ce04f93ff8a15a00345)

通过 TRACE 方法或 OPTIONS 方法，发送包含首部字段 Max-Forwards 的请求时，该字段以十进制整数形式指定可经过的服务器最大数目。服务器在往下一个服务器转发请求之前，Max-Forwards 的值减 1 后重新赋值。当服务器接收到 Max-Forwards 值为0的请求时，则不再进行转发，而是直接返回响应

使用HTTP协议通信时，请求可能会经过代理等多台服务器，途中，如果代理服务器由于某些原因导致请求转发失败，客户端也就等不到服务器返回的响应了，对此无从可知，所以可以灵活使用首部字段 Max-Forwards ，针对以上问题产生的原因展开调查，由于当Max-Forwards 字段值为0时，服务器就会立即返回响应，由此我们至少可以对以那台服务器为终点的传输路径的通信状况有把握

![](https://note.youdao.com/yws/res/0/WEBRESOURCE896742025982100ca8bcf6767bc03189)

## **Proxy-Authorization**

![](https://note.youdao.com/yws/res/6833/WEBRESOURCE3b918aa6ec556bba65a1f00e946e6d4a)

接受到从代理服务器发来的认证质询时，客户端会发送包含首部字段 Proxy-Authorization 的请求，以告知服务器认证所需要的信息

这个行为是与客户端和服务器之间的HTTP访问认证相类似的，不同点在于，认证行为发生在客户端与代理之间。客户端与服务器之间的认证，使用首部字段 Authorization 可起到相同作用

## **Range**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEf919986c4b6717320f6555b1ea234583)

 对于只需获取部分资源的范围请求，包含首部字段Range既可告知服务器资源的指定范围。
 上面实例表示请求获取从第5001字节至第10000字节的资源
 
 接受到附带Range首部字段请求的服务器，会在处理请求之后返回状态码为206 Partial Content 的响应，无法处理该范围请求时，则会返回状态码 200 OK 的响应及全部资源
 
 ## **Referer**
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE8dfa6519eeade3719a436512de44d15f)
 
 首部字段Referer会告知服务器请求的原始资源的URI
 客户端一般都会发送 Referer 首部字段给服务器。但当直接在浏览器的地址栏输入URI，或出于安全性的考虑时，也可以不发送该首部字段
 
 因为原始资源的URI 中的查询字符串可能含有ID和密码等保密信息，要是写进 Referer转发给其他服务器，则可能导致保密信息的泄漏
 
 ## **TE**
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE455c377e6523221e1430c983ac60fc39)
 
 首部字段TE会告知服务器 客户端能够处理响应的传输编码方式及相对优先级。
 它和首部字段 Accept-Encoding 的功能很相像，但是用于传输编码
 
 首部字段TE除指定传输编码外，还可以指定伴随trailer字段的分块传输编码方式。应用后者时，只需把trailers赋值给该字段值
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCEec7bff4d6fdf379d6256352cd91d3a54)
 
 ## **User-Agent**

![](https://note.youdao.com/yws/res/0/WEBRESOURCEd4936188bcab5bb25b109c5ef9c5c1ae) 
 
 
 首部字段 User-Agent 会将创建请求的浏览器和用户代理名称等信息传达给服务器
 
 由网络爬虫发起请求时，可能会在字段内添加爬虫作者的电子邮件地址，如果请求经过代理，那么中间也可能被添加上代理服务器的名称
 
 ## 响应首部字段
 
 响应首部字段是由服务器端向客户端返回响应报文中所使用的字段，用于补充响应的附加信息、服务器信息，以及对客户端的附加要求等信息
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE053f8430ae17bbfb28fb64611ccccf88)
 
 ## **Accept-Ranges**
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCEc47cc1bb10829ccf9432145ff25d44de)
 
 首部字段 Accept-Ranges 是用来告知客户端服务器是否能处理范围请求，以指定获取服务器端某个部分的资源
 
 可指定的字段值有两种，可处理范围请求时指定其为bytes，反之则指定其为none
 
 ## **Age**
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE8a8559f202a2d86a11489c290d912065)
 
 首部字段Age能告知客户端，源服务器在多久前创建了响应。字段值的单位为秒
 
 若创建该响应的服务器是缓存服务器，Age值是指缓存后的响应再次发起认证到认证完成的时间值，代理创建响应时必须加上首部字段Age
 
 ## **ETag**
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE2c45456ab934c3e78167384e2ede255a)
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE633f2eacdc9a6411d922b52a99fdd2f7)
 
 首部字段ETag能告知客户端实体标识。它是一种可将资源以字符串形式做唯一性标识的方式。服务器会为每份资源分配对应的ETag值
 
 另外，当资源更新时，ETag值也需要更新。生成ETag值时，并没有统一的算法规则，而仅仅是由服务器来分配
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE98d4aec48978843776eab8d4c1bd290d)
 
 资源被缓存时，就会被分配唯一性标识。
 
 强ETag值和弱Tag值
 
 强ETag值：不论实体发生多么细微的变化都会改变其值
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE92cc6f0524fd6ea3b88c57e5036fe284)
 
 弱ETag值：弱ETag值只用于提示资源是否相同。只有资源发生了根本改变，产生差异时才会改变ETag值，这时，会在字段值最开始出附加 W/
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE73872a61d1a7fa770b093f5454fd7d99)
 
 ## **Location**
 
 ![](https://note.youdao.com/yws/res/0/WEBRESOURCE279203b8b7d20e71295942759aa9bf9e)
 
 使用首部字段Location可以将响应接收方引导至某个与请求URI位置不同的资源
 
 基本上，该字段会配合3xx：Redirection的响应，提供重定向的URI
 
 几乎所有的浏览器在接收到包含首部字段Location的响应后，都会强制性地尝试对已提示的重定向资源的访问
 
 123页后笔记在PDF上