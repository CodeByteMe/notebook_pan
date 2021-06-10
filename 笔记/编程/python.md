# 使用python开启简单的HTTP服务
```bash
cd mydir
# 当前运行 不填写端口 默认8000
python -m SimpleHTTPServer 8888

# 当前终端后台运行
python -m SimpleHTTPServer 8888 &

# 完全后台运行
nohup python -m SimpleHTTPServer 8888 &
```
> SimpleHTTPServer有一个特性，如果待共享的目录下有`index.html`，那么`index.html`文件会被视为默认主页；如果不存在`index.html`文件，那么就会显示整个目录列表。  
> 在哪个目录下启动，就会以当前目录为根目录展示列表

[参考](https://www.cnblogs.com/jakaBlog/p/11394188.html)