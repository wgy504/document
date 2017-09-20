
```
#列出当前目录下所有文件夹信息
ls -l | grep -E "d-|dr" | awk '{print $NF}'
ls -d */    #当当前目录文件夹时会报错，否则会列出所有文件夹，但是后面均带/符号
#eval xxx 将xxx当作命令执行

#查看sshd的ps信息和netstat信息, 排除unix ::: grep关键字
sudo ps auxw | grep sshd && sudo netstat -apn | grep sshd | grep -v "unix\|:::\|grep"

#查看eth0的ipv4地址
ifconfig eth0 | grep "inet addr" | awk '{printf $2}' | awk -F ":" '{printf $2}'

#查看wan口接口
uci get osconfig.network.waniface


#查看机器内所有用户的信息
cat /etc/passwd|grep -v nologin|grep -v halt|grep -v shutdown|awk -F":" '{ print $1"|"$3"|"$4 }'|more

cat /etc/passwd 
#可以查看所有用户的列表
w 
#可以查看当前活跃的用户列表
cat /etc/group 
#查看用户组
```

#[linux下怎么查看ssh的用户登录日志](http://www.cnblogs.com/wangkangluo1/archive/2011/09/23/2185976.html)
```
cd /var/log
less secure
less messages
last

#linux下让脚本后台自动执行并且将日志按时间分割保存
nohup your_command > nohup-`date +%Y-%m-%d-%H`.out 2>&1 & 只输出错误信息到日志文件
nohup ./program >/dev/null 2>log &
什么信息也不要
nohup ./program >/dev/null 2>&1 &
```