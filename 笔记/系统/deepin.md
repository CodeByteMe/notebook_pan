# 安装ssr-command-client
1. 先安装[Python3](https://blog.csdn.net/wwb1234560/article/details/110329822)
2. 下载`ssr-command-client`并安装相应依赖

```bash
git clone https://github.com/TyrantLucifer/ssr-commend-client.git
cd ssr-commend-client
pip3 install -r requirement.txt
```

3. 是从目录下的`.\main.sh`进行相应操作
4. Linux终端设置代理方法

```bash
# 终端设置代理
export ALL_PROXY=socks5://127.0.0.1:1080
# 查看代理是否设置成功
curl http://ip-api.com/json/?lang=zh-CN
# 取消终端代理
unset ALL_PROXY
```

[参考](https://www.linuxstudy.cn/archives/67.html)

# deepin使用L2TP时注意
连接L2TP的`密钥交换协议`和`安全封装协议`都写`aes128-sha1-modp2048!`

[参考](https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients-zh.md#linux)

# ffmpeg下载m3u8
```bash
ffmpeg -i http://www.china.com/video/pla.m3u8 -c copy /home/chinese/mydl_video.mp4
```

[参考](https://bbs.deepin.org/post/200740#mod=viewthread&tid=200740&extra=)

# docker
## 安装docker
```bash
# Docker要求系统内核版本高于3.10，查看Linux内核
uname -r
# 使用以下命令更新apt包索引
sudo apt-get update
# 安装以下包以允许apt通过HTTPS使用存储库
sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
# 添加Docker的官方GPG密钥并载入密钥，载入密钥后会提示OK
wget https://download.docker.com/linux/debian/gpg
sudo apt-key add gpg
rm pgp
# 安装deepin仓库的Docker
sudo apt-get install docker-ce
# 使用docker version查看是否安装成功
docker version
```
[参考](https://www.cnblogs.com/aioe/p/13328663.html)  
[docker命令大全](https://www.runoob.com/docker/docker-command-manual.html)  

## 查看docker version时报错Got permission denied while trying to connect to the Docker daemon socket
因为Unix socket属于root用户,需要root权限
```bash
#添加docker用户组
sudo groupadd docker
#检测当前用户是否已经在docker用户组中, 其中XXX为用户名, 例如deepin
sudo gpasswd -a $XXX docker
#将当前用户添加至docker用户组
sudo gpasswd -a $USER docker
#更新docker用户组
newgrp docker
# 检测是否成功
docker version
# 然后可以用用户名登入
su deepin
```
[参考](https://blog.csdn.net/liangllhahaha/article/details/92077065)

## deepin添加docker镜像源
```bash
# 有该文件就修改,没有就创建
sudo vim  /etc/docker/daemon.json
# 下边的镜像源,163的可以用
```
```json
{
  "registry-mirrors": [
    "https://dockerhub.azk8s.cn",
    "https://hub-mirror.c.163.com"
  ]
}
```
```bash
# 编辑好保存后重启docker
sudo systemctl daemon-reload
sudo systemctl restart docker
```
[参考](https://blog.csdn.net/z_ruitao/article/details/105184787)

# deepin同时开启wifi和热点
```bash
# 通过如下命令找到valid interface combinations:,可能显示如下
# #{ managed } <= 1, #{ AP, P2P-client, P2P-GO } <= 1, #{ P2P-device } <= 1,
# total <= 3, #channels <= 2
# managed需要大于等于1,AP就是热点,channels至少需要2
# 记录下设备名,一般为phy0,在第一行Wiphy后面
iw phy
# 如果配置正确,先打开热点
# 然后添加无线网虚拟接口,
# sudo iw phy <设备名> interface add <自己起一个接口名，不要和原来的重复> type managed
sudo iw phy phy0 interface add wlp1s-deepin type managed
# 然后就会出现一个新的连接通道,连接无线即可.
# 如果要回复原状,可以删除新建的interface,然后断开热点重连无线网.或者直接重启系统
# sudo iw dev <新建的接口名> del
sudo iw dev wlp1s-deepin del
```

[参考](https://ywnz.com/linuxjc/6267.html)

# deepin使用RDP协议远程windows

```bash
# -u 用户名
# -p 密码
# -f 全屏,可使用CTRL + ALT + ENTER进行全屏与非全屏的替换
# -g 分辨率,如-g 1366*768
# 192.168.23.102 客户端IP
# -n 客户端主机名(显示windows任务管理器中的连接客户端名),如PC
# -a 连接颜色深度(最高16位,16位为真彩色)
# -0 数字0 表示连接上windows 控制台，等效mstsc /console命令
# -r disk:floppy=/mnt/floppy, 共享本地磁盘给远程
rdesktop -a 16 -u administrator -p T1igJEa~2 -f 192.168.1.7 -r disk:deepin=/home/deepin/Desktop
```
[参考](http://cache.baiducontent.com/c?m=0ZPBfl4cnQVjpPaOEWAC87NXlar7dMGg_8S9WG2a3jvw0rdy14q-eLGYI-UE3DgcVjq8HfMF0Z02IQKmpX3qPPmpvhAnFLgkj5T73S3HplaTm1RRje_heC8zgQG8s1OS7eYaFE3r-tRUSLn4rw0bD5dqcPa-nczmNwkkwNeDhte&p=846fc64ad48011a05bed9412150e86&newp=8b2a971986cc42af58fecd3d525297231610db2151d4d7156b82c825d7331b001c3bbfb423291707d2c7786307ae485decf03774350923a3dda5c91d9fb4c57479d4&s=cfcd208495d565ef&user=baidu&fm=sc&query=rdesktop%D4%B6%B3%CC%C1%AC%BD%D3&qid=aa3fead7001cc572&p1=1)

# deepin调整屏幕亮度为50%

```bash
dbus-send --print-reply --dest=com.deepin.daemon.Display /com/deepin/daemon/Display com.deepin.daemon.Display.SetBrightness string:'eDP-1' double:0.5
```
[参考](https://blog.csdn.net/eric_rain/article/details/104948296)

# deepin如果打不开设置中心

```bash
# 可以更新系统,可能会有dpkg相关的报错,可以无视
sudo apt-get dist-upgrade
# 也可以直接安装控制中心
sudo apt install dde-control-center
# deepin手动更新内核
sudo apt install linux-image-deepin-stable-amd64 linux-headers-deepin-stable-amd64
```  
[参考1](https://blog.csdn.net/weixin_34067102/article/details/91696944)  
[参考2](https://bbs.deepin.org/post/202092)  

# deepin分区出现了很多个squashfs
原因: 安装了`snap`或`xdroid`(安装了snap后, 每安装一个软件, 就会多一个虚拟分区, 虚拟分区运行软件更安全)  
解决方法:  
```bash
# terminal里输入如下命令用deepin的编辑器打开显示设备的配置文件
sudo deepin-editor /usr/lib/udev/rules.d/80-udisks2.rules 
# 在文末加入
KERNEL == "loop*", ENV {UDISKS_IGNORE} = "1"
```
[参考](https://bbs.deepin.org/post/213619)  