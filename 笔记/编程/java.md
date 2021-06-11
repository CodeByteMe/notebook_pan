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