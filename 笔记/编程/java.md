# 使用poi读取excel文件
* 引入的jar包

```xml
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>3.17</version>
</dependency>
```

* 基本调用代码

```java
package com.jd.jrmserver.base.test;

import org.apache.poi.hssf.usermodel.*;

import java.io.FileInputStream;
import java.io.InputStream;
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

/**
 * 读取上篇中的xls文件的内容，并打印出来
 *
 * @author Administrator
 */
public class ExcelTest {

    /**
     * 读取一个excel文件的内容
     *
     * @param args
     * @throws Exception
     */
    public static void main(String[] args) throws Exception {
        //extractor();
        readTable();
    }


    //通过对单元格遍历的形式来获取信息 ，这里要判断单元格的类型才可以取出值
    public static void readTable() throws Exception {
        InputStream ips = new FileInputStream("C:\\Users\\liubin52\\Desktop\\test.xls");
        HSSFWorkbook wb = new HSSFWorkbook(ips);
        HSSFSheet sheet = wb.getSheetAt(0);
        for (Iterator ite = sheet.rowIterator(); ite.hasNext(); ) {
            HSSFRow row = (HSSFRow) ite.next();
            System.out.println();
            for (Iterator itet = row.cellIterator(); itet.hasNext(); ) {
                HSSFCell cell = (HSSFCell) itet.next();
                switch (cell.getCellType()) {
                    case HSSFCell.CELL_TYPE_BOOLEAN:
                        //得到Boolean对象的方法
                        System.out.print(cell.getBooleanCellValue() + " ");
                        break;
                    case HSSFCell.CELL_TYPE_NUMERIC:
                        //先看是否是日期格式
                        if (HSSFDateUtil.isCellDateFormatted(cell)) {
                            //读取日期格式
                            System.out.print(cell.getDateCellValue() + " ");
                        } else {
                            //读取数字
                            System.out.print(cell.getNumericCellValue() + " ");
                        }
                        break;
                    case HSSFCell.CELL_TYPE_FORMULA:
                        //读取公式
                        System.out.print(cell.getCellFormula() + " ");
                        break;
                    case HSSFCell.CELL_TYPE_STRING:
                        //读取String
                        System.out.print(cell.getRichStringCellValue().toString() + " ");
                        break;
                }
            }
        }
    }

}
```

> 如果代码老是报错,那么把`xls`重新保存一下可能会解决问题
> 不是`xls`的文件可以保存成`xls`的文件

[参考](https://blog.csdn.net/xichengqc/article/details/90443773)

# java运行springboot的jar包

```bash
# nohup保证后台运行,后边需要加上&
# 操作系统常用输出流:
# 0：标准输入流 stdin
# 1：标准输出流 stdout
# 2：标准错误流 stderr
# > output,实际是 1 > output的省略用法
# < input,实际是 0 < input 的省略用法
# 2>&1是将标准错误流也写到标准输出流中

nohup java -jar app.jar >output 2>&1 &
```

[参考](https://www.jianshu.com/p/563497a6e1a7)

# jar包常用网站
* [maven的jar包查询](https://mvnrepository.com/)
* [类名找jar包](https://www.findjar.com/)

# .classpath的常用理解
`.classpath.xml`文件用于记录项目编译环境的所有信息,包括  
* 源文件路径
* 编译后`class`文件存放路径
* 依赖`jar`包路径
* 运行的容器信息
* 依赖的外部`project`信息

删除该文件java项目将无法被识别  
基本标签  
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html>
<!--根结点-->
<classpath>
    <!--
    每一个classpathentry是一个说明信息,
    kind属性指明类型
    path指明路径
    -->
    <!--这个路径表示源文件,和.classpath在同一路径-->
    <classpathentry kind="src" path="src"/>
    <!--这个路径表示静态资源文件,和.classpath在同一路径-->
    <classpathentry kind="src" path="resources"/>
    <!--combineaccessrules为false表示引入外部项目.这里/mybatis是工作空间下的该文件夹-->
    <classpathentry combineaccessrules="false" kind="src" path="/mybatis"/>
    <!--指定JRE容器的位置,这里使用自定义JRE.如果使用默认jre:path="org.eclipse.jdt.launching.JRE_CONTAINER"-->
    <classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/JavaSE-1.7">
        <attributes>
            <!--owner.project.facets的属性值,指定的是使用模板名称-->
            <attribute name="owner.project.facets" value="java"/>
        </attributes>
    </classpathentry>
    <!--用于指定tomcat容器位置-->
    <classpathentry kind="con" path="org.eclipse.jst.server.core.container/org.eclipse.jst.server.tomcat.runtimeTarget/学习 8080">
        <attributes>
            <!--owner.project.facets的属性值,指定的是使用模板名称-->
            <attribute name="owner.project.facets" value="jst.web"/>
        </attributes>
    </classpathentry>
    <!--Web App Libraries-->
    <classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.web.container"/>
    <!--User Library配置-->
    <classpathentry kind="con" path="org.eclipse.jst.j2ee.internal.module.container"/>
    <!--项目引入的依赖jar-->
    <classpathentry kind="lib" path="WebContent/WEB-INF/lib/commons-dbcp-1.2.1.jar"/>
    <!--exported="true",这样在其他项目中使用该jar包的东西时也能查看该 jar包中的依赖关系-->
    <classpathentry exported="true" kind="lib" path="dubbo_lib/dubbo-2.5.3.jar"/>
    <!--表示编译后的class存放路径-->
    <classpathentry kind="output" path="WebContent/WEB-INF/classes"/>
</classpath>
```
classpathentry的填入顺序一般是`src`,`con`, `lib`, `output`  
```yaml
kind的各种值:
    src: 表示该类型为源文件,其指定的路径是相对于.classpath路径.
    output: java源文件编译后的class文件存放路径
    con: 程序的运行环境(容器)
    lib: 指定项目引用的依赖jar包
```

[参考_1](https://blog.csdn.net/m0_37664906/article/details/78274143)  
[参考_2](https://www.cnblogs.com/himonkey/p/11188642.html)


# build.xml的相关知识
`build.xml`是`ANT`构建和部署`Java`项目的工具,比较老了.  
文件基本结构  
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
常用命令  
```
ant sayHelloWorld                               默认找寻当前目录下的build.xml,然后执行sayHelloWorld任务
ant -f hello.xml sayHelloWorld                  执行hello.xml文件下的sayHelloWorld任务
ant -projecthelp                                列出build.xml中所有target的名称
```
案例解释
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

ant构建Java工程
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

ant构建war工程
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

# velocity模板
velocity用于分离`持久化层`和`业务层`  
用`#`开头的是关键字  
用`$`开头的是变量

[参考](https://blog.csdn.net/nature_fly088/article/details/52497779)  