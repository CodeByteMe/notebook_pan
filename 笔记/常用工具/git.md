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

# 其他操作
## github获取token
1. 登录后, 点击头像, 进入`settings`
2. 进入`Developer settings`, `Personal access tokens`, `Generate new token`
3. 为token起一个名, 然后选择token的权限范围(可以全选), 然后`Generate token`即可

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
