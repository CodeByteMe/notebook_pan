# 常用操作
## 手动启动oracle
```bash
# 登录oracle用户
su - oracle
# 登录oracle的SQL命令行
sqlplus /nolog
# 启动oracle
startup
# 退出oracle的SQL命令行
exit
# 启动oracle监听
lsnrctl start
```
## 手动关闭oracle
```bash
# 登录oracle用户
su - oracle
# 登录oracle的SQL命令行
sqlplus /nolog
# 关闭oracle
shutdown
# 退出oracle的SQL命令行
exit
# 关闭oracle监听
lsnrctl stop
```
[参考1](https://www.cnblogs.com/mchina/archive/2012/11/27/2782993.html)
[参考2](https://www.cnblogs.com/muhehe/p/7944887.html)
[参考3](https://blog.csdn.net/qq_42833774/article/details/93471106)

## oracle开机自启动
```bash
# 修改$ORACLE_HOME/bin/dbstart
# 将ORACLE_HOME_LISTNER=$1修改为ORACLE_HOME_LISTNER=$ORACLE_HOME
[oracle@localhost ~]$ vi $ORACLE_HOME/bin/dbstart
# 修改$ORACLE_HOME/bin/dbshut
# 将ORACLE_HOME_LISTNER=$1修改为ORACLE_HOME_LISTNER=$ORACLE_HOME
[oracle@localhost ~]$ vi $ORACLE_HOME/bin/dbshut
# 修改/etc/oratab文件
# 将页末的N改为Y
[oracle@localhost ~]$ vi /etc/oratab
# 输入命令dbshut和dbstart测试是否有效
[oracle@localhost ~]$ dbshut
[oracle@localhost ~]$ dbstart
# 建立自启动脚本, 添加下边的bash脚本
# ORACLE_HOME和ORACLE_USER等根据实际情况可以修改
[root@localhost init.d]# vim /etc/rc.d/init.d/oracle
```
```bash
#!/bin/bash
# oracle: Start/Stop Oracle Database 11g R2
# chkconfig: 345 90 10
# description: The Oracle Database is an Object-Relational Database Management System.
#
. /etc/rc.d/init.d/functions
LOCKFILE=/var/lock/subsys/oracle
ORACLE_HOME=/u01/app/oracle/product/11.2.0/dbhome_1
ORACLE_USER=oracle
case "$1" in
'start')
if [ -f $LOCKFILE ]; then
      echo $0 already running.
      exit 1
   fi
   echo -n $"Starting Oracle Database:"
   su - $ORACLE_USER -c "$ORACLE_HOME/bin/lsnrctl start"
   su - $ORACLE_USER -c "$ORACLE_HOME/bin/dbstart $ORACLE_HOME"
   su - $ORACLE_USER -c "$ORACLE_HOME/bin/emctl start dbconsole"
   touch $LOCKFILE
   ;;
'stop')
   if [ ! -f $LOCKFILE ]; then
      echo $0 already stopping.
      exit 1
   fi
   echo -n $"Stopping Oracle Database:"
   su - $ORACLE_USER -c "$ORACLE_HOME/bin/lsnrctl stop"
   su - $ORACLE_USER -c "$ORACLE_HOME/bin/dbshut"
   su - $ORACLE_USER -c "$ORACLE_HOME/bin/emctl stop dbconsole"
   rm -f $LOCKFILE
   ;;
'restart')
   $0 stop
   $0 start
   ;;
'status')
   if [ -f $LOCKFILE ]; then
      echo $0 started.
      else
      echo $0 stopped.
   fi
   ;;
*)
   echo "Usage: $0 [start|stop|status]"
   exit 1
esac
exit 0 
```
```bash
# 修改/etc/init.d/oracle服务文件权限
[root@localhost bin]# chmod 755 /etc/init.d/oracle
# 设置为开机启动
[root@localhost bin]# chkconfig oracle on
# 进行测试
# 开启
service oracle start
# 关闭
service oracle stop
# 重启
service oracle restart
# 如需测试可重启服务器测试
reboot
```
[参考](https://blog.csdn.net/lyd135364/article/details/81661662)