# 区分流处理和批处理

* 流处理需要支持低延迟,Exactly-once(仅一次消费)
* 批处理需要支持高吞吐,高效处理

# flink的定义
flink完全支持流处理,看待输入数据流是无界的.批处理看成是特殊的流处理,其输入数据流看成是有界的
# flink组件栈(从下到上)

* delopy(部署层)
    * 部署模式有本地模式,集群模式(Standalone/YARN),云模式(GCE,EC2),常用`YARN`模式
    * `yarn`是脱胎于`Hadoop`的资源管理器,为上层应用提供统一的资源管理和调度
* core(核心层,提供flink的计算实现)
* api(流或批量数据的API)
* libraries(flink封装的满足特定应用的计算框架,分为面向流和面向批处理的)

# flink优势

* 支持高吞吐,低延迟(毫秒级),高性能的流处理
* 支持窗口操作(滑动窗口,滚动窗口,SESSION窗口)
* 支持有状态计算的Exactly-once语义
* 提供DataStream和DataSet的API

# flink基本概念和编程模型

* flink基础构建模块是`流`(streams,表示输入)与`转换`(transformations,对输入的操作)
* 每一个数据流起始于一个或多个`source`,终止于一个或多个`sink`(数据落地)
* 一个`source`可以拆分成多个`source`,用于分开计算
* 时间窗口
    * 流的聚合需要使用`窗口`划定范围(因为流是无界的),窗口可以是`5分钟`,`100个元素`
    * filnk支持`滚动窗口`(窗口之间没有数据重叠),`滑动窗口`(窗口之间有数据重叠),`会话窗口`

# flink分布式运行环境

* flink基于Master-Slave(主从架构)
* flink集群启动,会启动一个`JobManager`和至少一个`TaskManager`
    * `JobManager`
    > 是flink集群的协调者,接收flink job,调度组成job的多个task的执行  
    > 收集Job状态信息,管flink集群的从节点的TaskManager  
    * `TaskManager`
    > 是执行计算的Worker,执行flink job的一组task  
    > 负责管理当前节点的资源信息(内存,磁盘,网络),启动的时候将资源状态报告JobManager.  
    * `Client`
    > 用户提交一个flink程序,就会创建一个Client.Client对用户的flink程序预处理后提交到flink集群  
    > Client将用户提交的Flink程序组装成JobGraph,并以JobGraph提交  