# debian等linux加载远程samba
```
使用
# mount -t cifs -o username="username",password="password" //ip/sharedirectory /mnt/mountdirectory
```

# centos
## CentOS 7
### 安装tunctl

1.  Create the repository config file /etc/yum.repos.d/nux-misc.repo
```
[nux-misc]  
name=Nux Misc  
baseurl=http://li.nux.ro/download/nux/misc/el7/x86_64/  
enabled=0  
gpgcheck=1  
gpgkey=http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro  
```
2. Install tunctl rpm package
```
# yum --enablerepo=nux-misc install tunctl  
```
## CentOS 6

# Debian
## 安装依赖包ncurses zlib
```
$ sudo apt-get install libncurses5-dev
$sudo apt-get install zlib1g zlib1g-dev
```
    
## Debian中网卡的设置以及开机自动启用网卡
&emsp;&emsp;在Debian中网卡的设置可以通过/etc/network/interfaces文件来进行，具体可分为三种不同的配置方式：DHCP自动获取、静态分配IP地址和PPPoE宽带拨号。   

&emsp;&emsp;具体设置如下： 在进行配置之前，首先进入/etc/network目录中，用nano命令编辑interfaces文件：
  
### 网卡通过DHCP自动获取IP地址

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

#
# The loopback network interface（配置环回口）

# 开机自动激lo接口
auto lo
# 配置lo接口为环回口
iface lo inet loopback

#
# The primary network interface （配置主网络接口）

#开机自动激活eth0接口
auto eth0
#配置eth0接口为DHCP自动获取
iface eth0 inet dhcp

网卡静态分配IP地址

# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

#
# The loopback network interface（配置环回口）

# 开机自动激lo接口
auto lo
# 配置lo接口为环回口
iface lo inet loopback

#
# The primary network interface （配置主网络接口）

#开机自动激活eth0接口
auto eth0
#配置eth0接口为静态设置IP地址
iface eth0 inet static
address 10.16.3.99
netmask 255.255.255.0
network 10.16.3.0
broadcast 10.16.3.255
gateway 10.16.3.1

#单网卡配置多个ip，设置第二个ip地址

auto eth0:1

iface eth0:1 inet static
address x.x.x.x
netmask 255.255.255.0
network 10.16.3.0
broadcast 10.16.3.255
gateway 10.16.3.1

# dns-* options are implemented by the resolvconf package, if installed（DNS设置）
dns-nameservers 61.153.177.196 61.153.177.197
dns-search feelnet.org
```

配置好后推出编辑/etc/resolv.conf，配置DNS，加入以下信息：

```
nameserver 219.146.0.130
```

### 基本文件格式  
&emsp;&emsp;Debian的网络配置文件在目录/etc/network中，其中的interfaces文件中保存了每一个网络设备在启动时的属性。下面是一个很简单的配置文件：

例子 1. 简单的配置文件

```
auto lo eth0 ①
iface lo inet loopback ②
iface eth0 inet dhcp ③
iface eth1 inet static ④
address 10.1.133.165
netmask 255.255.255.0
gateway 10.1.133.1
① 表示系统中的lo和eth0两个网络设备在系统启动网络时自动启动。
② 表示网络设备lo使用TCP/IP网络并且是一个loopback设备，如果是IPV6网络则使用"inet6"，IPX网络使用"ipx"。
③ 表示网络设备eth0使用TCP/IP网络，同时使用DHCP自动获取IP地址。
④ 表示网络设备eth1使用TCP/IP网络，并且是占用固定的IP
10.1.33.165，子网掩码是255.255.255.0，网关是10.1.133.1。
```

&emsp;&emsp;上面的这种配置方式，可以使用于大多数的情况，但在一些特殊的情况下，就需要一些更为灵活的手段来配置网络。

### 通过PING配置网络

&emsp;&emsp;Linux在处理PCMCIA卡的时候有比较好的方式，可以在PCMICA卡插入时通过一个配置脚本来确定网络地址。但是，笔记本上的网卡是笔记本自带的，并非PCMCIA卡，由于经常需要奔波于办公室、实验室和家之间，就经常需要修改网络地址。如果我去的每一个地方都安装了DHCP，那么我就可以把 eth0设定成为DHCP的方式，然而我的情况却是：在家可以使用DHCP，在办公室和实验室都要使用固定地址。

&emsp;&emsp;为了解决这个问题，我们可以使用一种mapping机制，这种方法的基本原理是通过运行一个程序来确定目前所处的环境，并为这个环境选择一套配置。我现在使用的就是通过ping一个网络的网关来确定当前网卡究竟连接在哪个网络上，然后再选择这个网络的配置。

&emsp;&emsp;首先，在/usr/share/doc/ifupdown/examples中有一个文件ping-places.sh，把它复制到/etc/network目录中，然后chmod
+x /etc/network/ping-places.sh。下面就是编辑/etc/network/interfaces文件，下面是一个例子：

例子 2.


```
mapping eth0 ①
script /etc/network/ping-places.sh
map 192.168.0.107/24 192.168.0.1 home
map 10.1.133.165/24 10.1.133.1 office
map 10.1.0.107/24 10.1.0.1 lab
iface home inet dhcp ②
iface office inet static ③
address 10.1.133.165
netmask 255.255.255.0
gateway 10.1.133.1
up cp /etc/resolv.conf.school /etc/resolv.conf ④
iface lab inet static
address 10.1.0.107
netmask 255.255.255.0
gateway 10.1.0.1
up cp /etc/resolv.conf.school /etc/resolv.conf
① 表示对于网络设备调用脚本/etc/network/ping-places.sh，如果能够用地址192.168.0.107/24
ping通地址192.168.0.1，则将eth0映射为设备home，即启动home的配置。后面的office和lab与其类似。
② 表示虚拟设备home使用DHCP分配的地址。
③ 表示虚拟设备office使用固定地址。
④ 表示在启动这个网络设备后还要执行cp命令，从而指定一个域名解析方法。除了up以外，还有pre-up、down和post-down可以用来指定在启动或停止网络设备前后执行的命令。
```

&emsp;&emsp;在/usr/share/doc/ifupdown/examples中有一些配置网络的例子和需要的脚本。
## 安裝.deb軟件方法
```
$ sudo dpkg -i *.deb
```

## Debian安装docker
### 卸载旧版本
```
$ sudo apt-get remove docker docker-engine docker.io
```
### 检查内核版本
```
$ uname -r
//大于3.10即可，否则升级内核
```
### 安装docker社区版docker ce
#### 使用存储库安装
##### 更新apt包索引
```
$ sudo apt-get update
```
##### 安装必须软件包
```
//jessie或者stretch版本debian安装下列依赖包
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg2 \
    software-properties-common
wheezy安装下列依赖包
$ sudo apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     python-software-properties
```
##### 添加docker官方的GPG密钥
```
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```
##### 检测密钥ID 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88
```
$ sudo apt-key fingerprint 0EBFCD88

pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```
##### 使用下列命令安装稳定的仓库
```
//amd64
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
```
#### 安装docker-ce
##### 更新apt索引
```
$ sudo apt-get update
```
##### 安装最新版docker-ce
```
$ sudo apt-get install docker-ce

//也可以指定安装的docker版本
//使用apt-cache madison命令
$ apt-cache madison docker-ce

docker-ce | 17.06.0~ce-0~debian-jessie | https://download.docker.com/linux/debian jessie/stable amd64 Packages

$ sudo apt-get install docker-ce=<VERSION_STRING>
```

## Debian 9

### 修改网卡名字为默认ethx

```
# nano /etc/default/grub
```


查找:GRUB_CMDLINE_LINUX=""
修改为:GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"

重新生成grub引导配置文件
```
# grub-mkconfig -o /boot/grub/grub.cfg
```


最后重启即可恢复默认网卡为eth0，操作结束。

### 安装tunctl
```
apt-get install uml-utilities bridge-utils
```

## Debian配置zsh为默认终端方法
```
$ sudo apt update && apt install zsh -y
$ sudo chsh -s /bin/zsh
//修改/etc/passwd里面默认终端的配置
$ sudo vim /etc/passwd
//打开后在vim下输入如下命令按回车键后全部修改bash为zsh
:%s/bash/zsh/g
//如果/etc/passwd文件不存在，使用如下命令生成默认用户配置文件
$ pkpasswd -l > /etc/passwd
//修改后重启linux
```

## Debian下ls后终端颜色的变化设置
root用户打开文件~/.bashrc或者/etc/bash.bashrc或者/etc/profile。找到如下几行：  
```
//主要修改bashrc或者profile文件
//如果~/.bashrc /etc/bash.bashrc下没有下面的文字，则可以加上去，或者在/etc/profile中加上去

# You may uncomment the following lines if you want `ls' to be colorized:
# export LS_OPTIONS='--color=always'
# eval "`dircolors`"
# alias ls='ls $LS_OPTIONS'
# alias ll='ls $LS_OPTIONS -l'
# alias l='ls $LS_OPTIONS -lA'
```
按照第一行所说，将下面五行的注释去掉，则ls命令将显示颜色。但颜色可能很古怪，也许很正常，总之，不是我们自己根据自己对颜色的喜好定置的。  
要定制自己喜欢的颜色，可如下操作：  
```
$ dircolors -p >~/.colourrc
```
这将在root用户根目录下生成.colourrc文件，这个文件的注释极其详细，按照注释，修改颜色参数可定制颜色。然后，将上面红色的一行改变为：  
```
eval "`dircolors .colourrc`"
```
运行source ~/.colourrc命令。  
好了，ls可按照定制的颜色显示了。  

# Ubuntu 
## Ubuntu 16.04
### 安装tunctl
```
apt-get install uml-utilities bridge-utils
```

# red hat
