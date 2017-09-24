把以下脚本保存为nginx文件放入```/etc/init.d/nginx```，就可以通过```/etc/init.d/nginx start```命令启动```nging```，```/etc/init.d/nginx stop```命令停止```nginx```，```/etc/init.d/nginx restart```命令重启```nginx```。如果需要开机自启动```Nginx```，执行```chkconfig --add nginx && chkconfig --level nginx 2345 on```。
```
#! /bin/sh

PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

DESC="nginx daemon"

NAME=nginx

DAEMON=/usr/local/nginx/sbin/$NAME

CONFIGFILE=/usr/local/nginx/conf/$NAME.conf

PIDFILE=/usr/local/nginx/logs/$NAME.pid

SCRIPTNAME=/etc/init.d/$NAME

set -e

[ -x "$DAEMON" ] || exit 0

do_start() {
    $DAEMON -c $CONFIGFILE || echo -n "nginx already running"
}

do_stop() {
    kill -INT `cat $PIDFILE` || echo -n "nginx not running"
}

do_reload() {
    kill -HUP `cat $PIDFILE` || echo -n "nginx can't reload"
}

case "$1" in
    start)
        echo -n "Starting $DESC: $NAME"
        do_start
        echo "."
    ;;
    stop)
        echo -n "Stopping $DESC: $NAME"
        do_stop
        echo "."
    ;;
    reload|graceful)
        echo -n "Reloading $DESC configuration..."
        do_reload
        echo "."
    ;;
    restart)
        echo -n "Restarting $DESC: $NAME"
        do_stop
        do_start
        echo "."
    ;;
    *)
        echo "Usage: $SCRIPTNAME {start|stop|reload|restart}" >&2
        exit 3
    ;;
    esac
exit 0
```