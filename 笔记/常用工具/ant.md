# build.xml的相关知识
`build.xml`是`ANT`构建和部署`Java`项目的工具,比较老了.  

## 文件基本结构  
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xml>
<!--
    文件根元素,属性有
        name    指定project元素名称
        default 指定project默认执行时执行的target名称
        basedir 指定基路径位置.没有指定,以build.xml当前所在目录为基路径
-->
<project name="helloWorld">
    <!--
        用于定义ant的参数.可用做任务的属性值(${propertyName})
        如果要引入外部文件,可以使用<property file="build.properties"/>
        ant的内置属性(可以得到与Java的System.getPropertis()一致的属性)
            basedir: project基目录绝对路径
            ant.file: build.xml的绝对路径
            ant.vesion: ant的版本
            ant.project.name: 当前project的名字
            ant.java.vesion: ant检测到的JDK的版本
    -->
    <property name="test" value="1234"/>
    <!--
        是ant的基本执行单元,包含一个或多个具体任务,多个target可以相互依赖.基本属性有:
            name:target元素名称.一个project的target只能有一个唯一的name
            depends:描述target之间的依赖关系.若与多个target存在依赖关系,以","分割,ant会先执行且依次执行依赖target
            if:验证指定属性是否存在,不存在,该target不执行
            unless:验证指定属性是否存在,不存在,该target会被执行
            description:用于描述target.可以写成target的属性,也可以写成target内部的一个标签
    -->
    <target name="sayHelloWorld">
        <!--创建时间戳-->
        <tstamp />
        <!--ant常用任务-->

        <!--复制-->
        <!--复制单文件-->
        <copy file="file.txt" tofile="copy.txt"/>
        <!--复制目录-->
        <copy todir="../newdir/dest_dir">
            <fileset dir="src_dir" />
        </copy>
        <!--复制文件到目录-->
        <copy file = "my.txt" todir="../newdir/dest_dir"/>

        <!--删除-->
        <!--删除文件-->
        <delete file="photo/test.jpg" />
        <!--删除目录-->
        <delete dir="photo/" />
        <!--删除所有备份目录或空目录-->
        <delete includeEmptyDirs="true">
            <!--
                两个星号**/*.bak表示在所有文件夹及其子文件夹下寻找
            -->
            <fileset dir="." includes="**/*.bak"/>
        </delete>

        <!--创建文件夹-->
        <mkdir dir="build"/>

        <!--移动-->
        <!--移动单文件,可用于重命名或移动文件-->
        <move file="fromfile" tofile="tofile"/>
        <!--移动目录-->
        <move todir="../newdir/dest_dir">
            <fileset dir="src_dir" />
        </move>
        <!--移动文件到目录-->
        <move file="fromfile" todir="movedir"/>

        <!--
            输出信息,有message, file, append, level四个属性
        -->
        <echo message="test" file="logs/system.log" append="true" />
    </target> 
    <!--
        用于定义文件的路径,常用属性有
            id: 标识所引用路径集合的名称
    -->
    <path id="myPath"> 
        <!--包含lib文件夹下所有jar结尾的文件-->
        <fileset dir="lib" includes="*.jar">
    </path>
</project>
```
## 常用命令  
```
ant sayHelloWorld                               默认找寻当前目录下的build.xml,然后执行sayHelloWorld任务
ant -f hello.xml sayHelloWorld                  执行hello.xml文件下的sayHelloWorld任务
ant -projecthelp                                列出build.xml中所有target的名称
```
## 案例解释
* 案例1

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xml>
<project name="helloWorld" basedir="/home/deepin/Workspace/3_weixin">
    <target name="targetA" if="ant.java.version">
        <echo message="Java Version:${ant.java.version}"/>
    </target>
    <target name="targetB" depends="targetA" unless="easul">
        <description>
            基本描述
        </description>
        <echo message="基础路径是:${basedir}"/>
    </target>
</project>
```

```bash
ant targetB

Buildfile: /home/deepin/Workspace/3_weixin/ant_test/build.xml

targetA:
     [echo] Java Version:11

targetB:
     [echo] 基础路径是:/home/deepin/Workspace/3_weixin
BUILD SUCCESSFUL
```

> 在这个`build.xml`里边,运行`targetB`,因为他依赖`targetA`,所以先执行`targetA`,`if`判断的是有没有该属性  
> 系统安装了`java`环境,则`ant.java.version`属性存在,也即执行该target,输出了java版本  
> `targetA`执行后,执行`targetB`,`unless`判断`easul`不存在,于是执行`targetB` 

* 案例2

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xml>
<project default="myProperty" name="propertyDemo" basedir="/home">
    <property name = "myTest" value="im created by ant"/>
    <target name="myProperty">
        <echo message="${basedir}" />
        <echo message="${ant.project.name}" />
        <echo message="${myTest}" />
    </target>
</project>
```

```bash
ant

Buildfile: /home/deepin/Workspace/3_weixin/ant_test/build.xml

myProperty:
     [echo] /home
     [echo] propertyDemo
     [echo] im created by ant
BUILD SUCCESSFUL
```

> 通过定义变量`myTest`可以在输出语句中进行变量的使用.  
> 同时也可以输出ant内部支持的变量

## ant构建Java工程
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xml>
<project default="jar" name="jarTest" basedir=".">
    <target name="clean">
        <delete dir = "build/"/>
    </target>
    <target name="compile" depends="clean">
        <mkdir dir="build/classes"/>
        <javac srcdir="src" destdir="build/classes" />
    </target>
    <target name="run" depends = "compile">
        <java classname="HelloWorld">
            <classpath>
                <pathelement path = "build/classes"/>
            </classpath>
        </java>
    </target>
    <target name="jar" depends = "run">
        <jar destfile="helloworld.jar" basedir="build/classes">
            <manifest>
                <attribute name="Main-class" value="HelloWorld"/>
            </manifest>
        </jar>
    </target>
</project>
```
> 这个构建清单默认执行到`jar`.这里从依赖结构的开始进行解释  
> 先执行`clean`,删除`build`路径,里边可能存放有上次已经编译过的文件  
> 然后执行`compile`,创建`build/classes`路径,存放编译的`class`文件,  
> 执行`javac`将`src`路径下的java文件编译到`build/classes`下,   
> 然后执行`run`,通过`java`命令执行`HelloWorld`这个class文件,在其内部标签中指定项目根路径  
> 然后执行`jar`,将所有的`basedir`下的class文件打包成`helloworld.jar`文件,并在jar内制定`mainfest`清单  

## ant构建war工程
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE xml>
<project default="war" name="warTest" basedir=".">
    <property name="classes" value="build/classes" />
    <property name="build" value="build" />
    <property name="lib" value="webroot/WEB-INF/lib" />

    <target name="clean">
        <delete dir = "${build}"/>
    </target>

    <target name="compile" depends="clean">
        <mkdir dir="${classes}"/>
        <javac srcdir="src" destdir="${classes}" />
    </target>
    <target name="war" depends = "compile">
        <war destfile="${build}/webroot.jar" webxml="webroot/WEB-INF/web.xml">
            <!--拷贝webroot下的jsp文件-->
            <fileset dir="webroot" includes="**/*.jsp"/>
            <!--拷贝lib下的jar包-->
            <lib dir="${lib}"/>
            <!--拷贝build/classes下的class文件-->
            <classesdir dir="${classes}"/>
        </war>
    </target>
</project>
```

[参考](https://blog.csdn.net/zdc1990/article/details/6313112)  
[构建工具的进化:ant,maven,gradle](https://zhuanlan.zhihu.com/p/24429133)  