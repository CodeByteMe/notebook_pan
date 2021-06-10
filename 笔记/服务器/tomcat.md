# 报java.net.SocketException错误
* 主要原因  
在linux下,如果使用1024以下的端口则需要root权限,项目里如果使用了80端口则会出现该问题
* 解决方案
    1. 修改端口为`8080`等大于1024的端口号
    2. 进行端口转发
    ```bash
    # 转发端口
    iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
    # 取消转发
    iptables -t nat -F PREROUTING
    ```

[参考](https://www.cnblogs.com/dongxiao-yang/p/4410371.html)  
[端口转发](https://blog.csdn.net/w892824196/article/details/81201444)

# tomcat开启后,java监听ipv6
```bash
# tomcat/bin目录下新建setenv.sh(没有就新建),然后添加如下配置
JAVA_OPTS="$JAVA_OPTS -Djava.net.preferIPv4Stack=true -Djava.net.preferIPv4Addresses=true "
```

# springboot开启自启动工具, tomcat无法自启动
```xml
<!--在tomcat的server.xml的host修改context标签的reloadable为true-->
<Context path="/api" docBase="/opt/apps/tomcat-9/webapps/api/" debug="0" reloadable="true" />
```