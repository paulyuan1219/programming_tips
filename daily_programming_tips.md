# 日常编程技巧

## ElasticSearch

**`localhost:9200/media_43tc/_search?pretty&q=people`**

+ `midea_43tc` is index name
+ `people` is the query

**`http://localhost:9200/_cat/indices?v`**

+ show all indices

## 虚拟机 vmwar virtual box
[尚硅谷大数据全套视频教程40阶段（2019.6月线下班）](https://www.bilibili.com/video/BV1BJ411U7hW?p=100) P100 05_Linux -03_安装CentOS6.8

[07、环境 虚拟机网络设置](https://www.youtube.com/watch?v=GrXzq8ZvcRc)




## git

[视觉化学习git](https://learngitbranching.js.org/?NODEMO) 一个很好地图形化教程，我还没有看完，刚刚开了个头



### 每次生成ticket的标准流程
```
Git checkout master
Git pull
Git checkout -b topic-mrxxx
...
git push -u origin topic-mr386

Git checkout — file name # 本地修改了一堆文件(并没有使用git add到暂存区)，想放弃修改 单个文件或者文件夹
Git checkout . # 所有文件夹
```

### git本地项目首次推送到远程
```
git init
// 将“README.md”文件保存至缓存区，实际开发中一般使用 `git add -A`
// 使用-A:将新增、删除、修改的文件改动全保存至缓存区
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/wteam-xq/testGit.git # origin就是远程仓库的名字，遥远的地方
git push -u origin master # 把本地的master分支推送到远程仓库origin上。实际应用中，把master替换成topic-mrxxx即可。 -u指定默认主机
```
+ [小姐姐用动图展示 10 大 Git 命令](https://yqh.aliyun.com/detail/9460?utm_content=g_1000116248)
+ 



## python

### [Module vs Package](https://realpython.com/python-modules-packages/)

There are actually **three** different ways to define a module in Python:

1. A module can be written in Python itself.
2. A module can be written in C and loaded dynamically at run-time, like the re (regular expression) module.
3. A built-in module is intrinsically contained in the interpreter, like the itertools module.

A module’s contents are accessed the same way in all three cases: with the import statement.

```
# mod.py

s = "If Comrade Napoleon says it, it must be right."
a = [100, 200, 300]

def foo(arg):
    print(f'arg = {arg}')

class Foo:
    pass
```

只要文件的位置合适，我们就可以引入这个module

```
import mod
print(mod.s)
...
```

使用import语句的时候，会在以下路径依次寻找

+ The directory from which the input script was run or the current directory if the interpreter is being run interactively
+ The list of directories contained in the PYTHONPATH environment variable, if it is set. (The format for PYTHONPATH is OS-dependent but should mimic the PATH environment variable.)
+ An installation-dependent list of directories configured at the time Python is installed

最终，详见`sys.path`

```
sys.path.append(...)
import mod
```

+ `mod.__file__` to show module file location
+ `import mod` and `mod.s`
+ `from mod import *`
+ `import mod as mod1`
+ `from mod import s as s1`
+ `dir()` list of names in the current **local symbol table** When given an argument that is the name of a module, dir() lists the names defined in the module:

```
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__']

>>> qux = [1, 2, 3, 4, 5]
>>> dir()
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'qux']

>>> class Bar():
...     pass
...
>>> x = Bar()
>>> dir()
['Bar', '__annotations__', '__builtins__', '__doc__', '__loader__', '__name__',
'__package__', '__spec__', 'qux', 'x']
```


任何py文件都可以当做脚本直接运行
一般来说import语句对同一个module多次运行，只会load一次。如果实在需要，可以使用`importlib.reload(mod)`

#### Package
+ 和之前类似，如果一个pkg folder放在`sys.path`中的一个目录下，那么我们就可以使用
    + `import pkg.mod1, pkg.mod2` and `pkg.mod1.foo()`
    + `from pkg.mod1 import foo` 
    + `from pkg.mod2 import Bar as Qux`
    + `import pkg`
        + 注意，如果pkg下面没有初始化文件的话，上面这句话没有任何意义
        + it doesn’t do much of anything useful. In particular, it does not place any of the modules in pkg into the local namespace
        + `pkg.mod1` fails
        + `pkg.mod1.foo()` fails
        + `pkg.mod2.Bar()` fails
        +  
+ If a file named __init__.py is present in a package directory, it is invoked when the package or a module in the package is imported. This can be used for execution of package initialization code, such as initialization of package-level data.
+ 如果__init__.py里面定义了变量A，那么就可以使用`import pkg` and `pkg.A` 来访问这个变量
+ 如果pkg目录下有一个模块mod1，那么就可以用`from pkg import mod1` and `mod1.foo()`
+ __init__.py can also be used to effect automatic importing of modules from a package. Put `import pkg.mod1, pkg.mod2` in this file. Then `import pkg` will auto invoke these `import` statements.


+ Note: Much of the Python documentation states that an __init__.py file must be present in the package directory when creating a package. This was once true. It used to be that the very presence of __init__.py signified to Python that a package was being defined. The file could contain initialization code or even be empty, but it had to be present.

+ Starting with Python 3.3, Implicit Namespace Packages were introduced. These allow for the creation of a package without any __init__.py file. Of course, it can still be present if package initialization is needed. But it is no longer required.

+ You have already seen that when import * is used for a module, all objects from the module are imported into the local symbol table, except those whose names begin with an underscore, as always
+ 

`from package import *`

+ 默认情况下，什么也不做
+ 因为这个是package，所以一般来说存在`__init__.py`
+ 在上述py文件中寻找一个`__all__`的list，并且把list中的每一个module都import。如果没有这个list，那么就什么也不做。
+ In `__init__.py`, there exists `__all__ = ['mod1','mod2']`
+ `from pkg import *`
+ 然后 我们可以直接调用`mod1` `mod2`...

`from module import *`
+ 默认情况下，加载everything，除了那些以单下划线开头的
+ 如果该文件开头有一个`__all__`的list，那么上面的语句就会import 这个列表中的每一项。

#### Subpackages
+ pkg
    + sub_pkg1
        + mod1.py
        + mod2.py  
    + sub_pkg2
        + mod3.py
        + mod4.py  

```
>>> import pkg.sub_pkg1.mod1
>>> pkg.sub_pkg1.mod1.foo()
[mod1] foo()

>>> from pkg.sub_pkg1 import mod2
>>> mod2.bar()
[mod2] bar()

>>> from pkg.sub_pkg2.mod3 import baz
>>> baz()
[mod3] baz()

>>> from pkg.sub_pkg2.mod4 import qux as grault
>>> grault()
[mod4] qux()
```

In addition, a module in one **subpackage** can reference objects in a **sibling subpackage**

+ in `pkg/sub__pkg2/mod3.py`
    + `from pkg.sub_pkg1.mod1 import foo`
    + `foo()`
    + `from pkg.sub_pkg2 import mod3`
    + `mod3.foo()`
    + `from .. import sub_pkg1`
    + `print(sub_pkg1)`
    + `from ..sub_pkg1.mod1 import foo`
    + `foo()` 
 

+ `import pkg.sub_pkg1.mod1` and `pkg.sub_pkg1.mod1.foo()`
+ `from pkg.sub_pkg1 import mod2` and `mod2.bar()`
+ 



### [Counter](https://stackoverflow.com/questions/19356055/summing-the-contents-of-two-collections-counter-objects)

```
>>> from collections import Counter
>>> a = Counter({'menu': 20, 'good': 15, 'happy': 10, 'bar': 5})
>>> b = Counter({'menu': 1, 'good': 1, 'bar': 3})
>>> a + b
Counter({'menu': 21, 'good': 16, 'happy': 10, 'bar': 8})
```

### [Enumerate()](https://www.geeksforgeeks.org/enumerate-in-python/)

```
enumerate(iterable, start=0)

Parameters:
Iterable: any object that supports iteration
Start: the index value from which the counter is 
              to be started, by default it is 0
```
```
l1 = ["eat","sleep","repeat"] 
  
# printing the tuples in object directly 
for ele in enumerate(l1): 
    print ele 
print 
# changing index and printing separately 
for count,ele in enumerate(l1,100): 
    print count,ele 
    
(0, 'eat')
(1, 'sleep')
(2, 'repeat')

100 eat
101 sleep
102 repeat 
```

### [二维列表转一维列表](https://www.geeksforgeeks.org/python-ways-to-flatten-a-2d-list/)

使用chain.iterable()

```
from itertools import chain 
  
ini_list = [[1, 2, 3], 
            [3, 6, 7], 
            [7, 5, 4]] 
              
flatten_list = list(chain.from_iterable(ini_list)) 
```

使用list comprehension

```
flatten_list = [j for sub in ini_list for j in sub] 
```

使用functools.reduce

```
from functools import reduce
flatten_list = reduce(lambda z, y :z + y, ini_list) 
```

使用sum

```
# converting 2d list into 1d 
flatten_list = sum(ini_list, []) 
```

### sort a list

```
tup = [('rishav', 10), ('akash', 5), ('ram', 20), ('gaurav', 15)]
tup.sort(key = lambda x: x[1]) 
sorted(tup, key = lambda x: x[1])

```



### list representation

1. [i for i in range(k) if condition]：此时if起条件判断作用，满足条件的，将被返回成为最终生成的列表的一员。

2. [i if condition else exp for exp]：此时if...else被用来赋值，满足条件的i以及else被用来生成最终的列表。


## linux Mac 命令
+ `grep -r VideoPartial .` # 循环寻找一个pattern
+ `find . -name "foo*"` # find files with names recursively
+ `echo $PATH`
+ `which echo` # show the absolute path of this command

### [grep 命令](https://www.cyberciti.biz/faq/howto-use-grep-command-in-linux-unix/)

```
grep 'word' filename
fgrep 'word-to-search' file.txt
grep 'word' file1 file2 file3
grep 'string1 string2'  filename
cat otherfile | grep 'something'
command | grep 'something'
command option1 | grep 'data'
grep --color 'data' fileName
grep [-options] pattern filename
fgrep [-options] words file
```


## Vim

### pattern related
+ `:%s/pattern//gn` # count number of matches of a pattern
+ `:10,50s/pattern//gn` # count number of matches of a pattern
+ `:%s/pattern//n` # count number of lines matching a pattern


## DNS 相关

Some useful dns related cmds3

### windows

```
ping www.google.com
ipconfig /displaydns
ipconfig /flushdns # clear out cache
nslookup www.google.com
nslookup # show local dns
```

+ `netstat -ano` 
  + `a` shows all connections, 
  + `n` shows ip addresses in stead of host names, 
  + `o` shows the process id that owns the connection


### Mac
+ `dig` querying DNS name servers for information about host addresses, mail exchanges, name servers, and related information.
+ `netstat -nr | grep default` find ip address
+ `traceroute 8.8.8.8` trace ip包

## Docker

+ `docker system prune -a` 删除所有用不到的image之类。我之前用这条命令，空出了将近90GB的空间
+ `du -sh ~/Library/Containers/com.docker.*` 显示该目录大小
+ `docker image ls`
+ `docker info`


## [图片转文字网站](https://www.onlineocr.net/zh_hans/)

## 虚拟机 VMWare
+ [为什么在VMWare的NAT模式下无法使用traceroute](https://blog.csdn.net/dog250/article/details/52194975)
+ [深入浅出VMware的组网模式](https://blog.csdn.net/dog250/article/details/7363534)



## Network
+ [Firewall](https://www.youtube.com/watch?v=kDEX1HXybrU&list=PL7zRJGi6nMRzHkyXpGZJg3KfRSCrF15Jg&index=1)运行在网络层，本质上就是根据各种rule来过滤ip包。
+ [WiFi (Wireless) Password Security - WEP, WPA, WPA2, WPA3, WPS Explained](https://www.youtube.com/watch?v=WZaIfyvERcA&list=PL7zRJGi6nMRzHkyXpGZJg3KfRSCrF15Jg&index=2)
    + In some router, we can set up **access control** to allow or forbid certain **mac address**. These devices can still get ip addresses from your router, but they cannot access the internet, or other local devices.
+ [PING Command - Troubleshooting](https://www.youtube.com/watch?v=IIicPE38O-s)
    + `ping yahoo.com` if it works, we know that the cable, modem, router, ISP all work well
    + `ping 127.0.0.1` or `ping localhost` to test network card.  
    + `ping dns` to test DNS issues. Google's `8.8.8.8`. If fails, try `ipconfig /flushdns` to flush the DNS Resolver Cache
    + `ping /?` to see more options
+ [PING and TRACERT (traceroute) networking commands](https://www.youtube.com/watch?v=vJV-GBZ6PeM)
    + `Request timed out`, either the server is down, or the server is blocking our ping requests
    + `Destination host unreachable`, it's from the router, saying the routing to the dest cannot be found.
    + `tracert 8.8.8.8` on windows
    +  or `traceroute -n 8.8.8.8` on mac

```
(base) MacBook-Pro:springer weiyuan$ traceroute 8.8.8.8
traceroute to 8.8.8.8 (8.8.8.8), 64 hops max, 52 byte packets
 1  hitronhub.home (192.168.0.1)  4.064 ms  5.199 ms  4.554 ms
 2  24.80.64.1 (24.80.64.1)  16.275 ms  19.309 ms  14.979 ms
 3  rc1bb-be113-1.vc.shawcable.net (64.59.152.121)  18.527 ms  19.374 ms  15.337 ms
 4  rc4sj-pos0-8-5-0.cl.shawcable.net (66.163.76.66)  26.314 ms  27.738 ms  28.289 ms
 5  72.14.221.102 (72.14.221.102)  26.456 ms  21.070 ms  17.461 ms
 6  108.170.245.113 (108.170.245.113)  17.834 ms
    74.125.243.177 (74.125.243.177)  20.171 ms  18.975 ms
 7  209.85.254.237 (209.85.254.237)  23.884 ms
    209.85.254.171 (209.85.254.171)  31.020 ms
    108.170.233.159 (108.170.233.159)  18.787 ms
 8  dns.google (8.8.8.8)  21.811 ms  22.039 ms  19.423 ms
```

+ [Port Forwarding Explained](https://www.youtube.com/watch?v=2G1ueMDgwxw)
    + Allows computers over the internet to connect to a specific computer or service within a private network. 
    + Even if my computer is behind a router and in a private network, other computers on the internet can access my computer.
    + For example, B can connect to A using RDC - Remote Desttop Connection
    + B will try to send a request of `66.94.34.13:3389` to my router
    + My router needs to know where to forward the request for port 3389
    + Without any port forwarding configured, your router does not know hwat to do with this request
    + **Port Forwarding** we're going to tell our router to send or foward any requests that tcome in with port `3389` and send the request to our computer.
        + on our own computer, log into the routers configuration page by typing the router's internal ip address using a web browser
        + `ipconfig` to check `Default Gateway`, which is your router's internal ip address.
        + a port number is always associated with an ip address
        + when the router receives a request, the `ip address` part in the request is done. Then they router use `port` as kay to check a table. For this specific port number, it will be mapped to a `local_ip_address:local_port`. Then the router will forward it.
+ [SSL, TLS, HTTP, HTTPS Explained](https://www.youtube.com/watch?v=hExRDVZHhig)
    + In HTTP, all information is sent in clear text.
    + In HTTPS, all data is encrypted
+ **SSL secure sockets layer**
    + Protocol that's used to ensure security on the internet
    + Uses public key encryption to secure data.
    + When a computer connects to a server, it will ask the server to identify itself.
    + Server will send a SSL certificate, which is used to authenticate the identity of a website. 其实就是告诉别的电脑说，我是真实可信的。
    + The client browser will check this certificate to verify identity.  If OK, it will send a msg to the server
    + The server responds with ack, then an SSL session can be estiablished.
    + Afterwards, encrypted data will be transferred in between.
+ **TLS - transport layer security**
    + The latest industry standard cryptographi protocol
    + successor to ssl
+ [POP3 vs IMAP](https://www.youtube.com/watch?v=SBaARws0hy4)
    + Used for retrieving email form an email server
    + Which protocol to use?
    + **POP3** is simpler. 
        + The only thing it does is download the email to your device from a mail server.
        + Only downloads what's in your inbox folder. 不会去下载别的folder，比如draft send deleted 等等
        + 没有任何的同步
        + 默认 pc1 上下载email，在下载后，从mail server上删除，pc2上永远看不到这个邮件了
    + **IMAP**
        + Allows you to view your email, that's on the server, form multiple devices.
        + Caches local copies of the email onto all of your devices
        + Synchronize folers and anything in them
        + If we delete a mail on pc-1, then the mail server will delete the email, then pc-2 will sync and delete it as well.
    + Set up in `incoming mail server`, like `pop.gmail.com` or `imap.gmail.com` 
    + POP3的邮件都在本地的，所以下载之后可以本地访问，没有网络连接也可以。但是IMAP的话，没有网就看不了，因为IMAP only caches local copies of the email of your device instead of downloading them
+ [What is SMTP - Simple Mail Transfer Protocol](https://www.youtube.com/watch?v=PJo5yOtu7o8) 
    + Used for sending email
    + A set of commands that authenticates and directs the transfer of email
    + Send an email for local device to SMTP server. `smtp.gmail.com`. Then the server will use smtp to send mail to the recipient's email server. Recipient can download the email form the mail server using `pop3 or imap`, or just view emails on the server using browser.
    +  Use TCP, which guarantee email delivery
+ [VLAN Explained](https://www.youtube.com/watch?v=jC6MJTh9fRE)
    + In a vlan, the computers, servers, and other network devices are logically connected, regardless of their physical location. 即使这些设备物理上分散在不同的地方，没关系。
    + Improved security
    + traffic management
    + make a network simpler
    + 注意，如果没有vlan，那么几个部门的设备在一个lan or switch上的话，这就是一个lan，所有在这个lan上的广播，所有机器都知道。这样不同部门的机器就乱了。
    + separate the network broadcast traffic between these departments
    + On a switch, for each port, assign a vlan id.
    + all broadcast are limited in each vlan, the overall traffic is less
+ [What is a Proxy Server?](https://www.youtube.com/watch?v=5cPIukqXe5w)
    + Retrieves data on the internet on behalf of a user 换句话说，本来我在浏览器里面直接访问A。现在我可以选一个代理服务器B，我先告诉B，我要访问A。然后B就会直接去访问A，然后把结果告诉我。
    + Privacy - allows you to surf the internet anonymously. If I access A directly, my public ip address is visible. With a proxy, your public ip address is not visible; only the proxy server's ip address is. 
    + Speed - proxy server caches web page database on proxy server. 
    + it also saves band with
    + Activity logging
    + A proxy server cannot encrypt data
    + **VPN - virtual private network**
        + A VPN encrypts the data that's being transferred over the internet. 
        + No data or activities can be logged
        + Provides a dedicated secure tunnel between 2 points over the internet.
+ [What is a DMZ? (Demilitarized Zone)](https://www.youtube.com/watch?v=dqlzQXo1wqo)
    + Used to improve the security of an organization's network
    + By segregating devices, such as computers and servers, on the opposite sides of a firewall
    + Suppose a company has the following devices
        + A web server (open to public)
        + A email server (open to public
        + several work stations
        + internal servers
        + A firewall hiding all prevous devices
    + The servers are behid the company's firewall and are inside the company's private network
    + The company is letting in people from an **untrusted** netwok (the internet) and are given access behind the company's firewall. Hackers could use this as an opening to cause havoc on the company's network. That's because they have passed the firewall, and then can attach the web server and email server. At the same time, they can also attach other internal databases or work stations.
    +  简单而言，如果把web server email server都放到firewall的左边，那么这两个就完全暴露在internet上，而firewall用来保护内部网络，如果觉得不安全，还可以用两层防火墙，通过第一层防火墙访问server，通过第二层防火墙来访问内部网络。
    +  从字面上来理解，**Demilitarized**就是说没有防火墙作为保护。
    +  在家庭路由器上，通常可以把dmz设置成为game console的ip地址。这样，router本身就作为一个防火墙，所有进来的traffic全部导向game console（因为打游戏要实时性，所有的traffic都看过才安心），然后只有安全的traffic才会进入内部网络。
+ [NAT Explained - Network Address Translation](https://www.youtube.com/watch?v=FTUV0t6JaDA)
    + NAT is used in routers
    + NAT translates a set of IP addresses to another set of IP addresses
    + NAT helps preserve the limited amount of IPv4 public IP addr
    + Must have a public ip addr to visit internet
    + private ip addr are only used internally
    + **IPv6** won't need NAT
+ [ARP Explained - Address Resolution Protocol](https://www.youtube.com/watch?v=cn8Zxh9bPio)    
+ Used to resolve ip addr to mac addr
+ mac addr is the physical addr of the device, globally unique number
+ Devices need the MAC address for communication on a local area network
+ Devices uses **ARP** to acquire the MAC addr for a device
+ An IP addr is used to locate a device on a network
+ A MAC addr is what identifies the actual device
+ Procedure
    + A device will first look at its internal list, called **ARP cache**, `arp -a`
    +  If not found, it will send a broadcast, who is 10.0.0.4. The dest machine will send a msg back with this ip addr and mac addr. Then they can communicate.
    +  ARP cache stores ip addr to mac addr mapping.
    +  dynamic vs static: `apr -s ip_addr mac_addr`
## Router Security
+ [Router Security](https://www.routersecurity.org/index.php)
+ [A Defensive Computing Checklist](https://defensivecomputingchecklist.com/)
## OCR
+ [免费在线OCR文字识别](https://www.onlineocr.net/zh_hans/)
+ 