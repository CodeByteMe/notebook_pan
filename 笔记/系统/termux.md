# termux安装及配置
安装好`termux`之后,需要装一些软件来进行相应的环境配置.  
* 安装`ssh`

```bash
pkg install openssh
# 安装好之后需要手动启动
sshd
# 客户端连接需要使用8022端口.由于termux是单用户,所以不用输入用户名也可以登陆.输入了会忽略.
ssh localhost -p 8022
```
[参考](https://www.linuxprobe.com/termux-ssh-server.html)

* 安装`python3`

```bash
pkg instll python
```

* 安装`nodejs`

会一同装好`npm`  
```bash
pkg install nodejs
```

* 安装终端配色

需要已经安装好`wget`  
```bash
# 安装终端配色脚本
sh -c "$(curl -fsSL https://github.com/Cabbagec/termux-ohmyzsh/raw/master/install.sh)"
# 修改颜色,只需要再次运行该脚本即可
~/termux-ohmyzsh/install.sh
```
出现下边两个选项是来选`背景色`和`字体`  
```
Enter a number, leave blank to not to change: 14
Enter a number, leave blank to not to change: 6
```
该脚本会自动将外置存储挂载,在`~/storage/`下  
修改主题可以使用如下方式  
```bash
vim .zshrc
# 然后修改第一行
ZSH_THEME="jaischeema"
```
[参考](https://www.freebuf.com/geek/170510.html)

* npm安装`wcode`

```bash
npm install -g wcode
# 启动
wcode -p 9999 /home
# 访问
ip:9999
```
[参考_1](https://zhuanlan.zhihu.com/p/106298311)  
[参考_2](https://corvo.myseu.cn/)  
[参考_3](https://www.sqlsec.com/2018/05/termux.html#toc-heading-186)  

# termux安装内网穿透工具cploar
```bash
# 安装dnsutils工具包
apt install dnsutils
# 下载最新版cploar
curl -O -L https://www.cpolar.com/static/downloads/cpolar-stable-linux-arm.zip
# 解压cploar
unzip cpolar-stable-linux-arm.zip
```
解压好之后,去[官网](https://www.cpolar.com)注册帐号,获取`token`  
登录后台[仪表盘](https://dashboard.cpolar.com/)，验证，复制`token`值  
```bash
# 认证token
./cpolar authtoken xxxxxxxxxxtokenxxxxxxx
# 映射端口到外网
./cpolar http 8080
# 映射SSH
./cpolar tcp 22
```
映射完后等一会会有链接出现  
[在线文档](https://www.cpolar.com/docs),[参考](https://blog.csdn.net/probezy/article/details/90509879)  

# termux避免长时间熄屏自动断开ssh
```bash
termux-wake-lock
```

# termux加载启动脚本
```bash
# 将脚本添加到.zshrc中即可
vim .zshrc
# 添加启动脚本到最后一行
./study_init.sh
```

# code-server安装
## termux安装code-server
```bash
# 升级软件源
pkg update -y
# 安装code-server相应软件
pkg install -y python nodejs yarn
# 安装code-server
# 安装的时候会非常慢,耐心等待(在google-cloud/logging处会等很久)
yarn global add code-server
# 修复Visual Studio Code依赖的spdlog
cd ~/.config/yarn/global/node_modules/code-server/lib/vscode/node_modules/spdlog/
vim binding.gyp
# 在"target_name": "spdlog"下面添加"libraries": [ "-latomic" ],然后保存退出
```
```json
"targets": [{
    "target_name": "spdlog",
    "libraries": [ "-latomic" ],
    "sources": [
            "src/main.cc",
            "src/logger.cc"
],
```
```bash
# 重新编译 spdlog
npm install
# 检测spdlog是否正确编译
npm test
# 回到 home 目录并执行下面的命令来启动 code-server
cd ~
# 这样启动后localhost可以访问
code-server --auth none --disable-telemetry
# 这样启动后会有密码,切局域网可以访问
export PASSWORD=123456 
nohup code-server --auth password --bind-addr 0.0.0.0:8080 &
# 如果不设置登录密码可以这样查看
cat ~/.config/code-server/config.yaml
```
[参考](https://www.v2ex.com/t/693969)

## code-server官方安装教程
```bash
# 先更新一下仓库
apt update && apt upgrade
# 然后安装code-server所需要的所有软件
apt install build-essential python git nodejs yarn
# 然后安装code-server
yarn global add code-server
# 直接运行code-server, 这样localhost:8080就会有界面出现
code-server
# 如果要对code-server更新则执行
yarn global upgrade code-server --latest

# 另外, 启动的时候可以如下
# 这样启动后会有密码,切局域网可以访问
export PASSWORD=123456 
nohup code-server --auth password --bind-addr 0.0.0.0:8080 &
# 如果不设置登录密码可以这样查看
cat ~/.config/code-server/config.yaml
```
[参考](https://github.com/cdr/code-server/blob/5d5e6314a15877d1e6d6f82fcdcfb38355f1f758/docs/install.md#termux)

## termux挂载的debian中安装code-server 
这个方法适用于在termux中安装`code-server`怎么都安装不好的情况(如果速度过慢可以使用小飞机加速)  
使用之前可以参考[termux安装debian](#termux安装debian)先行安装好系统  
```bash
# 在`http://github.com/cdr/code-server/releases`中找到最新版的arm64的code-server的软件包链接并复制, 使用wget下载
cd ~
wget https://github.com/cdr/code-server/releases/download/v3.10.2/code-server_3.10.2_arm64.deb
# 下载好后使用dpkg -i 下载的文件名进行安装
dpkg -i code-server_3.10.2_arm64.deb
# 接着就可以启动了
# 这样启动后会有密码, 且切局域网可以访问
export PASSWORD=123456 
nohup code-server --auth password --bind-addr 0.0.0.0:8080 &
# 如果不设置登录密码可以这样查看
cat ~/.config/code-server/config.yaml
```

# termux安装debian
1. 下载[AnLinux](https://f-droid.org/zh_Hans/packages/exa.lnx.a/), 装好后
    1. 点击选择, 选择一个linux发行版, 这里选择`debian`
    2. 点击复制, 然后点击启动, 将命令粘贴到termux命令行运行.中间过程可能较慢, 建议使用小飞机进行加速
    3. 安装好后使用`./start-debian.sh`来运行新安装的debian
2. 一些其他操作

```bash
# termux的操作
# 挂载手机外部存储到debian
vi ./start-debian.sh
# 然后去掉`#command+=“ -b /sdcard”`的注释
# 这样是默认挂载到debian的根目录, 可以改成下边的格式挂载到/root目录下
command+=“ -b /sdcard:/root/sdcard”

# debian的操作
# 进行软件源更新
apt update
apt upgrade
# 安装常用软件
apt install gcc
apt install g++
apt install gfortran
apt install cmake
# 如需在debian安装code-server可参考`termux挂载的debian中安装code-server`
```
[参考](https://zhuanlan.zhihu.com/p/95865982)

# 安卓5安装termux
```bash
# 安卓5能够安装的termux版本为0.49
# 因为新版termux软件源变了, 安卓5装好termux后是无法进行仓库更新的
# 需要先修改仓库源地址
vi $PREFIX/etc/apt/sources.list.d/game.list
# 原地址为下边这个, 先注释掉他
# deb https://dl.bintray.com/grimler/game-packages-21 games stable
# 然后换成下边这个地址
deb https://termux.com/game-packages-21-bin games stable

# 接着修改另一个仓库源地址
vi $PREFIX/etc/apt/sources.list.d/science.list
# 原地址为下边这个, 先注释掉他
# deb https://dl.bintray.com/grimler/science-packages-21 science stable
# 然后换成下边这个地址
deb https://termux.com/science-packages-21-bin science stable
# 接着更新仓库源, 
pkg update
pkg upgrade
# 更新仓库源的过程中可能有些选项会选错, 导致上边的仓库源地址变成原来的了(更新完后再更新会有报错)
# 这个时候可以再检查下仓库源地址是不是变回原来的了, 如果是就再改一下

# 安装openssh
pkg install openssh
# 这个貌似需要密码, 使用passwd设置一下密码
passwd
# 然后开启sshd即可使用其他设备登录了
ssh 手机ip -p 8022
```
[参考1](https://termux.com/science-packages-21-bin)
[参考2](https://termux.com/game-packages-21-bin)