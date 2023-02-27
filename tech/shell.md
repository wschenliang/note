
## redis

```shell
#!/bin/sh
# chkconfig: 2345 10 90  
# description: Start and Stop redis   

REDISPORT=6379
EXEC=/usr/local/redis/src/redis-server
CLIEXEC=/usr/local/redis/src/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/usr/local/redis/redis.conf"

case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF &
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $CLIEXEC -p $REDISPORT shutdown
                while [ -x /proc/${PID} ]
                do
                    echo "Waiting for Redis to shutdown ..."
                    sleep 1
                done
                echo "Redis stopped"
        fi
        ;;
    restart)
        "$0" stop
        sleep 3
        "$0" start
        ;;
    *)
        echo "Please use start or stop or restart as first argument"
        ;;
esac
```

## elasticsearch

```shell
#!/bin/sh
#chkconfig: 2345 80 05
#description: elasticsearch
 
export JAVA_HOME=/env/jdk-11.0.11
export JRE_HOME=${JAVA_HOME}/jre
export PATH=PATH:${JAVA_HOME}/bin:$PATH
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
 
case "$1" in
start)
    su elk<<!
    cd /usr/local/elasticsearch
    ./bin/elasticsearch -d
!
    echo "elasticsearch startup"
    ;;
stop)
    es_pid=`ps aux|grep elasticsearch | grep -v 'grep elasticsearch' | awk '{print $2}'`
    kill -9 $es_pid
    echo "elasticsearch stopped"
    ;;
restart)
    es_pid=`ps aux|grep elasticsearch | grep -v 'grep elasticsearch' | awk '{print $2}'`
    kill -9 $es_pid
    echo "elasticsearch stopped"
    su elk<<!
    cd /usr/local/elasticsearch
    ./bin/elasticsearch -d
!
    echo "elasticsearch startup"
    ;;
*)
    echo "start|stop|restart"
    ;;
esac
 
exit $?
```

## kibana

```shell
#!/bin/sh
#chkconfig: 2345 80 90
#description: kibana

case "$1" in
start)
    su elk<<!
    cd /usr/local/kibana
    nohup ./bin/kibana &
!
    echo "kibana startup"
    ;;
stop)
    kibana_pid=`ps aux|grep node | grep -v 'grep node' | awk '{print $2}'`
    kill -9 $kibana_pid
    echo "kibana stopped"
    ;;
restart)
    kibana_pid=`ps aux|grep node | grep -v 'grep node' | awk '{print $2}'`
    kill -9 $kibana_pid
    echo "kibana stopped"
    su elk<<!
    cd /usr/local/kibana
    nohup ./bin/kibana &
!
    echo "kibana startup"
    ;;
*)
    echo "start|stop|restart"
    ;;
esac

exit $?
```

## mongodb

```shell
#!/bin/sh
#chkconfig: 2345 80 90
#description: mongodb
start() {
 /usr/local/mongodb/bin/mongod -f /usr/local/mongodb/mongodb.conf
}
 
stop() {
  /usr/local/mongodb/ -f /usr/local/mongodb/mongodb.conf --shutdown
}
 
case "$1" in
  start)
 start
 ;;
  stop)
 stop
 ;;
  restart)
 stop
 start
 ;;
  *)
 echo $"Usage: $0 {start|stop|restart}"
 exit 1
esac
```

## rabbitmq

```shell
#!/bin/sh
#
# rabbitmq-server RabbitMQ broker
#
# chkconfig: - 80 05
# description: Enable AMQP service provided by RabbitMQ
#

### BEGIN INIT INFO
# Provides:          rabbitmq-server
# Required-Start:    $remote_fs $network
# Required-Stop:     $remote_fs $network
# Description:       RabbitMQ broker
# Short-Description: Enable AMQP service provided by RabbitMQ broker
### END INIT INFO

# Source function library.
. /etc/init.d/functions

PATH=/sbin:/usr/sbin:/bin:/usr/bin
NAME=rabbitmq-server
DAEMON=/usr/sbin/${NAME}
CONTROL=/usr/sbin/rabbitmqctl
DESC=rabbitmq-server
USER=rabbitmq
ROTATE_SUFFIX=
INIT_LOG_DIR=/var/log/rabbitmq
PID_FILE=/var/run/rabbitmq/pid

START_PROG="daemon"
LOCK_FILE=/var/lock/subsys/$NAME

test -x $DAEMON || exit 0
test -x $CONTROL || exit 0

RETVAL=0
set -e

[ -f /etc/default/${NAME} ] && . /etc/default/${NAME}

ensure_pid_dir () {
    PID_DIR=`dirname ${PID_FILE}`
    if [ ! -d ${PID_DIR} ] ; then
        mkdir -p ${PID_DIR}
        chown -R ${USER}:${USER} ${PID_DIR}
        chmod 755 ${PID_DIR}
    fi
}

remove_pid () {
    rm -f ${PID_FILE}
    rmdir `dirname ${PID_FILE}` || :
}

start_rabbitmq () {
    status_rabbitmq quiet
    if [ $RETVAL = 0 ] ; then
        echo RabbitMQ is currently running
    else
        RETVAL=0
        ensure_pid_dir
        set +e
        RABBITMQ_PID_FILE=$PID_FILE $START_PROG $DAEMON \
            > "${INIT_LOG_DIR}/startup_log" \
            2> "${INIT_LOG_DIR}/startup_err" \
            0<&- &
        $CONTROL wait $PID_FILE >/dev/null 2>&1
        RETVAL=$?
        set -e
        case "$RETVAL" in
            0)
                echo SUCCESS
                if [ -n "$LOCK_FILE" ] ; then
                    touch $LOCK_FILE
                fi
                ;;
            *)
                remove_pid
                echo FAILED - check ${INIT_LOG_DIR}/startup_\{log, _err\}
                RETVAL=1
                ;;
        esac
    fi
}

stop_rabbitmq () {
    status_rabbitmq quiet
    if [ $RETVAL = 0 ] ; then
        set +e
        $CONTROL stop ${PID_FILE} > ${INIT_LOG_DIR}/shutdown_log 2> ${INIT_LOG_DIR}/shutdown_err
        RETVAL=$?
        set -e
        if [ $RETVAL = 0 ] ; then
            remove_pid
            if [ -n "$LOCK_FILE" ] ; then
                rm -f $LOCK_FILE
            fi
        else
            echo FAILED - check ${INIT_LOG_DIR}/shutdown_log, _err
        fi
    else
        echo RabbitMQ is not running
        RETVAL=0
    fi
}

status_rabbitmq() {
    set +e
    if [ "$1" != "quiet" ] ; then
        $CONTROL status 2>&1
    else
        $CONTROL status > /dev/null 2>&1
    fi
    if [ $? != 0 ] ; then
        RETVAL=3
    fi
    set -e
}

rotate_logs_rabbitmq() {
    set +e
    $CONTROL rotate_logs ${ROTATE_SUFFIX}
    if [ $? != 0 ] ; then
        RETVAL=1
    fi
    set -e
}

restart_running_rabbitmq () {
    status_rabbitmq quiet
    if [ $RETVAL = 0 ] ; then
        restart_rabbitmq
    else
        echo RabbitMQ is not runnning
        RETVAL=0
    fi
}

restart_rabbitmq() {
    stop_rabbitmq
    start_rabbitmq
}

case "$1" in
    start)
        echo -n "Starting $DESC: "
        start_rabbitmq
        echo "$NAME."
        ;;
    stop)
        echo -n "Stopping $DESC: "
        stop_rabbitmq
        echo "$NAME."
        ;;
    status)
        status_rabbitmq
        ;;
    rotate-logs)
        echo -n "Rotating log files for $DESC: "
        rotate_logs_rabbitmq
        ;;
    force-reload|reload|restart)
        echo -n "Restarting $DESC: "
        restart_rabbitmq
        echo "$NAME."
        ;;
    try-restart)
        echo -n "Restarting $DESC: "
        restart_running_rabbitmq
        echo "$NAME."
        ;;
    *)
        echo "Usage: $0 {start|stop|status|rotate-logs|restart|condrestart|try-restart|reload|force-reload}" >&2
        RETVAL=1
        ;;
esac

exit $RETVAL
```

## node_exporter

```shell
#!/bin/bash
#
# /etc/rc.d/init.d/node_exporter
#
#  Prometheus node exporter
#
#  description: Prometheus node exporter
#  processname: node_exporter

# Source function library.
. /etc/rc.d/init.d/functions

PROGNAME=node_exporter
PROG=/opt/apps/node_exporter/$PROGNAME
USER=root
LOGFILE=/var/log/prometheus.log
LOCKFILE=/var/run/$PROGNAME.pid

start() {
    echo -n "Starting $PROGNAME: "
    cd /opt/prometheus/
    daemon --user $USER --pidfile="$LOCKFILE" "$PROG &>$LOGFILE &"
    echo $(pidofproc $PROGNAME) >$LOCKFILE
    echo
}

stop() {
    echo -n "Shutting down $PROGNAME: "
    killproc $PROGNAME
    rm -f $LOCKFILE
    echo
}

case "$1" in
    start)
    start
    ;;
    stop)
    stop
    ;;
    status)
    status $PROGNAME
    ;;
    restart)
    stop
    start
    ;;
    reload)
    echo "Sending SIGHUP to $PROGNAME"
    kill -SIGHUP $(pidofproc $PROGNAME)#!/bin/bash
    ;;
    *)
        echo "Usage: service node_exporter {start|stop|status|reload|restart}"
        exit 1
    ;;
esac
```

## 重置idea

```shell
#!/bin/sh
# reset jetbrains ide evals

OS_NAME=$(uname -s)
JB_PRODUCTS="IntelliJIdea CLion PhpStorm GoLand PyCharm WebStorm Rider DataGrip RubyMine AppCode"

if [ $OS_NAME == "Darwin" ]; then
	echo 'macOS:'

	for PRD in $JB_PRODUCTS; do
    	rm -rf ~/Library/Preferences/${PRD}*/eval
    	rm -rf ~/Library/Application\ Support/JetBrains/${PRD}*/eval
	done
elif [ $OS_NAME == "Linux" ]; then
	echo 'Linux:'

	for PRD in $JB_PRODUCTS; do
    	rm -rf ~/.${PRD}*/config/eval
    	rm -rf ~/.config/${PRD}*/eval
	done
else
	echo 'unsupport'
	exit
fi

echo 'done.'
```