# server.xml解析
```xml
<Server port="8005" shutdown="SHUTDOWN">  
    <!--
        Services是一组engine的集合, 包括线程池Executor和连接器Connector的定义
    -->
    <Service name="Catalina">  
        <!--
            默认一个Connector一个线程池, 若想多个Connector共享一个线程池, 则可以设置该标签
        -->
        <Executor ...... />  
        <Connector ...... />  
        <!--
            一个Connector是一个对外界开放的端口, 常用属性有
            protocol, 值为
                org.apache.coyote.http11.Http11NioProtocol：NIO连接器, 并发性能良好
                org.apache.coyote.http11.Http11AprProtocol：APR连接器, 建议在生产环境使用
            redirectPort, 用于重定向端口
            SSLEnabled: 是否使用SSL传输
            executor: 指定连接器线程池名称
        -->
        <Connector ...... />  
        <!--
            Engine看作是虚拟主机的一个集合
            defaultHost指的是当指向该域名指向该服务器, 该服务器没有对应域名的虚拟主机, 默认返回的虚拟主机
                这里默认返回域名为localhost的主机
        -->
        <Engine name="Catalina" defaultHost="localhost">  
            <!--
                一个host一个虚拟主机, 如name="www.a.com"对应一个host,name="www.b.com"对应一个host
                其路径下的每个文件夹被看做一个虚拟主机下的一个应用
                name存放虚拟主机域名, 也可以直接写ip
                appBase指向虚拟主机根目录, 可以写绝对路径, 也可以写相对于tomcat根路径的相对路径
            -->
            <Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="true"> 
                <!--
                    Alias用来定义该虚拟主机的其他域名
                --> 
                <Alias>doc.a.com</Alias>  
                <!--
                    一个context一个应用, docBase指向应用的目录, 
                    可以不在appBase下, 需要指定绝对路径
                    reloadable为true, 则文件有变化后项目自动重新加载
                -->
                <Context docBase="/home/project/" reloadable="true" />  
                <!--
                    Value用于定义日志存储位置即文件名
                -->
                <Valve ....../>
            </Host>  
        </Engine>  
    </Service>  
</Server>  
```
[参考](https://blog.csdn.net/yztezhl/article/details/80753252)

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