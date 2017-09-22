# 1、开始配置
## 1、安装依赖包
```
$ sudo apt-get install curl openssh-server ca-certificates postfix
```
&emsp;&emsp;其中postfix需要安装时进行配置，因此稍后编写配置信息[http://blog.csdn.net/hyman_c/article/details/64474151](http://blog.csdn.net/hyman_c/article/details/64474151)
## 2、使用apt安装gitlab-ce
### 1、信任GitLab的GPG公钥，在root用户下运行，不要用sudo：
```
curl https://packages.gitlab.com/gpg.key 2> /dev/null | sudo apt-key add - &>/dev/null
```
### 2、添加apt源，vim新建/etc/apt/sources.list.d/gitlab-ce.list文件，添加下面
```
deb https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu xenial main
```
### 3、安装gitlab-ce:
```

```

暂时出现问题，所以先不写，先写docker下配置，参考链接先留下
[http://blog.csdn.net/pucao_cug/article/details/64919820](http://blog.csdn.net/pucao_cug/article/details/64919820)
[http://blog.csdn.net/discoverer100/article/details/51814171](http://blog.csdn.net/discoverer100/article/details/51814171)
[http://www.jianshu.com/p/036c60e78952](http://www.jianshu.com/p/036c60e78952)