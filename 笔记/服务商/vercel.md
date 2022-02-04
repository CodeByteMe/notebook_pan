# vercel反代网站
1. 注册[vercel](https://vercel.com)
2. 安装nodejs
3. 安装Vercel命令行工具
```bash
npm i vercel
```

4. vercel登陆
```bash
# vercel的运行文件是/node_modules/vercel/dist/index.js
# 使用邮箱验证进行登陆
vercel login
```

5. 创建反代配置文件`goo.json`
```bash
{
    "version": 2,
    "routes": [
        {
            "src": "/(.*)",
            "dest": "https://www.google.com/$1"
        }
    ]
}
```

6. 部署配置文件
```bash
# 部署后命令行会提供部署后的域名
# 自定义域名的话可以在项目网站的view domains进行修改
vercel -A goo.json --prod
# 添加新的域名也可以用命令行
vercel domains add goo.vercel.app
```
[参考](https://haoduck.com/626.html)
