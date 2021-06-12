# 防火墙处理
```bash
# 关闭
systemctl stop firewalld
# 开启
systemctl start firewalld
# 重启
systemctl restart firewalld
# 查看防火墙状态
systemctl status firewalld
# 在开机时启用防火墙
systemctl enable firewalld
# 在开机时禁用防火墙
systemctl disable firewalld
```

# 端口处理
##　开放端口
```bash
# 永久开放5672端口
firewall-cmd --zone=public --add-port=5672/tcp --permanent
# 永久关闭5672端口
firewall-cmd --zone=public --remove-port=5672/tcp --permanent
# 配置立即生效
firewall-cmd --reload   
```
## 查看
```bash
# 获取所有开放端口
firewall-cmd --zone=public --list-ports
```
[参考](https://www.cnblogs.com/heqiuyong/p/10460150.html)

# 进程处理
```bash
# 查看当前运行进程信息
netstat -lntp
# 查看端口被那个进程占用
netstat -lntp | grep 8080
# 查看某进程详细信息
ps pid
```