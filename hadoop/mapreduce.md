# mapreduce入门--内容概要
## MapReduce概述
### 特点:
	- 易于编程
	- 良好的扩展性
	- 高容错性
	- 适合PB级以上海量数据的离线处理
### MapReduce的特色--不擅长的方面
	- 实时计算
		- 像MySQL一样.在毫秒级或者秒级内返回结果
	- 流式计算
		- MapReduce的输入数据集是静态的.不能动态变化
		- MapReduce自身的设计特点决定了数据源必须是静态的
	- MapReduce (大规模数据处理)
		- 处理海量数据(>1tb) 上百/上千cpu实现并行处理  简单的实现以上目的
			移动计算比移动数据更划算
	
	- MapReduce是一种并行编程模型，用于对大规模数据集（大于1TB）的并行处理。
		Hadoop中实现了MapReduce框架，为用户提供MapReduce程序的开发接口和运行环境。
	-  MapReduce将作业的这个运行过程分为两个阶段:
		Map阶段和Reduce阶段
	- Map阶段由一定数量的MapTask组成
		- 输入数据格式解释: inputFormat
		- 输入数据处理:Mapper
		- 数据分组: Partitioner
	- Reduce阶段由一定数量的Reduce Task组成
		- 数据远程拷贝
		- 数据按照key排序
		- 数据处理: Reduce
		- 数据输出格式:OutputFormat
	
### MapReduce的应用场景:
	- 场景:有大量文件,里面存储了单词,且一个单词占一行.
	- 类似应用场景:
		- 搜索引擎中:统计最流行的k个搜索词;
		- 统计搜索词频率,帮助优化搜索词提示;
	
![图片1.png](https://upload-images.jianshu.io/upload_images/14477271-7997ff8fd30d9b52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Java版——WordCount
	- 功能：统计输入文件中的每个单词出现的次数
	
![图片2.png](https://upload-images.jianshu.io/upload_images/14477271-f968576969bcbe9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
![图片3.png](https://upload-images.jianshu.io/upload_images/14477271-eeaf3eb2c9c09de5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
![图片4.png](https://upload-images.jianshu.io/upload_images/14477271-7cd98e2b62881fde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图片5.png](https://upload-images.jianshu.io/upload_images/14477271-5c6033f3a8c84c17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图片6.png](https://upload-images.jianshu.io/upload_images/14477271-fbc97112a12398c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 开发一个MapReduce程序
	- 你所需要做的是 实现map()和reduce()函数，
		剩下的框架完成。
		
![图片7.png](https://upload-images.jianshu.io/upload_images/14477271-b9b0039ea8cbfd04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Hadoop2.0新特性:YARN产生背景
	- 直接源于MRv1的几个方面的局限性
		- 扩展性差: JobTracker成为瓶颈,难以支持MR之外的计算
		- 可靠性差,NameNode单点故障
		- 资源利用率低
	- 多计算框架各自为战,数据共享难
		- MR 离线计算框架
		- Storm :实时计算框架
		- Spark :内存计算框架
		
## Hadoop1.0与Hadoop2.0 Mr架构设计区别
	
![图片8.png](https://upload-images.jianshu.io/upload_images/14477271-041e49f45a67cf64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图片9.png](https://upload-images.jianshu.io/upload_images/14477271-d8492af5f06ce7a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图片10.png](https://upload-images.jianshu.io/upload_images/14477271-8c9ff1f4b8e7a89a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## ResourceManager
	- 整个集群只有一个,负责集群资源的统一管理和调度
	- 详细功能:
		- 处理客户端请求
		- 启动/监控ApplicationMaster 
		- 监控NodeManager
		- 资源分配与调度
	- 整个集群有多个,负责单节点资源管理和使用
		- 单个节点上的资源管理和任务管理
		- 处理来自ResoourceManager的命令
		- 处理来自ApplicationMaster的命令 

##  ApplicationMaster
	- 每个应用有一个,负责应用程序的管理
		- 数据切分
		- 为警用程序申请资源,并进一步分配给内部任务
		- 任务监控与容错
	





	