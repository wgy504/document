# 1、安装docker
&emsp;&emsp;安装docker参考docker安装文档。[https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/](https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/)
# 2、安装gitlab
## 1、下载gitlab
```
$ docker pull gitlab/gitlab-ce:latest
```
## 2、启动gitlab
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
    --publish 4443:443 \
    --publish 880:80 \
    --publish 222:22 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:/mnt/share/win-share/gitlab/etc/gitlab \
    --volume /srv/gitlab/logs:/mnt/share/win-share/gitlab/var/log/gitlab \
    --volume /srv/gitlab/data:/mnt/share/win-share/gitlab/var/opt/gitlab \
    gitlab/gitlab-ce
```