# Apache实用

### 使用zhonganSDK时出现的坑：

- 环境为Apache/2.4.23 (Win32) OpenSSL/1.0.2j PHP/5.4.45，使用phpstudy。

- **1.使用zhonganSDK传递参数时Apache HTTP server停止服务**

  > [Wed Oct 03 11:33:29.794770 2018] [mpm_winnt:crit] [pid 12532:tid 688] AH02538: Child: Parent process exited abruptly. Child process is ending
  >
  > [Wed Oct 03 11:35:02.476670 2018] [core:warn] [pid 14068:tid 740] AH00098: pid file D:/phpStudy16/Apache/logs/httpd.pid overwritten -- Unclean shutdown of previous Apache run
  >
  > [Wed Oct 03 11:35:02.665766 2018] [mpm_winnt:notice] [pid 14068:tid 740] AH00455: Apache/2.4.23 (Win32) OpenSSL/1.0.2j PHP/5.4.45 configured -- resuming normal operations
  >
  > [Wed Oct 03 11:35:02.665766 2018] [mpm_winnt:notice] [pid 14068:tid 740] AH00456: Server built: Jul  1 2016 16:42:20
  >
  > [Wed Oct 03 11:35:02.665766 2018] [core:notice] [pid 14068:tid 740] AH00094: Command line: 'D:\\phpStudy16\\Apache\\bin\\httpd.exe -d D:/phpStudy16/Apache'
  >
  > [Wed Oct 03 11:35:02.676962 2018] [mpm_winnt:notice] [pid 14068:tid 740] AH00418: Parent: Created child process 8672
  >
  > [Wed Oct 03 11:35:04.369157 2018] [mpm_winnt:notice] [pid 8672:tid 628] AH00354: Child: Starting 150 worker threads.

综合网上资料，该问题可能为以下几点：

- 1.代码使用子进程数过多，导致超过最大额度，Apache服务崩溃
- 2.父进程被杀死，子进程自动销毁
- 3.Apache与PHP版本不兼容
- 4.代码与PHP版本不兼容
- 5.服务器配置过低，跟不上你的场景
- 6.端口冲突基本以上几类常见问题。

1.2.两个情况简单了解了下，觉得应该不是这个问题。顺带1.2.解决思路：

- 修改 conf/extra/httpd-mpm.conf文件

  > ThreadsPerChild      150             #父进程下子进程数
  >
  > MaxRequestsPerChild    0          #子进程最大处理请求数量

3.问题暂时搁置，因为环境为phpstudy，兼容性不会有太大问题。

4.问题代码为众安demo，且明确提示PHP版本>=5.2，且7.0版本已经过测试。pass

5.服务器为1核/2G/40G/2008server服务器,配置问题不存在。

6.80端口、443端口、3306端口等主要端口皆经过查询未被占用。使用phpstudy2016版对版本控制使用PHP5.2+Apache2.4无法使用使用PHPstudy经典版php5.2+Apache2.2可以使用，故认定该问题为版本问题。由于对Apache操作不是很熟练，采用退版本操作，解压缩phpstudy经典版MySQL使用sql-font数据库，对数据库进行备份、恢复等操作

![1614308997157](D:\essay\Essay\images\1614308997157.png)

解决以上问题，对经典版phpstudy开启curl、开启openssl，问题解决。

解决一个坑，又有一个坑：apache的ssl认证缺失：

复制来原版的cert、conf/httpd.conf、vhosts_ssh.conf加入经典版，重启Apache，没反应

> LoadModule ssl_module modules/mod_ssl.so
>
> Include conf/vhosts_ssl.conf       #对其添加注释，可以开启apache

故确定问题，vhosts_ssh.conf文件代码错误。

源文件：

> Listen 443
>
> <VirtualHost *:443>
>
> DocumentRoot "C:\websoft\web\phpwind"
>
> ServerName [www.dixinl.com](http://www.dixinl.com/)
>
> ServerAlias [dixinl.com](http://dixinl.com/)
>
> SSLEngine on
>
> SSLProtocol TLSv1 TLSv1.1 TLSv1.2
>
> SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5
>
> SSLCertificateFile "C:/phpStudy52/Apache/cert/public.pem"
>
> SSLCertificateKeyFile "C:/phpStudy52/Apache/cert/214899548070975.key"
>
> SSLCertificateChainFile "C:/phpStudy52/Apache/cert/chain.pem"
>
> <Directory "C:\websoft\web\phpwind">
>
> Options +Indexes +FollowSymLinks +ExecCGI
>
> AllowOverride All
>
> Order allow,deny
>
> Allow from all
>
> Require all granted
>
> </Directory>
>
> </VirtualHost>

逐行确定问题，ssl协议问题

修改文中代码，重启Apache，开启ssl，可使用https    

> SSLProtocol all -SSLv2 -SSLv3
>
> SSLCipherSuite HIGH:!RC4:!MD5:!aNULL:!eNULL:!NULL:!DH:!EDH:!EXP:+MEDIUM