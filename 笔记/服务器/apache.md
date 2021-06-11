# apache与nginx的简单对比
可以nginx做前端文件显示, apache做后端动态处理  

| Apache | Nginx |
| :-----:| :----:|
| 稳定, 动态请求处理强 | 擅长静态请求处理 |
| 高并发时性能弱, 耗费资源多 | 高并发能力强, 擅长反向处理, 负载均衡, 内存占用少 |

# 编译安装apache
## yum安装apache的目录解析
```yml
/usr/local/bin: 存放可执行文件
/usr/local/lib: 存放库文件
/usr/local/etc: 存放配置文件
/usr/local/share: 存放其他资源文件
```

## 源码编译步骤
```yml
配置: configure,依赖于gcc编译器，binutils，glibc。此步骤会配置软件特性，检查编译环境，生成Makefile文件
编译: make
安装: make install
```  

## 源码编译好处
1. 编译安装可以自定义编译参数, 提高性能
2. 解决不必要的软件依赖
3. 方便清理(直接删除目录, 或在安装目录`make uninstall`)与数据迁移(可以直接复制编译的目录到其他机器)

## 源码编译依赖
1. httpd依赖于apr(apache portable runtime), apr-util
2. httpd-2.4依赖apr1.4+

## 源码编译安装步骤
```bash
# 安装基础依赖软件
yum install gcc
yum install pcre-devel
yum install binutils
yum install glibc
yum install openssl-devel

# 下载软件包
# 这里使用了apr-1.6.5.tar.gz, apr-util-1.6.1.tar.gz, httpd-2.4.37.tar.gz
# 可以选择下载最新版减少漏洞攻击
wget https://mirrors.tuna.tsinghua.edu.cn/apache/apr/apr-1.6.5.tar.gz
wget https://mirrors.tuna.tsinghua.edu.cn/apache/apr/apr-util-1.6.1.tar.gz
wget https://mirrors.tuna.tsinghua.edu.cn/apache/httpd/httpd-2.4.37.tar.gz

# 解压压缩包
tar -zxvf apr-1.6.5.tar.gz
tar -zxvf apr-util-1.6.1.tar.gz
tar -zxvf httpd-2.4.37.tar.gz

# 编译安装
cd apr-1.6.5
./configure --prefix=/usr/local/apr # 配置apr安装目录
make && make install

cd ../apr-util-1.6.1
./configure \ 
        --prefix=/usr/local/apr-util \ # 指定apr-util安装目录
        --with-apr=/usr/local/apr # 指定依赖的apr目录
make && make install
# 这时候可能报错 fatal error: expat.h: No such file or directory
# 则需要安装如下依赖, 然后继续编译安装
yum install expat-devel

cd ../httpd-2.4.37
./configure \
        --with-apr=/usr/local/apr           \ # 指定依赖的apr目录
        --with-apr-util=/usr/local/apr-util \ # 指定依赖的apr-util目录
        --prefix=/usr/local/apache \ # 指定安装目录
        --sysconfdir=/etc/httpd24  \ # 指定配置文件路径
        --enable-so                \ # 启动模块动态装卸载
        --enable-ssl               \ # 编译ssl模块
        --enable-cgi               \ # 支持cgi机制
        --enable-rewrite           \ # 支持url重写
        --with-zlib                \ # 支持数据包压缩
        --with-pcre                \ # 支持正则表达式
        --with-mpm=prefork         \ # 指定httpd的工作方式为prefork
        --enable-modules=most      \ # 启用的模块
        --enable-mpms-shared=all     # 支持三种工作模式，关于工作模式，请查看我另一篇介绍apache工作模式的文章
make && make install
```


# 更改apache相关配置
* 文件根目录

找到配置文件, 一般在`/etc/apache`或`/etc/httpd`之类的文件夹里
```bash
cd /etc/apache2
vim apache2.conf
```
修改目录为自己的自定义目录
```xml
<!-- origin path:/var/www-->
<Directory /home/deepin/Workspace/htmls>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>

<!--  文档根路径也需要修改-->
DocumentRoot "/var/www"
```
修改后重启`apache`  
```bash
sudo service apache2 restart
```

* 端口

找到配置文件, 一般在`/etc/apache`或`/etc/httpd`之类的文件夹里
```bash
cd /etc/apache2
vim ports.conf
```
修改`Listen`为需要的端口
```
Listen 80
```
修改后重启`apache`
```bash
sudo service apache2 restart
```

* 关闭和开启apache2开机自启动

```bash
# 关闭开机自启动, 服务被升级后则无法禁用开机自启动
update-rc.d -f apache2 remove
# 开启开机自启动
update-rc.d apache2 defaults
```  
如实在关不掉, 可以在系统的自启动脚本里添加`service apache2 stop`  
[参考](https://blog.csdn.net/chen200910a/article/details/22089267)