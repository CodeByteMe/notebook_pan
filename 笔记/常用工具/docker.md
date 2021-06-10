# docker相关基础教程
* docker是一种容器,用于实现虚拟化,且容器之间不会相互有接口(沙箱机制),性能开销低  
* docker的三个概念
    * 镜像(Image): 相当于一个root文件系统
    * 容器(Container): 镜像是静态的,容器是镜像运行时的实体(类似类与实例).容器可以被创建,启动,停止,删除,暂停.
    * 仓库(Repository): 相当于代码控制中心,用来保存镜像
* docker常识
```bash
# docker: Docker的二进制执行文件
# run: 运行一个容器
# tomcat:9.0.36: 镜像:版本号
# /bin/bash: 在启动的容器里启动bash
# -t: 在容器内指定一个伪终端或终端
# -i: 可以与容器进行输入输出交流
docker run -it tomcat:9.0.36 /bin/bash
```
* 常用命令
```bash
# 运行容器
docker run --name mytomcat -p 8082:8080 -v $PWD/mytest:/usr/local/tomcat/webapps/ROOT/ -d tomcat:9.0.36
# 查看运行容器,容器状态有
#   created（已创建）
#   restarting（重启中）
#   running 或 Up（运行中）
#   removing（迁移中）
#   paused（暂停）
#   exited（停止）
#   dead（死亡）
#   docker ps
docker ps
# 查看所有容器
docker ps -a
# 查看容器的标准输出
docker logs mytomcat
# 停止容器
docker stop mytomcat
# 创建容器的bash
docker exec -it mytomcat /bin/bash
# 删除容器
docker rm -f 容器名
# 导出容器(导出后再导入可能无法运行)
docker export 容器名 > test.tar
# 导入容器(导出后再导入可能无法运行)
docker import 容器名.tar 新容器名:版本
# 容器导入后运行需要写到运行命令,否则报Error response from daemon: No command specified(加了运行命令也可能出错)
# docker run --name mytomcat -p 8082:8080 -v $PWD/mytest:/usr/local/tomcat/webapps/ROOT/ -d 新容器名:版本 "catalina.sh run"
# 主机目录拷贝到容器目录
docker cp /www/runoob 96f7f14e99ab:/www/
# 主机目录拷贝到容器目录并改名
docker cp /www/runoob 96f7f14e99ab:/www
# 容器目录拷贝到主机目录
docker cp  96f7f14e99ab:/www /tmp/
# 暂停容器
docker pause mytomcat
# 恢复容器
docker unpause mytomcat
# 容器a404c6c174a2保存为新的镜像
docker commit -a "easul" -m "new file" a404c6c174a2  easul/tomcat:v1
# 将镜像easul/tomcat:v3归档文件到mytomcat.tar
docker save -o mytomcat.tar easul/tomcat:v3
# 将归档文件mytomcat.tar恢复为镜像easul/tomcat:v3(无法修改镜像名)
docker load -i mytomcat.tar
# 查看本地镜像 
docker images
# 搜索镜像
docker search tomcat
# 拉取镜像
docker pull tomcat:9.0.36
# 删除镜像
docker rmi -f 镜像名
```
[参考菜鸟教程](https://www.runoob.com/docker/docker-tutorial.html)  
[基础实例](https://www.runoob.com/docker/docker-container-usage.html)  
[常用命令](https://www.runoob.com/docker/docker-command-manual.html)  

# docker拉取并使用centos7
```bash
# 拉取centos7
docker pull centos:7
# 运行centos7的容器,不加/bin/bash,容器运行后会立即停止
docker run --name big_data -d -i -t centos:7 /bin/bash
# 进入容器的命令行模式
docker exec -it big_data bash
# 更新yum
yum update
# 安装网络工具包
yum install -y net-tools
```
[参考](https://www.cnblogs.com/adolfmc/p/12124427.html)

# docker容器添加端口信息
```bash
# 停止容器
docker stop container_id
# 停止docker及其socket
sudo systemctl stop docker
sudo systemctl stop docker.socket
# 修改配置文件,
sudo vim /var/lib/docker/containers/[containerId]/hostconfig.json
# hostconfig.json的键PortBindings配置如下
# 22是容器端口,5022是本地端口
```
```json
"PortBindings":{
    "22/tcp":[{
        "HostIp":"",
        "HostPort":"5022"
    }]
} 
```
```bash
# 修改另一个配置文件,
sudo vim /var/lib/docker/containers/[containerId]/config.v2.json 
# Config键的AttachStderr键下添加下边的配置
```
```json
"AttachStderr": false,
"ExposedPorts":{
    "22/tcp":{}
}
```
```bash
# 然后配置Ports项,之后重启docker即可看到效果
sudo systemctl start docker
docker ps -a
```
```json
"Ports": {
    "22/tcp": [{
        "HostIp": "0.0.0.0",
        "HostPort": "5022"
    }]
}
```

[参考](https://www.cnblogs.com/keh123000/p/13936697.html)

# docker添加ssh供vscode免密登录
```bash
# 更新yum
yum -y update
# 安装必要工具
yum -y install passwd openssl openssh-server  openssh-clients
# 添加秘钥
ssh-keygen -q -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key -N ''
ssh-keygen -q -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''
ssh-keygen -t dsa -f /etc/ssh/ssh_host_ed25519_key -N ''
# 修改sshd_config
vim /etc/ssh/sshd_config
UsePAM yes 改为 UsePAM no
UsePrivilegeSeparation sandbox 改为 UsePrivilegeSeparation no
PubkeyAuthentication yes
RSAAuthentication yes
# 启动sshd为后台模式
/usr/sbin/sshd -D &
# 查看sshd是否启动
ps -ef | grep sshd 
# 修改root密码
passwd root
# 将rsa.pub的秘钥复制为authorized_keys
cd /etc/ssh/
mkdir /root/.ssh/
cat ssh_host_rsa_key.pub  >> authorized_keys
mv  authorized_keys /root/.ssh/
# 复制rsa的私钥
cat ssh_host_rsa_key
```
[参考_1](https://www.cnblogs.com/0to9/p/11309587.html)  
[参考_2](https://blog.csdn.net/u013140345/article/details/79777311)  
[参考_3](https://blog.csdn.net/wwb1234560/article/details/107521203)  