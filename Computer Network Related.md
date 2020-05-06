# Computer Network Related

### 网络知识要点
+ 理论上，simple switch operates at layer-2, 所以switch本身不需要mac地址。client在lan中发送dataframe的时候，目标mac用的是router的mac地址。
+ 两台机器要直接通信，需要他们在同一个ip地址的网段下，而且在一个LAN里面。两条中有一条不满足，就不能直接通信，而必须使用roter
+ 目前来看，一个路由器至少有两个IP地址，一个针对lan，一个针对wan
+ 广播ip地址，即host的所有位都为1
+ 网络ip地址，即host的所有位都为0
+ 在windows中，default gateway就是local router ip address. It's also called the Gateway of last resort.
+ 255.255.255.255 是默认的广播ip地址，会在本地的lan中广播。但是local router不会向其他router转发此广播
+ 常见的私有ip地址,, not allowed on the Internet
    + 10.0.0.0 / 8
    + 172.16.0.0 / 12
    + 192.168.0.0 / 16
+ 当本地机器172.16.0.1发送一个ip包给1.1.1.1时候
    + local router接受`source: 172.16.0.1, dest:1.1.1.1`
    + local router发送`source: 203.4.9.4, dest:1.1.1.1`
    + 这就叫NAT
+ In Data link layer, it seems that a router has at least 2 different mac addresses. One for receiving a dataframe from a local client, which appears in the `dest` field. The other for sending the dataframe to other connected router, which appears in the `source` field.
+ TCP/UDP 要点
    + client side，keep `local ip, local port, protocol`
    + server side, keep `local ip, remote ip, local port, remote port, protocol`
    + On Windows, `netstat -ano` shows all connections
    + On Mac, just use `netstat` 
  
#### Cisco router settings

    + On windows, suppose we use `COM3` to connect to router, using putty
    + `enable` to enter previlige mode
    + `show version` or `show clock`
    + `configure terminal` to enter config mode
        + `hostname Internet-Router` change router name
  	  + `exit`
    + `show ip interface brief`  
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_041.png)
    + `conf t` to enter configuration mode
        + `interface gigabit 0/1`
        + `description Corporate Network`
        + `ip address 192.168.0.1 255.255.255.0`
        + `shutdown`
        + `no shutdown` 
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_047.png)    
    + `show ip interface brief` 每个网络接口有单独ip地址
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_048.png)    
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_049.png)
    + `show interface description`
    +  `conf t`
        + `interface loopback 0`
        + `ip add`
        + `ip address 172.16.0.1 255.255.255.255`
        + `^Z`
    + `show ip interface brief`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_054.png) 
    +  `conf t`
        + `enable secret pass123`
        + `username admin privilege 15 secret pass123`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_055.png)
        + `line vty 0 4`
        + `transport input ssh`
        + `login local`
        + `exit`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_056.png)        
    +  `ip domain-name networkdirection.net`
    +  `crypto key generate rsa` follow by `2048	`
    +  `ip ssh version 2`
    +  `banner login #`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_057.png)    
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_058.png)    
    +  Connect `192.168.0.1:22` via `putty`
    +  `username: admin`
    +  `show running-config`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_064.png)            
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_065.png)    
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_066.png)    
    + `dir`
    + `copy running-config startup-config`
    + `copy run start`
    + `write mem`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_068.png)    
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/10/image_071.png)                       

#### Ethernet要点
+ Ethernet uses MAC address to identify each computer in the network.
+ One Mac address per network interface
+ A router usually has more then one network interface, therefore more MAC addresses
+ MAC 有48位
    + First 24 bits: OUI or Organization Unique Identifier
    + Second 24 bits： Vendor assigned
    + Broadcast： ffff.ffff.ffff
    + Routers do not forward broadcasts 
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_016.png)   
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_020.png)

##### Hubs

+ 主要特点
    + All devices in a collision Domain
    + Half Duplex, canot send and receive the same time
    + Data replicated to all ports
    + Limited growth
     

![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_023.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_026.png)
+ 一个client发送，hub上其他所有port上的机器都收到
+ device cannot send and receive at the same time
+ More devices = Larger Collision Domain
    + Carrier Sense Multiple Access (CSMA)
    + Collision Avoidance (CSMA/CA)
    + Collision Detection (CSMA/CD)

#### Bridges

+ 主要功能
    + Flooding
    + Learning
    + Forwarding
    + Filtering
    + Aging
     
+ 主要特点 
    + 把mac地址分块，和每个port绑定
    + Less traffic flooding
    + Smaller collision domains
    + Better peformance
    + Better network growth (scalability)
     
 ![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_037.png)
 ![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_042.png)
 ![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_048.png)
 ![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_051.png)
 + 从上图看出，虽然bridge有三个port，但是在学习完成以后，每次转发dataframe只会选择一个port来输出。避免了flooding

#### Switches

+ 主要功能
    + In Star ToPology, every single port behaves like a bridge port. This is no longer a bus topology. So no flooding as often as hubs.
    + Each port and its connected device forms a collision domain. 减少了冲突，而且full duplex
    + 
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_071.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_073.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_074.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_076.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_080.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_082.png)

#### Lab

+ Log on `switch-1`, then `show interfaces status`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_089.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_091.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_093.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/11/image_094.png)

  
    + 在switch-1的界面，我们其实看到了四个接口
          + `Gi0/0` for management
        + `Gi0/1` to server-1
        + `Gi0/2` to server-2
        + `Gi1/1` to switch-2
+ `show mac address-table`
+ `show mac address-table aging-time`
+ `show mac address-table`
+ `conf t`     
    + `mac address-table aging-time 900`
    + `^Z`
+ `show mac address-table`


#### How VLANs Work

![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_003.png)

+ 首先，定义什么是LAN: A layer 2 broadcast domain
+ `Dest:ffff.ffff.ffff`会发送到LAN中的各个device. Switch and bridges will forward this frame out, while routers will receive it but willnot forward it.
+ 根据上述定义，所有连接在一起的device, bridge, hub, switch，组成一个LAN，而router不在这个lan里面。
+ 即使在一个lan中，我们仍然希望分成几个子LAN，来负责保密。比如，所有server在一个子LAN中，其余设备在另一个子LAN。中间可以用防火墙相连。
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_006.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_009.png)

所以，接下来的重点是，如何把一个LAN分成几个小的子LAN。最简单的方案就是每一个子LAN中使用一个不同的switch，然后不同lan的switch可以直接相连，或者通过防火墙 or router相连。
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_012.png)

另一种更加灵活的方案就是使用VLAN
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_014.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_015.png)
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_018.png)

+ 每个VLAN有一个12bit的VLAN ID, RANGE = 1-4094. 注意，0和2095被保留了。
+ 假设switch上有8个port，每个port分配一个vlan id。 这样，broadcasts and flooding are limited to the VLAN.
+ VLAN is switch function, on layer 2. 
+ VLAN 与 ip协议配合使用。一般来说，one subnet per VLAN. 这是传统，但不是强求
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_023.png)

+ Cisco 有些不同
    + VLAN 1002 TO 1005 are reserved
    + Normal range: 1 to 1005
    + Extended range： 1006 to 4094 二者处理方式不同

下面的问题。不同VLAN之间如何通信呢？They provide layer 2 boundry, so that a frame from vlan 1 will not reach vlan 2. we can use layer 3 to help, i.e. ip protocol. Each vlan should be associated with one subnet. The router has interfaces connected to each vlan; and each interface has an ip address of the corresponding subnet. Within each subnet, each device will configure its gateway as the interface's ip address. 

当vlan1中的一个device需要发送frame to vlan2的时候，会把frame发送给router，连接的router interface的mac地址 作为dest。 The router is connected to both subnets, so it knows where to send this frame. It will rewrite the dest mac address field, set it as the real server's mac address, then send it. 

![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_031.png)

最大的问题，devce和router是如何把mad地址和ip地址联系在一起的?
简单的回答：**ARP = Address Resolution Protocol**
+ A device broadcats an ARP message, asking who owns a particular ip address. 
+ If the owner is in the local vlan, it will responds with its mac address. 

下面是实验部分

![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_033.png)

首先，登录switch，创建vlan
+ `conf t`
+ `vlan 10`
+ `name Workstations`
+ `exit`
+ `vlan 20`
+ `name Servers`
+ `exit`
+ `^Z`
+ `show vlan brief`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_034.png)

从上面的图片可知，两个vlan已经创建，接下来需要设置。

+ `conf t`
+ `interface gi 2/1`
+ `switchport access vlan 10` put port `gi 2/1` in vlan 10
+ `no shut`
+ `interface gi 2/2`
+ `switchport access vlan 10` put port `gi 2/2` in vlan 10
+ `no shut`
+ `interface gi 1/1`
+ `switchport access vlan 20` put port `gi 1/1` in vlan 20
+ `no shut`
+ `interface gi 1/2`
+ `switchport access vlan 20` put port `gi 1/2` in vlan 20
+ `no shut`
+ `^Z`
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_034.png)
我们看到，上面的四个port已经被设置到两个vlan中了。

接着，登录workstation-1, ping it
+ 本身的ip=192.168.10.1
+ `ping 192.168.10.2`  workstation-2成功，因为在同一个vlan 10中
+ `ping 192.168.20.1` server-1不成功，因为不在同一个vlan。此时，两个vlan互相无法访问。

为了join这两个vlan，我们需要使用router

在workstation-1中，运行ipconfig
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_040.png)
+ In `ens4`, we see its ip address is `192.168.10.1` 
+ 然后，运行`netstat -nr` 来查看default gateway
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_042.png)
+ 显然，default gateway is `192.168.10.254` 和之前的逻辑图里面一致，没问题。但是我们现在还不能访问router，因为还没有配置一个port连接到router。所以说光有ip地址没有用，因为还没有连线啊。

接下来，回到switch的界面，进行连线
+ `conf t`
    + `interface gi 0/1`
    + `switchport access vlan 10`
    + `no shut`
    + `interface gi 0/2`
    + `switchport access vlan 20`
    + `no shut`
    +  `^Z`
+  

然后，我们检查连线是否成功。我们先回到workstation-1， 然后
+ `ping 192.168.10.254` 这是router的IP地址，in vlan 10，成功。
+ `ping 192.168.20.2` 这是server-2的IP地址，in vlan 20, 成功。
+ 
接下来，我们要证明上面这条的确经过了router，而不是直接访问server-2

用`traceroute`，和ping相似，但是会追踪所有layer3的device on the path

+ `traceroute -n 192.168.20.2` 直接显示每一个ip地址
+ `traceroute 192.168.20.2` 使用dns尽量解析每一个IP地址的名字。
![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_047.png)









![](../course/Network\ Fundamentals\ by\ Network\ Direction/image/12/image_003.png)







### 问题。一个switch or router上有没有mac地址，有几个。AP呢? 一个AP有没有mac地址，有几个。


### [Network Interface](https://www.youtube.com/watch?v=PYTG7bvpvRI)

Each network interface is a combination of `ipaddress:port`. 

Here is the code: [main01.js](./code/main01.js)

Here's how to use it

+ Server: `node main 0.0.0.0`
+ Client: `curl 0.0.0.0:8181`

Note, the ip address here can be:

+ `0.0.0.0` accepts all requests
+ `192.168.0.20` uses this interface
+ `127.0.0.1` or `localhost`, can only visited from the localhost loop interface


### PieterExplainsTech from Youtube

+ [How Network Address Translation Works](https://www.youtube.com/watch?v=QBqPzHEDzvo) This is a very concise introduction
+ [Hub, Switch or Router? Network Devices Explained](https://www.youtube.com/watch?v=Ofjsh_E4HFY)

![](./image/hubswitchorrouter/image_001.png)
![](./image/hubswitchorrouter/image_002.png)
![](./image/hubswitchorrouter/image_003.png)
![](./image/hubswitchorrouter/image_004.png)
![](./image/hubswitchorrouter/image_005.png)
![](./image/hubswitchorrouter/image_006.png)
![](./image/hubswitchorrouter/image_007.png)
![](./image/hubswitchorrouter/image_008.png)
![](./image/hubswitchorrouter/image_009.png)
![](./image/hubswitchorrouter/image_010.png)
![](./image/hubswitchorrouter/image_011.png)

+ [Automatic IP Address Assignment: How DHCP Works](https://www.youtube.com/watch?v=RUZohsAxPxQ)

![](./image/dhcp/image_001.png)
![](./image/dhcp/image_002.png)
![](./image/dhcp/image_003.png)
![](./image/dhcp/image_004.png)
![](./image/dhcp/image_005.png)
![](./image/dhcp/image_006.png)
![](./image/dhcp/image_007.png)
![](./image/dhcp/image_008.png)
![](./image/dhcp/image_009.png)
![](./image/dhcp/image_010.png)
![](./image/dhcp/image_011.png)
![](./image/dhcp/image_012.png)
![](./image/dhcp/image_013.png)
![](./image/dhcp/image_014.png)

+ [Inside the Domain Name System
](https://www.youtube.com/watch?v=GlZC4Jwf3xQ)

![](./image/dns/image_001.png)
![](./image/dns/image_002.png)
![](./image/dns/image_003.png)
![](./image/dns/image_004.png)
![](./image/dns/image_005.png)
![](./image/dns/image_006.png)
![](./image/dns/image_007.png)
![](./image/dns/image_008.png)
![](./image/dns/image_009.png)
![](./image/dns/image_010.png)
![](./image/dns/image_011.png)
![](./image/dns/image_012.png)
![](./image/dns/image_013.png)
![](./image/dns/image_014.png)
![](./image/dns/image_015.png)
![](./image/dns/image_016.png)
![](./image/dns/image_017.png)
![](./image/dns/image_018.png)
![](./image/dns/image_019.png)
![](./image/dns/image_020.png)
![](./image/dns/image_021.png)
![](./image/dns/image_022.png)
![](./image/dns/image_023.png)
![](./image/dns/image_024.png)
![](./image/dns/image_025.png)
![](./image/dns/image_026.png)
![](./image/dns/image_027.png)
![](./image/dns/image_028.png)
![](./image/dns/image_029.png)
![](./image/dns/image_030.png)
![](./image/dns/image_031.png)
![](./image/dns/image_032.png)
![](./image/dns/image_033.png)
![](./image/dns/image_034.png)
![](./image/dns/image_035.png)
![](./image/dns/image_036.png)
![](./image/dns/image_037.png)
![](./image/dns/image_038.png)
![](./image/dns/image_039.png)
![](./image/dns/image_040.png)
![](./image/dns/image_041.png)
![](./image/dns/image_042.png)
![](./image/dns/image_043.png)
![](./image/dns/image_044.png)
![](./image/dns/image_045.png)
![](./image/dns/image_046.png)
![](./image/dns/image_047.png)

+ [UDP and TCP: Comparison of Transport Protocols](https://www.youtube.com/watch?v=Vdc8TCESIg8)

![](./image/tcpandudp/image_001.png)
![](./image/tcpandudp/image_002.png)
![](./image/tcpandudp/image_003.png)
![](./image/tcpandudp/image_004.png)
![](./image/tcpandudp/image_005.png)
![](./image/tcpandudp/image_006.png)
![](./image/tcpandudp/image_007.png)
![](./image/tcpandudp/image_008.png)
![](./image/tcpandudp/image_009.png)
![](./image/tcpandudp/image_010.png)
![](./image/tcpandudp/image_011.png)
![](./image/tcpandudp/image_012.png)
![](./image/tcpandudp/image_013.png)
![](./image/tcpandudp/image_014.png)
![](./image/tcpandudp/image_015.png)
![](./image/tcpandudp/image_016.png)
![](./image/tcpandudp/image_017.png)
![](./image/tcpandudp/image_018.png)
![](./image/tcpandudp/image_019.png)
![](./image/tcpandudp/image_020.png)
![](./image/tcpandudp/image_021.png)
![](./image/tcpandudp/image_022.png)
![](./image/tcpandudp/image_023.png)
![](./image/tcpandudp/image_024.png)
![](./image/tcpandudp/image_025.png)
![](./image/tcpandudp/image_026.png)
![](./image/tcpandudp/image_027.png)
![](./image/tcpandudp/image_028.png)
![](./image/tcpandudp/image_029.png)
![](./image/tcpandudp/image_030.png)
![](./image/tcpandudp/image_031.png)
![](./image/tcpandudp/image_032.png)
![](./image/tcpandudp/image_033.png)
![](./image/tcpandudp/image_034.png)
![](./image/tcpandudp/image_035.png)





