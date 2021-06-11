# nginx与apache的比较
可以参考[apache与nginx的简单对比](apache.md#apache与nginx的简单对比)  

# nginx编译安装
```bash
# 安装基础所需依赖
sudo apt install gcc gcc-c++ automake pcre pcre-devel zlib zlib-devel openssl openssl-devel make -y
# 下载nginx源码包并解压
wget http://nginx.org/download/nginx-1.19.6.tar.gz
tar -zxvf nginx-1.19.6.tar.gz
cd nginx-1.19.6
# 指定所需参数并编译, 编译过程如有报错, 则搜索错误信息并安装所需依赖
# 默认--sbin-path, --conf-path, --error-log-path, --http-log-path的路径都在--prefix下
./configure \
    --prefix=/usr/local/nginx \ # 指向根安装目录
    --sbin-path=/usr/bin/nginx  \ # 指定执行可程序文件存放位置
    --conf-path=/etc/nginx/nginx.conf \ # 指定配置文件存放位置
    --error-log-path=/var/log/nginx/error.log \ # 指定错误日志存放位置
    --http-log-path=/var/log/nginx/access.log \ # 设定access log路径
    --pid-path=/var/run/nginx.pid \ # pid路径
    --lock-path=/var/run/nginx.lock \ # 进程锁
    -–modules-path=/usr/lib64/nginx/modules \ # 模块安装位置
```
[参考](https://www.cnblogs.com/flashfish/p/11025961.html)

# nginx常用命令
```bash
# 查看nginx可执行文件路径
ps -ef | grep nginx
# 查看nginx版本
/usr/local/nginx/sbin/nginx -v
/usr/local/nginx/sbin/nginx -V
# 查看nginx配置文件路径
/usr/local/nginx/sbin/nginx -t
# 启动
/usr/local/nginx/sbin/nginx
# 重启
/usr/local/nginx/sbin/nginx -s reload
# 停止
/usr/local/nginx/sbin/nginx -s stop
```
[参考1](https://www.cnblogs.com/cangqinglang/p/9278223.html)
[参考2](https://blog.csdn.net/richie696/article/details/113086781)

# nginx简易教程
## 参考资料如下
参考教程为[w3cschool](https://www.w3cschool.cn/nginx/)编写的文档

## nginx架构
1. 以`daemon`方式运行, 有一个`master`进程和多个`worker`进程(nginx主流方式为多进程, 也可以使用多线程)
2. `master`进程用来管理`worker`进程,
    1. master进程接收外界信号, 给worker发信号, 监控worker, 不执行任务
    2. `worker`异常退出, `master`会重启新`worker`
    3. worker进行请求的处理, 一个请求只在一个worker处理, 一个worker只处理一个请求
    4. worker个数一般为cpu核数
    5. worker之间是平等的, 且每个worker都是从master处fork过来的
3. nginx重启或停止的几种方式
    1. nginx重启可以使用`kill -HUP pid`
        1. `-HUP`是挂起进程, 挂起的同时会加载配置文件, `-9`是六亲不认杀死进程
        2. 这里挂起的时候`master`加载配置文件, 并加载新的`worker`执行新请求, 老的`worker`处理完当前所有请求后, 直接退出
    2. 使用命令行参数操作nginx
    ```bash
    # 重启
    ./nginx -s reload
    # 停止
    ./nginx -s stop
    ```