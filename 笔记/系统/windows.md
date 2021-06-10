# win10优化设置
* Win10专业版关闭Windows Defender
> WIN+R→gpedit.msc→计算机配置→管理模板→Windows组件→双击Windows Defender→双击关闭Windows Defender→选择启用→确定
* Win10家庭版关闭Windows Defender
> WIN+R→regedit→HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender
> 在右侧点右键，选择新建—DWORD（32位）值→将新值命名为DisableAntiSpyware，并双击赋值为1
* win10禁用自动更新
> WIN+R→services.msc→Windows Update→禁用。或者工具栏右键选择"任务管理器→打开服务→Windows Update→禁用
* win10关闭家庭组
> 工具栏右键选择"任务管理器→打开服务→HomeGroup Listener和HomeGroup Provider将其关闭
* 关闭Windows Search
> 工具栏右键选择"任务管理器→打开服务→Windows Search→禁用
* 关闭IPV6
> 工具栏右键选择"任务管理器→打开服务→IPHelper→禁用
* 设置自动登录
> Win+R输入“netplwiz”回车，然后选中当前正在使用的账号，取消“要使用本计算机，用户必须输入用户名和密码”前面的复选框。
> 确定后系统会再次确认一遍密码，再重启计算机时，就可以越过登录界面直接进入桌面了；
* 开启快速启动
> 点击“开始”菜单→“设置”→“系统（显示、通知、电源）”→“电源和睡眠”→“其他电源设置”
> 在弹出面板中点击“选择电源按钮的功能”→“更改当前不可用的设置”，勾选“启用快速启动”就可以了；
> （请先开启休眠：Win+R输入powercfg–h on，关闭休眠：powercfg –h off(关闭后C盘会空出几G的空间)）
* 用360优化win10后开不了机的问题原因是禁用了三个服务
> 在360应用软件服务里dmwappushsvc、diagnsticsTrackingservice、coreMessaging这三个要开启服务。
> WINDOWS FONT CACHE SERVICE 服务此项不能优化，优化后开机要1分钟
* 设置好Superfetch服务
> 服务Superfetch启动类型改成延迟启动或禁用，可以避免Win10对硬盘的频繁访问。
* 关闭性能特效(配置不好的机器可以关闭)
> 系统属性>高级>性能>设置>关闭淡出淡入效果。打开文件夹，小软件很快的
* 关闭计划任务里的隐藏的自启动程序
> 控制面板→所有控制面板项→管理工具→任务计划程序→任务计划程序库→右侧任务列表→禁用不需要的任务。
* 关闭windows安全中心服务
> Win+R键 输入services.msc回车——找到SecurityCenter ，设置禁用。
* 关闭程序兼容助手服务
> 服务→Program CompatibilityAssistant Service服务 禁用。
* 代理自动发现服务
> 打开一个网页，找到工具，然后是Internet选项，连接，局域网设置，自动检测设置的勾去掉
* 关闭onedrive，上传文件
> Win+R键输入 gpedit.msc在windows组件里，onedrive有三个选项关闭

# CMD添加代理
* SSR设置

```
 SSR客户端右键 -> 选项设置 -> 本地代理（勾选允许来自局域网的连接） 
```

* CMD添加代理

```bash
set http_proxy=127.0.0.1:1080
set https_proxy=127.0.0.1:1080
```

