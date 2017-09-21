# 介绍

&emsp;&emsp;前面介绍了ngrok，也说了它的1.x版本具有一些Bug并且作者放弃了维护，那么，谁能够取代ngrok在内网穿透工具中的地位呢？我觉得frp也许可以，frp是fast reverse proxy的缩写，其可用于反向代理和内网穿透，支持tcp, udp, http, https 协议，这比ngrok还多了个udp。并且发布以来广受欢迎，积累了不少用户，作者也是很勤快，一直在修复bug和更新新特性。  

&emsp;&emsp;并且，作者是个中国人，所以在[GitHub](https://github.com/fatedier/frp)上还有中文文档，写的还挺详细的，不过……不去看文档是很多人的特点，所以我这边还是记点东西来介绍下简单的安装和操作配置。

# 安装配置
&emsp;&emsp;frp提供了热门平台的程序文件，所以安装也变得比较简单了，我这边的是Linux 64位平台的，其它的请自己去下——>[传送门]()
```
wget https://github.com/fatedier/frp/releases/download/v0.12.0/frp_0.12.0_linux_amd64.tar.gz
tar xzf frp_0.12.0_linux_amd64.tar.gz
mv frp_*/frps /usr/bin/
mkdir /etc/frp/
rm -rf frp_*
```

&emsp;&emsp;然后根据下面配置你自己改了填到配置文件中
```
vi /etc/frp/frps.ini
//下面是我翻译的带说明的服务端配置文件，就这么凑和着看吧
[common]
#frp服务器监听地址，如果是IPV6地址必须用中括号包围
bind_addr = 0.0.0.0
#frp服务器监听端口
bind_port = 7000
 
#kcp的udp监听端口，如果不设那就不启用
#kcp_bind_port = 7000
#指定使用的协议，默认tcp，可选kcp
#protocol = kcp
 
#如果要使用vitual host，就必须设置
#vhost_http_port = 80
#vhost_https_port = 443
 
#Web后台监听端口
dashboard_port = 7500
 
#Web后台的用户名和密码
dashboard_user = admin
dashboard_pwd = admin
 
#Web后台的静态资源目录，调试用的，一般不设
#assets_dir = ./static
 
#日志输出，可以设置为具体的日志文件或者console
log_file = /var/log/frps.log
 
#日志记录等级，有trace, debug, info, warn, error
log_level = info
#日志保留时间
log_max_days = 3
 
#启用特权模式，从v0.10.0版本开始默认启用特权模式，且目前只能使用特权模式
#privilege_mode = true
 
#特权模式Token，请尽量长点且复杂
privilege_token = 12345678
 
#特权模式允许分配的端口范围
privilege_allow_ports = 2000-3000,3001,3003,4000-50000
 
#心跳超时，不用改
#heartbeat_timeout = 90
 
#每个代理可以设置的连接池上限
#max_pool_count = 5
 
#认证超时时间，一般不用改
#authentication_timeout = 900
 
#如果配置了这个，当你的模式为http或https时，就能设置子域名subdomain
#subdomain_host = frps.com
 
#是否启用tcp多路复用，默认就是true，不用管
#tcp_mux = true
```

&emsp;&emsp;在这个配置文件中老版本是可以不启用特权模式或者同时添加其它的section来在服务端配置其它的转发设置的，但是从v0.10.0版本开始后特权模式暂时是唯一可用的（因为方便，不用为了一个配置而既要改服务端又要改客户端）。  
在修改好服务端配置文件后，我们可以启用frp的服务端了
```
frps -c /etc/frp/frps.ini
```

&emsp;&emsp;然后服务端就OK了，下面开始配置客户端，客户端程序自己在GitHub上面下，各个平台的包里面frps是服务器，frpc就是客户端。  
配置文件如下，自己修改后保存为frpc.ini
```
[common]
#frp服务器地址
server_addr = 1.2.3.4
#frp服务器端口
server_port = 7000
#特权模式Token
privilege_token = 12345678
#转发SSH
[ssh]
type = tcp
#可以指定为其它IP，默认是本地
#local_ip = 127.0.0.1
local_port = 22
remote_port = 6000
#启用加密
use_encryption = true
#启用压缩
use_compression = true
 
#转发Web
[web]
type = http
local_port = 80
custom_domains = www.yourdomain.com
#修改header中的host
#host_header_rewrite = dev.yourdomain.com
#启用简单HTTP认证
#http_user = abc
#http_pwd = abc
#在服务端配置了subdomain_host的情况下用于自定义二级域名
#subdomain = test
#在存在多个相同域名的情况下通过请求的URL路由到不同的配置
#locations = /news,/about
 
#转发DNS请求
[dns]
type = udp
local_ip = 8.8.8.8
local_port = 53
remote_port = 6000
 
#转发Unix域套接字（这儿是Docker）
[unix_domain_socket]
type = tcp
remote_port = 6000
plugin = unix_domain_socket
plugin_unix_path = /var/run/docker.sock
 
#HTTP代理
[http_proxy]
type = tcp
remote_port = 6000
plugin = http_proxy
#配置http代理的简单认证
#plugin_http_user = abc
#plugin_http_passwd = abc
```

&emsp;&emsp;以上是大部分的参考，如果你有啥需要的参照着改就行了，在Linux等平台下，执行如下命令就行
```
frpc -c /path/to/frpc.ini
```

&emsp;&emsp;而在Windows下，需要你创建一个frpc.exe的快捷方式，然后将参数加到快捷方式属性的目标那一栏的frpc.exe后面，也可以自己百度下如果将命令注册为系统服务来实现常驻。  

&emsp;&emsp;成功运行后，访问服务器的7500端口会有一个WebGUI，如下，显示当前的状态，左侧的Proxies里会显示客户端注册的各种转发规则。  
&emsp;&emsp;至于客户端这儿有日志提示是否连上服务器，是否成功启用配置啥的。总的来说还是挺好用的，比较期待以后还有啥新的功能_(:з」∠)_

# 使用教程