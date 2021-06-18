# 环境配置
## 用户配置
```json
{
    "workbench.colorTheme": "Default Light+",
    "workbench.activityBar.visible": false,
    "breadcrumbs.enabled": false,
    "update.mode": "none",
    "workbench.startupEditor": "newUntitledFile",
    "editor.minimap.enabled": false,
    "java.home": "/opt/apps/jdk-11.0.7",
    "maven.executable.path": "/opt/apps/maven-3.6.3/bin/mvn",
    "java.configuration.maven.userSettings": "/opt/apps/maven-3.6.3/conf/settings.xml",
    "maven.terminal.customEnv": [
        {
            "environmentVariable": "JAVA_HOME",
            "value": "/opt/apps/jdk-11.0.7"
        }
    ],
    "explorer.confirmDelete": false,
    "java.jdt.ls.vmargs": "-XX:+UseParallelGC -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -Dsun.zip.disableMemoryMapping=true -Xmx1G -Xms100m -javaagent:\"/home/easul/.vscode-insiders/extensions/gabrielbb.vscode-lombok-1.0.1/server/lombok.jar\"",
    "java.project.importOnFirstTimeStartup": "automatic",
    "files.exclude": {
        "**/.classpath": true,
        "**/.project": true,
        "**/.settings": true,
        "**/.factorypath": true
    },
    "git.ignoreLegacyWarning": true,
    "window.zoomLevel": 0,
    "java.refactor.renameFromFileExplorer": "autoApply",
    "[json]": {
        "editor.defaultFormatter": "HookyQR.beautify"
    },
}
```

## 常用插件
```
Auto Close Tag
Auto Rename Tag
Beautify
Bracket Pair Colorizer
C#
Language
Cloudfoundry Manifest YML Support
Concourse CI Pipeline Editor
Debugger for Java
ESLint
HTML CSS Support
HTML Snippets
Java Extension Pack
Java Test Runner
JavaScript (ES6) code snippets
jQuery Code Snippets
Language support for Java(TM) by Red Hat
Lombok Annotations Support for VS Code
Markdown All in One
Markdown Preview Enhanced
Markdown Preview Github Styling
Maven for Java
NuGet Package Manager
Project Manager for Java
Remote - SSH
Remote -SSH: Editing Configuration Files
Spring Boot Dashboard
Spring Boot Extension Pack
Spring Boot Support
Spring Boot Tools
Spring Initializr Java Support
Tomcat for Java
Visual Studio IntelliCode
vscode-pdf
XML
```

## 代码片段
代码片段(文件->首选项->用户片段)
* HTML
```json
{
	// Place your snippets for html here. Each snippet is defined under a snippet name and has a prefix, body and 
	// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
	// same ids are connected.
	// Example:
	// "Print to console": {
	// 	"prefix": "log",
	// 	"body": [
	// 		"console.log('$1');",
	// 		"$2"
	// 	],
	// 	"description": "Log output to console"
	// }
	"init html": {
		"prefix": "htmlInit",
		"body": [
			"<!DOCTYPE html>",
			"<html>",
			"    <head>",
			"        <meta charset=\"UTF-8\">",
			"        <link rel=\"stylesheet\" href=\"\"/>",
			"        <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",
			"    </head>",
			"    <body>",
			"        <script src=\"\"></script>",
			"    </body>",
			"</html>"
		],
		"description": "html init"
	}
}
```

* java
```json
{
	// Place your snippets for java here. Each snippet is defined under a snippet name and has a prefix, body and 
	// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
	// same ids are connected.
	// Example:
	// "Print to console": {
	// 	"prefix": "log",
	// 	"body": [
	// 		"console.log('$1');",
	// 		"$2"
	// 	],
	// 	"description": "Log output to console"
	// }
	"sout": {
		"prefix": "sout",
		"body": [
			"System.out.println($1);"
		],
		"description": "get print code"
	},
	"psvm": {
		"prefix": "psvm",
		"body": [
			"public static void main(String[] args) {",
			"    $1",
			"}"
		],
		"description": "get main code"
	},
	"project": {
		"prefix": "project",
		"body": [
			"/**",
			" * @Author: Easul Wang",
			" * @Date: ${CURRENT_YEAR}-${CURRENT_MONTH}-${CURRENT_DATE} ${CURRENT_HOUR}:${CURRENT_MINUTE}:${CURRENT_SECOND}",
			" * @Last Modified by: DALIU",
			" * @Last Modified time: 2020-09-11 11:18:59",
			" */"
		],
		"description": "author informations"
	}
}
```

## [代理](https://stackoverflow.com/questions/57289351/is-it-possible-to-create-a-proxy-in-remote-ssh-visual-studio-code)配置
```
Host wx
    HostName 192.168.2.1
    User root
    Port 2222
    ProxyCommand nc -X 5 -x 127.0.0.1:1080 %h %p 
```

## 插件文档
* [Markdown Preview Enhanced](https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/)  
* [i5ting_ztree_toc](http://i5ting.github.io/i5ting_ztree_toc/)   

# 常见问题
## vscode运行使用其他JRE
新建`.vscode`文件夹,创建`settings.json`文件,然后配置运行时环境  
```json
{
    "java.configuration.runtimes": [
        {
          "name": "JavaSE-1.7",
          "path": "/opt/apps/jdk/jdk-7",
        }
      ]
}
```

## vscode更新后无法远程remotessh

这里只部分错误
```
events.js:292 throw er； //Unhandled ‘error‘ event；Error:spawn cmd ENOENT.
Failed to parse remote port from server output
```
这可能是因为更新了vscode之后,远端服务器上的remotessh相关组件依旧比较老,所以两边不配套  
再装回原来的版本即可  
[参考](https://blog.csdn.net/qq_36763031/article/details/115526981)  

## vscode添加markdown用户片段不生效
需要在`settings.json`中激活如下配置, 如果是自定义格式, 则可以将`[markdown]`换为自定义格式, 如`.todo`格式可以写`[todo]`
```json
{
	"[markdown]": {
        "editor.quickSuggestions": true
    }
}
```