# 图解 HTTP 笔记

##### [Essay](https://dixinl.github.io/Essay/)

#### 请求报文首部字段 Accept

- 文本文件
  - text/html
  - text/plain
  - text/css ... 
  - application/xhtml+xml
  - application/xml ... 
- 图片文件
  - image/jpeg
  - image/gif
  - image/png ... 
- 视频文件
  - video/mpeg
  - video/quicktime ... 
- 应用程序使用的二进制文件
  - application/octet-stream
  - application/zip ... 

## HTTP 不足

- 通信使用明文（不加密），内容可能会被窃听

- 不验证通信方的身份，因此有可能遭遇伪装

- 无法证明报文的完整性，所以有可能已遭篡改 

- 无法确定请求发送至目标的 **Web** 服务器是否是按真实意图返回响应的那台服务器。有可能是已伪装的 **Web** 服务 

  器。

- 无法确定响应返回到的客户端是否是按真实意图接收响应的那个客户端。有可能是已伪装的客户端。 

- 无法确定正在通信的对方是否具备访问权限。因为某些 **Web** 服务器上保存着重要的信息，只想发给特定用户通 

  信的权限。

- 无法判定请求是来自何方、出自谁手。即使是无意义的请求也会照单全收。无法阻止海量请求下的 **DoS** 攻击（**Denial of Service**，拒绝服务攻击）。 

## HTTPS

HTTP 协议中没有加密机制，但可以通过和 SSL（Secure Socket Layer，安全套接层）或 TLS（Transport Layer Security，安全层传输协议）的组合使用，加密 HTTP 的通信内容。 

用 SSL建立安全通信线路之后，就可以在这条线路上进行 HTTP 通信了。与 SSL组合使用的 HTTP 被称为 HTTPS（HTTP Secure，超文本传输安全协议）或 HTTP over SSL。 

![1565690038502](./images/1565690038502.png)

### SSL

SSL采用一种叫做公开密钥加密（Public-key cryptography）的加密处理方式。使用两把密钥的公开密钥加密方式很好地解决了共享密钥加密的困难。 

公开密钥加密使用一对非对称的密钥。一把叫做私有密钥（private key），另一把叫做公开密钥（public key）。顾名思义，私有密钥不能让其他任何人知道，而公开密钥则可以随意发布，任何人都可以获得。 

使用公开密钥加密方式，发送密文的一方使用对方的公开密钥进行加密处理，对方收到被加密的信息后，再使用自己的私有密钥进行解密。利用这种方式，不需要发送用来解密的私有密钥，也不必担心密钥被攻击者窃听而盗走。

#### **SSL证书认证方式**

![1565694711159](./images/1565694711159.png)

#### HTTPS 通信方式

![1565694670564](./images/1565694670564.png)

- 步骤 **1**： 客户端通过发送 Client Hello 报文开始 SSL通信。报文中包含客户端支持的 SSL的**指定版本、加密组件（Cipher Suite）列表（所使用的加密算法及密钥长度等）**。 

- 步骤 **2**： 服务器可进行 SSL通信时，会以 **Server Hello 报文**作为应答。和客户端一样，在报文中**包含 SSL 版本以及加密组件。服务器的加密组件内容是从接收到的客户端加密组件内筛选出来的**。 

- 步骤 **3**： 之后服务器发送 Certificate 报文。报文中包含**公开密钥证书**。

- 步骤 **4**： 最后服务器发送 **Server Hello Done 报文**通知客户端，**最初阶段的 SSL握手协商**部分结束。 

- 步骤 **5**： SSL第一次握手结束之后，客户端以 Client Key Exchange 报文作为回应。**报文中包含通信加密中使用的一种被称为 Pre-master secret 的随机密码串。该报文已用步骤 3 中的公开密钥进行加密**。 

- 步骤 **6**： 接着**客户端继续发送 Change Cipher Spec 报文**。该报文会**提示服务器，在此报文之后的通信会采用 Pre-master secret 密钥加密**。 

- 步骤 **7**： 客户端发送 Finished 报文。**该报文包含连接至今全部报文的整体校验值。这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准**。 

- 步骤 **8**： 服务器同样发送 Change Cipher Spec 报文。 

- 步骤 **9**： 服务器同样发送 Finished 报文。 

- 步骤 **10**： **服务器和客户端的 Finished 报文交换完毕之后，SSL连接就算建立完成**。当然，通信会受到 SSL的保护。**从此处开始进行应用层协议的通信，即发送 HTTP 请求**。 

- 步骤 **11**： 应用层协议通信，即发送 HTTP 响应。 

- 步骤 **12**： 最后**由客户端断开连接**。断开连接时，发送 close_notify 报文。上图做了一些省略，**这步之后再发送 TCP FIN 报文来关闭与 TCP的通信**。 

在以上流程中，应用层发送数据时会附加一种叫做 MAC（Message Authentication Code）的报文摘要。MAC 能够查知报文是否遭到篡改，从而保护报文的完整性。 

#### Session 管理及 Cookie 的应用

因为 HTTP 是无状态协议，即使当该用户下一次继续访问，也无法区分他与其他的用户。于是我们会使用 Cookie 来管理 Session，以弥补 HTTP 协议中不存在的状态管理功能。 

为减轻跨站脚本攻击（XSS）造成的损失，建议事先在 Cookie 内加上 httponly 属性。

![1565695947001](./images/1565695947001.png)

## 当前HTTP的改进方法

#### Ajax

Ajax 的核心技术是名为 XMLHttpRequest 的 API，通过 JavaScript 脚本语言的调用就能和服务器进行 HTTP 通信。

但Ajax 仍未解决 HTTP 协议本身存在的问题。 

![1565740228336](./images/1565740228336.png)

#### Commet

通常，服务器端接收到请求，在处理完毕后就会立即返回响应，但为了实现推送功能，Comet 会先将响应置于挂起状态，当服务器端有内容更新时，再返回该响应。

内容上虽然可以做到实时更新，但为了保留响应，一次连接的持续时间也变长了。期间，为了维持连接会消耗更多的资源。另外，Comet 也仍未解决 HTTP 协议本身存在的问题。 

![1565740142429](./images/1565740142429.png)

#### Websocket

一旦建立起 WebSocket 协议的通信连接，之后所有的通信都依靠这个专用协议进行。

- 推送功能

  支持由服务器向客户端推送数据的推送功能。这样，服务器可直接发送数据，而不必等待客户端的请求。 

- 减少通信量

  只要建立起 WebSocket 连接，就希望一直保持连接状态。和 HTTP 相比，不但每次连接时的总开销减少，而且由于 WebSocket 的首部信息很小，通信量也相应减少了。 

JavaScript 可调用“The WebSocket API”（http://www.w3.org/TR/websockets/ ，由 W3C 标准制定）内提供的 WebSocket 程序接口，以实现 WebSocket 协议下全双工通信。

![1565740073561](./images/1565740073561.png)

#### HTTP2.0

HTTP/2.0 在 2014 年 11 月实现标准化。 

- **SPDY** 

- **HTTP Speed** ＋ **Mobility** ：HTTP Speed ＋ Mobility 由微软公司起草，是用于改善并提高移动端通信时的通信速度和性能的标准。它建立在 Google 公司提出的 SPDY 与 WebSocket 的基础之上。 

- **Network-Friendly HTTP Upgrade** ：Network-Friendly HTTP Upgrade 主要是在移动端通信时改善 HTTP 性能的标准。 

|                        方向                         |            技术            |
| :-------------------------------------------------: | :------------------------: |
|                        压缩                         |       SPDY、Friendly       |
|                      多路复用                       |            SPDY            |
|                     TLS 义务化                      |      Speed＋ Mobility      |
|                        协商                         | Speed＋ Mobility，Friendly |
| 客户端拉曳（Client Pull）/服务器推送（Server Push） |      Speed＋ Mobility      |
|                      流量控制                       |            SPDY            |
|                      WebSocket                      |      Speed＋ Mobility      |

注：HTTP Speed ＋ Mobility 简写为 Speed ＋ Mobility，Network-Friendly HTTP Upgrade 简写为 Friendly。 

## Web应用

#### CGI

CGI（Common Gateway Interface，通用网关接口）是指 Web 服务器在接收到客户端发送过来的请求后转发给程序的一组机制。在 CGI 的作用下，程序会对请求内容做出相应的动作，比如创建 HTML等动态内容。 

使用 CGI 的程序叫做 CGI 程序，通常是用 Perl、PHP、Ruby 和 C 等编程语言编写而成。 

![1565748288506](./images/1565748288506.png)

#### Servelt

Servlet 是一种能在服务器上创建动态内容的程序。Servlet 是用 Java 语言实现的一个接口，属于面向企业级 Java（JavaEE，Java Enterprise Edition）的一部分。 

之前提及的 CGI，由于每次接到请求，程序都要跟着启动一次。因此一旦访问量过大，Web 服务器要承担相当大的负载。而 Servlet 运行在与 Web 服务器相同的进程中，因此受到的负载较小（常驻内存）。Servlet 的运行环境叫做 Web 容器或 Servlet 容器。 

### 数据发布的格式及语言

- XML（eXtensible Markup Language，可扩展标记语言）是一种可按应用目标进行扩展的通用标记语言。

- RSS（简易信息聚合，也叫聚合内容）和 Atom 都是发布新闻或博客日志等更新信息文档的格式的总称。两者都用到了 XML。 

- JSON（JavaScript Object Notation）是一种以 JavaScript（ECMAScript）的对象表示法为基础的轻量级数据标记语言。能够处理的数据类型有 false/null/true/ 对象 / 数组 / 数字 / 字符串，这 7 种类型。 
