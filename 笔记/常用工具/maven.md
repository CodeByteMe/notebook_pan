# maven打包时包含更多资源文件
maven只会打包`resources`下的资源文件  
对于`src\main\java`下的除`.java`文件是不打包的(maven认为这个路径下只应该有java源码)  
如果需要打包更多的文件, 则可以在`pom.xml`的`build`标签中加入下边的标签, 用于同时打包某些路径下的其他资源文件
```xml
<build>
    <resources>  
        <resource>  
            <directory>src/main/resources</directory>  
            <includes>  
                <include>**/*.properties</include>  
                <include>**/*.xml</include>  
            </includes>  
            <filtering>false</filtering>  
        </resource>  
        <resource>  
            <directory>src/main/java</directory>  
            <includes>  
                <include>**/*.mdc</include>  
            </includes>  
            <filtering>false</filtering>  
        </resource>  
    </resources>  
</build>
```
[参考](https://www.cnblogs.com/sos-blue/p/5011279.html)里也提供了加入jar包的方法, 显然这样更方便