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
主方法添加`@EnableAsync`注解
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

# springboot项目部署到tomcat, 动态编译无法找到相关依赖jar或调用类
在测试动态编译的时候,本地可能因为有maven仓库, 不会有依赖jar或调用类找不到的情况  
但部署到服务器, 由于动态编译的时候没有显式的指明依赖的jar包路径, 所以可能出现调用的jar包找不到的情况  
可参考如下进行操作  
```java

```