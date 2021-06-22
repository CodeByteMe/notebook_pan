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

# termux安装code-server
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