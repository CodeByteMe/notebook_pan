# 创建带参数的构造函数
* 报错信息

```
***************************

APPLICATION FAILED TO START
***************************


Description:
Parameter 0 of constructor in com.deng.service.impl.ImageServiceImpl required a bean of type 'java.lang.String' that could not be found.

Action:
Consider defining a bean of type 'java.lang.String' in your configuration.
```

* 创建带参数构造函数方法
  * 创建带参数的`Bean`,然后`@Autowired`该`Bean`即可

```java
@Configuration
public class BeanConfig {
    @Bean
    ImageService imageService(){
        return new ImageServiceImpl("test");
    }
}
```

[参考](https://blog.csdn.net/egg1996911/article/details/80398847)

# 异步任务
主方法添加`@EnableAsync`注解
```java
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;

@SpringBootApplication
@EnableAsync
public class Application{
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
异步任务执行类添加`@Async`注解
```java
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Component;

@Component
public class DataExecutor {
    //@Async 添加到类上表示该类的方法都是异步方法.也可以添加到方法上.value放线程池Bean的名字
    @Async(value = "publishExecutor")
    public void executeSth() {
      
    }
}
```
异步线程池配置
```java
import java.util.concurrent.Executor;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
public class PublishAsynExecutor {
    // 核心线程数（默认线程数）线程池创建时候初始化的线程数:10
    @Value("${tk.pe.corePoolSize}")
    private int corePoolSize;
    // 最大线程数 线程池最大的线程数，只有在缓冲队列满了之后才会申请超过核心线程数的线程:200
    @Value("${tk.pe.maxPoolSize}")
    private int maxPoolSize;
    // 缓冲队列数 用来缓冲执行任务的队列:100
    @Value("${tk.pe.queueCapacity}")
    private int queueCapacity;
    // 允许线程空闲时间（单位：默认为秒）当超过了核心线程之外的线程在空闲时间到达之后会被销毁:300
    @Value("${tk.pe.keepAliveSeconds}")
    private int keepAliveSeconds;
    // 线程池名前缀 方便我们定位处理任务所在的线程池
    @Value("${tk.pe.threadNamePrefix}")
    private String threadNamePrefix;

    @Bean
    public Executor publishExecutor() {
        ThreadPoolTaskExecutor publishExecutor = new ThreadPoolTaskExecutor();
        publishExecutor.setCorePoolSize(corePoolSize);
        publishExecutor.setMaxPoolSize(maxPoolSize);
        publishExecutor.setQueueCapacity(queueCapacity);
        publishExecutor.setKeepAliveSeconds(keepAliveSeconds);
        publishExecutor.setThreadNamePrefix(threadNamePrefix);
        publishExecutor.initialize();

        return publishExecutor;
    }
}
```
调用时调用异步任务的方法即可.  
> 如下方式会使`@Async`失效  
> 1. 异步方法使用`static`修饰
> 2. 异步类没有使用`@Component`注解（或其他注解）导致spring无法扫描到异步类
> 3. 异步方法不能与调用异步方法的方法在同一个类中
> 4. 类中需要使用`@Autowired`或`@Resource`等注解自动注入，不能自己手动`new`对象
> 5. 如果使用SpringBoot框架必须在启动类中增加`@EnableAsync`注解
> 6. 在`Async方法`上标注`@Transactional`是没用的.在`Async方法`调用的方法上标注`@Transactional`有效。

[参考](https://www.cnblogs.com/fujingtao5470/p/12666747.html)

# 定时任务
## 普通定时任务创建
主方法添加`@EnableScheduling`注解
```java
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableScheduling;

@SpringBootApplication
@EnableScheduling
public class Application{
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
添加定时任务
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.Scheduled;

@Configuration
public class ScheduleTask{
    /*
     * fixedDelay:方法执行间隔。上次未执行完，下一次不执行。执行完后间隔指定时间再次执行
     * fixedRate:方法执行间隔。上次未执行完，下一次不执行。记录阻塞过程中应该执行的次数。不阻塞时一口气执行完，再按原时间执行。
     * corn:可以定制执行任务时间
     * initialDelay:启动时延迟指定时间
     * @Scheduled(initialDelay = 10000,fixedRate = 15000)
     */
    @Scheduled(fixedDelay = 180000)
    public static void myTask() {
        
    }
}
```
如果执行的时间间隔需要从配置文件读取(时间需要使用[corn表达式](#corn表达式))
```java
import java.util.Date;

import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.Trigger;
import org.springframework.scheduling.TriggerContext;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.SchedulingConfigurer;
import org.springframework.scheduling.config.ScheduledTaskRegistrar;
import org.springframework.scheduling.support.CronTrigger;

@Configuration
@EnableScheduling
public class ScheduleTask implements SchedulingConfigurer {
    @Override
    public void configureTasks(ScheduledTaskRegistrar scheduledTaskRegistrar) {
        String timeStr = "配置文件的时间获取";
        Runnable task = new Runnable() {
            @Override
            public void run() {
                // 任务代码
            }
        };

        Trigger trigger = new Trigger() {
            @Override
            public Date nextExecutionTime(TriggerContext triggerContext) {
                CronTrigger cronTrigger = new CronTrigger(timeStr);
                return cronTrigger.nextExecutionTime(triggerContext);
            }
        };

        scheduledTaskRegistrar.addTriggerTask(task, trigger);
    }
    
}
```
[参考](https://blog.csdn.net/qq_34125349/article/details/77430956)

# corn表达式

```
每隔5秒执行一次：*/5 * * * * ?
每隔1分钟执行一次：0 */1 * * * ?
每天23点执行一次：0 0 23 * * ?
每月1号凌晨1点执行一次：0 0 1 1 * ?
每月最后一天23点执行一次：0 0 23 L * ?
每周星期天凌晨1点实行一次：0 0 1 ? * L
在26分、29分、33分执行一次：0 26,29,33 * * * ?
每天的0点、13点、18点、21点都执行一次：0 0 0,13,18,21 * * ?
```
[参考](https://www.cnblogs.com/junrong624/p/4239517.html)

# springboot动态编译class
可以[参考](#springboot的war包动态编译无法找到相关依赖jar或调用类), 这里边参考了[这篇](https://blog.csdn.net/zhao_xinhu/article/details/82499062)博客

# springboot的war包动态编译无法找到相关依赖jar或调用类
在测试动态编译的时候,本地可能因为有maven仓库, 不会有依赖jar或调用类找不到的情况  
但部署到服务器, 由于动态编译的时候没有显式的指明依赖的jar包路径, 所以可能出现调用的jar包找不到的情况  
可参考如下进行操作  
```java
import java.io.File;
import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.StringJoiner;

import javax.tools.JavaCompiler;
import javax.tools.JavaCompiler.CompilationTask;
import javax.tools.JavaFileObject;
import javax.tools.StandardJavaFileManager;
import javax.tools.SimpleJavaFileObject;
import javax.tools.ToolProvider;

private void codeCompiler(String className, String classCode) throws URISyntaxException, IOException {
    String dest = this.getClass().getProtectionDomain().getCodeSource().getLocation().getPath();
    JavaCompiler compiler = ToolProvider.getSystemJavaCompiler();
    StandardJavaFileManager standardFileManager = compiler.getStandardFileManager(null,null,null);
    StringObject stringObject = new StringObject(new URI(className + ".java"), JavaFileObject.Kind.SOURCE, 
        classCode);
    List<String> options = new ArrayList<String>();
    options.add("-d");
    options.add(dest);
    // 加-g是因为动态编译的时候会有错误, 加上就没有了
    options.add("-g");
    // 在动态编译的时候加入classpath参数
    options.add("-classpath");
    options.add(getClasspath(dest));
    CompilationTask task = compiler.getTask(null, standardFileManager, null, options, null, 
        Arrays.asList(stringObject));
    task.call();
    standardFileManager.close();
}
private static class StringObject extends SimpleJavaFileObject{

    private String content;

    public StringObject(URI uri, Kind kind, String content) {
        super(uri, kind);
        this.content = content;
    }

    @Override
    public CharSequence getCharContent(boolean ignoreEncodingErrors) throws IOException {
        return this.content;
    }
}
private String getClasspath(String dest) {
    // 这里我编译成的war包后, 在lib和lib-provided都有jar, 故加入了两个路径下的jar
    String libProvidedPath = dest.replaceAll("classes", "lib-provided");
    String libPath = dest.replaceAll("classes", "lib");
    String pathSeparator = File.pathSeparator;
    StringJoiner classpathStr = new StringJoiner(pathSeparator);
    classpathStr.add(getAllFilePathInCurrentDir(libProvidedPath))
        .add(getAllFilePathInCurrentDir(libPath))
        .add(dest);

    return classpathStr.toString();
}
private String getAllFilePathInCurrentDir(String dirStr) {
    String pathSeparator = File.pathSeparator;
    StringJoiner filePathStr = new StringJoiner(pathSeparator);
    File dir  = new File(dirStr);
    if (dir.isDirectory()) {
        File[] allFiles = dir.listFiles();
        for (File file: allFiles) {
            filePathStr.add(file.getAbsolutePath());
        }
    }

    return filePathStr.toString();
}
```
[参考1](https://www.cnblogs.com/songxingzhu/p/8980409.html)
[参考2](https://www.cnblogs.com/songxingzhu/p/8980409.html)

# springboot启动完成后执行某个代码
通过实现`ApplicationRunner`接口  
```java
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

@Component
public class ApplicationRunnerImpl implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("通过实现ApplicationRunner接口，在spring boot项目启动后打印参数");
        String[] sourceArgs = args.getSourceArgs();
        for (String arg : sourceArgs) {
            System.out.print(arg + " ");
        }
        System.out.println();
    }
}
```
或者可以实现`CommandLineRunner`接口
```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

@Component
public class CommandLineRunnerImpl implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("通过实现CommandLineRunner接口，在spring boot项目启动后打印参数");
        for (String arg : args) {
            System.out.print(arg + " ");
        }
        System.out.println();
    }
}
```
区别在于`run()`方法的参数类型不同  
[参考](https://www.cnblogs.com/zuidongfeng/p/9926471.html)

# 获取springboot的jar包中的jar方法
依赖jar包
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-loader</artifactId>
  <version>2.2.4.RELEASE</version>
</dependency>
```
```java
public void testJarLaunch() throws IOException, ClassNotFoundException {
    String path = "jar:file:/IdeaProjects/demo-application/target/demo-application-1.0-SNAPSHOT.jar!";
    URL url = new URL(path);
    Handler handler = new Handler();
    org.springframework.boot.loader.jar.JarFile root = handler.getRootJarFileFromUrl(url);

    JarFileArchive archive = new JarFileArchive(root);
    //灵魂方法getNestedArchives获取嵌套的jar等文件，参数是个EntryFilter，过滤条件
    List<Archive> list = archive.getNestedArchives(entry -> {
        System.out.println(entry.getName());
        return entry.getName().endsWith("BOOT-INF/lib/app-core-1.0-SNAPSHOT.jar");
    });

    //list里就是每个jar了
    for (Archive entries : list) {
        //获取jar里的内容
        Iterator<Archive.Entry> it = entries.iterator();
        while (it.hasNext()){
            Archive.Entry entry = it.next();
            String name = entry.getName();
            System.out.println(name);
        }
    }
}
```
[参考](https://blog.csdn.net/qq_35752021/article/details/107302766)

# springboot的异常邮件提醒
依赖jar包
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```
```properties
spring.mail.host=smtp.qq.com
spring.mail.username=发送邮箱
spring.mail.password=SMTP授权码
spring.mail.default-encoding=UTF-8
```
```java
import org.springframework.mail.SimpleMailMessage;
import org.springframework.mail.javamail.JavaMailSender;

public class Test {
    @Autowired
    private JavaMailSender javaMailSender;

    public void sendEmail() {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setFrom("from@qq.com");
        message.setTo("to@qq.com");
        message.setSubject("异常通知");
        message.setText("异常");
        javaMailSender.send(message);
    }
}
```
[参考](https://blog.csdn.net/qq_33591903/article/details/100540629)

# springboot自动重启
## 使用DevTools重启
在`pom.xml`添加如下依赖jar, 是springboot的开发者工具
```xml
<dependencys>
    <dependency>
        <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
    </dependency>
</dependencys> 
```
`classpath`的任何更新都会触发程序启动, 但会排除下边的目录  
1. /META-INF/resources
2. /resources
3. /static
4. /public
5. /templates

常用配置文件属性
```properties
# 用来覆盖默认的重启排除目录
spring.devtools.restart.exclude=WEB-INF/**
# 设置重启的目录, 可不加
# spring.devtools.restart.additional-paths: src/main/java
# 关闭自动重启
spring.devtools.restart.enable=false
# 关闭自动重启后, 可设置触发文件, 当有了某些文件的更新, 并更新了该触发文件后, 则可以触发重启
# 触发文件可以起名为restart-trigger.properties
spring.devtools.restart.trigger-file=restart-trigger.properties
```
当生成war包后放到tomcat无法自动重启可[参考](tomcat.md#springboot开启自启动工具, tomcat无法自启动)
[参考](https://blog.csdn.net/weixin_39330443/article/details/81739271)

## 使用代码重启
使用代码重启的时候, 需要把热部署(自启动)的pom中的jar注释掉  
某些情况下, 如在tomcat, 可能下边的`context`仍旧会有空指针异常
```java
// 在启动类添加restart()方法
public class MyApplication {
    private static String[] args;
	private static ConfigurableApplicationContext context;

	public static void main(String[] args){
		MyApplication.args = args;
		MyApplication.context = SpringApplication.run(MyApplication.class, args);
    }
	// 用于重启项目
    public static void restart() {
        context.close();
        MyApplication.context = SpringApplication.run(MyApplication.class, args);
    
    }
}

// 编写一个方法, 用于调用启动类中封装的restart()
public void startProject () {
    Thread restartThread = new Thread(new Runnable() {
        @Override
        public void run() {
            try {
                Thread.sleep(1000);
                MyApplication.restart();
            } catch (InterruptedException ignored) {}
        }
    });
    restartThread.setDaemon(false);
    restartThread.start();
}
```
[参考](https://blog.csdn.net/weixin_44695125/article/details/108059877)

# 利用反射动态修改注解值
[参考](https://blog.csdn.net/u014750606/article/details/79977114)