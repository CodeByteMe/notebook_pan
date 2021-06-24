# git常用操作
## 常用流程
```bash
# 新建一个文件夹, 然后在该文件夹内初始化一个git仓库
git init
# 创建.gitignore来标注不需要提交的文件路径
vi .gitignore
# 添加工作文件或文件夹到暂存区(临时保存提交内容)
git add README.md mydir
# 也可以添加所有除.gitignore中路径的文件到暂存区
git add *
# 提交改动到head(确定已经提交的内容)
git commit -m "test"
# 连接到远端服务器
git remote add origin https://github.com/Easul/notebook.git
# 推送提交到head的改动到master分支
git push origin master
```

## 设置用户信息
```bash
# 需要设置邮箱和用户名, 去掉--global参数将只添加该用户信息到该仓库
git config --global user.email "you@example.com"
git config --global user.name "YourName"
```

## 查看本repo连接的仓库连接
```bash
git remote show origin
```

## 提交文件到暂存区的不同命令
```bash
# 提交除.gitignore外所有文件
git add *
# 提交所有更新文件
git add .
# 查看所有文件状态
git status
```

## 文件推送的不同方式
```bash
# 带-u的参数, 下次推送可以直接git push就推到master了
git push -u origin master 
```

## 文件的获取方式
```bash
# 克隆远程仓库到本地仓库
git clone [url] 
```

## git设置代理
```bash
# 设置socks5, 取消--global将只添加代理到本仓库
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080' 

# 设置http代理, 取消--global将只添加代理到本仓库
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080

# 取消添加的代理, 取消--global将只添加代理到本仓库
git config --global --unset http.proxy
git config --global --unset https.proxy
```

## git查看配置
```bash
# git查看系统配置
git config --system --list
# git查看当前用户配置
git config --global --list
# git查看当前仓库配置
git config --local --list
```

## git版本回退
```bash
# 从近到远显示提交日志, 
# 加上--pretty=oneline可以只显示commit id 和 commit message, 会整齐一些
git log
# 回退到某个commit message的版本, 需要加上commit id回退
git reset --hard commitId
# 然后即可推送旧版本
# 如果想要再推送回退版本后的新版本, 可以如下找到commit id
git reflog
# 然后设为新版本即可
git reset --hard commitId
```
[参考](https://www.jianshu.com/p/3020740561a8)

# 其他操作
## github获取token
1. 登录后, 点击头像, 进入`settings`
2. 进入`Developer settings`, `Personal access tokens`, `Generate new token`
3. 为token起一个名, 然后选择token的权限范围(可以全选), 然后`Generate token`即可

## github使用ssh登录
```bash
# 进入~/.ssh/下是否有authorized_keys, id_rsa, id_rsa.pub, 没有则使用如下命令生成ssh公私钥
cd ~/./ssh
# email换成自己的地址, 中间一路回车即可
ssh-keygen -t rsa -C "email@email.com"
# 查看并复制公钥
cat id_rsa.pub
# 登录github, 点击头像-> Settings-> SSH and GPG keys-> New SSH key
# title任意起名, key复制进取刚刚复制的公钥
# 然后保存, 使用如下命令检测是否操作成功
ssh git@github.com
# 出现如下提示则成功
# Hi xxx! You've successfully authenticated, but GitHub does not # provide shell access. 
# Connection to github.com closed.
# 然后修改推送remote
# git remote set-url origin git@github.com:github用户名/github仓库名.git
git remote set-url origin git@github.com:user/repo.git
# 然后就可以提交了
# 若提示如下, 则可以等下再推送, 即可成功推送
#   kex_exchange_identification: Connection closed by remote host
#   Connection closed by 13.250.177.223 port 22
#   fatal: Could not read from remote repository.
#   
#   Please make sure you have the correct access rights
#   and the repository exists.
```

# 常见报错
##  ! [rejected] master -> master (fetch first)
```bash
# 原因: 本地仓库和远程仓库版本不一致
# 1. 先拉取远程仓库, 再推送(可能覆盖本地某些文件)
git pull origin master
# 2. 先将远程仓库与本地同步, 再重新add, commit, push
git pull origin master --allow-unrelated-histories
# 3. 直接覆盖推送, 如果有他人推送的记录则直接覆盖掉
git push -f origin master
# 4. 先把远程fetch到本地然后merge后再push
git fetch
git merge
```