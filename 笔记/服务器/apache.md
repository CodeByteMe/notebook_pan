# 更改apache相关配置
* 文件根目录

找到文件
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

找到文件
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
[参考](https://blog.csdn.net/chen200910a/article/details/22089267)