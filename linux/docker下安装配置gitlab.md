# [1. 安装docker ](#2) <span id="jump">
&emsp;&emsp;安装docker参考[docker安装文档](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/)。
# [2.安装gitlab](#2)
## [2.1 下载gitlab](#2.1)
```
$ docker pull gitlab/gitlab-ce:latest
```
## [2.2 启动gitlab](#2.2)
&emsp;&emsp;用下面的命令启动一个默认配置的Gitlab。如果我们只在本机测试使用的话，将hostname替换为localhost。如果需要让外部系统也能访问的话使用外网IP地址。--publish参数是将docker的端口映射到主机的端口，比如--publish 4443:443就是将容器的443端口映射到主机的4443端口去。  
```
$ sudo docker run --detach \
    --hostname gitlab.example.com \
    --publish 4434:443 --publish 880:80 --publish 222:22 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:/etc/gitlab \
    --volume /srv/gitlab/logs:/var/log/gitlab \
    --volume /srv/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce
```
下面是我使用的命令
```
sudo docker run --detach \
    --hostname localhost \
    --publish 4443:443 --publish 880:80 --publish 222:22 \
    --name gitlab \
    --restart always \
    --volume /mnt/share/win-share/gitlab/config/:/etc/gitlab \
    --volume /mnt/share/win-share/gitlab/logs/:/var/log/gitlab \
    --volume /mnt/share/win-share/gitlab/data/:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```
&emsp;&emsp;上面的命令将会下载并且开启gitlab-ce容器，并且将容器中所需要得ssh[22],http(s)[80,443]端口映射到主机指定端口[222,880,4443]，以方便用户访问，所有得gitlab数据都将存储在/mnt/share/win-share/gitlab中的三个文件夹中。  
&emsp;&emsp;如果要配置gitlab容器，可以使用```sudo docker exec -it gitlab /bin/bash```命令进入容器得shell，此时使用得是root身份进入，进入可以配置密码，也可以编辑```/etc/gitlab/gitlab.rb```文件，对gitlab进行自由配置。额外提示，如果容器使用得是官方得gitlab安装包安装，那么gitlab得所有配置文件都存储在一个单一得文件```/etc/gitlab/gitlab.rb```中。    
&emsp;&emsp;如果想和github一样当你关注得仓库有新的动态时接受到这些信息，那么需要[手动配置电子邮件信息](https://docs.gitlab.com/omnibus/settings/smtp.html)，如果需要启用HTTPS访问gitlab，可以参考这篇文章,[启用GITLAB得HTTPS](https://docs.gitlab.com/omnibus/settings/nginx.html#enable-https)。  
&emsp;&emsp;列出一些有用的管理容器得命令
```
#进入容器得shell gitlab为容器名称
docker exec -it gitlab /bin/bash
#重启或关闭容器
docker restart gitlab
docker stop gitlab
docker start gitlab
```  
更多有用得命令请参考[docker使用教程]()
[jump](#jump)

# [3 升级gitlab](#3)
&emsp;&emsp;因为gitlab运行所产生得配置信息，日志，以及存储得库都在宿主机指定得目录当中，因此容器仅仅只是作为一个运行环境，每次gitlab重启时从宿主机指定目录读取配置文件信息，写入日志，读取代码库。那么升级gitlab变得异常简单，仅需删除旧得容器，更新gitlab得docker镜像，以之前得命令重新创建一个新得容器即可，剩下得就是等待创建完毕，继续使用gitlab了。下面得步骤简单说明了如何升级gitlab。  
## [3.1 停止正在运行得容器 ](#3.1)
```
sudo docker stop gitlab
```
## [3.2 删除旧得gitlab容器](#3.2)
```
sudo docker rm gitlab
```
## [3.3 拉取gitlab得最新镜像到本地](#3.3)
```
sudo docker pull gitlab/gitlab-ce:latest
```
## [3.4 使用之前指定参数得命令创建新得gitlab容器](#3.4)
额外提示：确保要使用上一次创建gitlab容器得命令去创建，否则可能会出现问题。
```
sudo docker run --detach \
    --hostname localhost \
    --publish 4443:443 --publish 880:80 --publish 222:22 \
    --name gitlab \
    --restart always \
    --volume /mnt/share/win-share/gitlab/config/:/etc/gitlab \
    --volume /mnt/share/win-share/gitlab/logs/:/var/log/gitlab \
    --volume /mnt/share/win-share/gitlab/data/:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```
一旦启动完成，gitlab会重新载入之前得配置信息来更新自己，你也可以向以前一样继续使用它了。
