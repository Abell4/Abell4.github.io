# hadoop介绍:
## 大数据的核心特征 4v:
	- 数据量大（Volume）
	- 类型繁多（Variety）
	- 价值密度低（Value）
	- 速度快时效高（Velocity）
	
## 	技术路线:

![图片1.png](https://upload-images.jianshu.io/upload_images/14477271-e7ae3204c5453a8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## hadoop概述:
	-  Apache开源软件基金会开发的，运行于大规模普通服务器上的，大数据存储、计算、分析的，分布式存储系统和分布式运算框架
	- 组成:
		- 分布式存储系统HDFS
		- 资源管理系统YARN
		- 分布式计算框架MapReduce

## 分布式存储系统HDFS
	- 特点: 
		- 良好的扩展性
		- 容错性高
		- 适合PB级以上海量数据的存储
	- 基本原理:
		-  将文件切分成等大的数据块，存储到多台机器上
		- 将数据切分、容错、负载均衡等功能透明化
		- 可将HDFS看成一个容量巨大、具有高容错性的磁盘
	- 应用场景
		- 海量数据的可靠性存储
		- 数据归档
		
## 资源管理系统YARN
	- 介绍
		- Hadoop2.0新增系统
		- 负责集群的资源管理和调度
		- 使得多种计算框架可以运行在一个集群中
	- 特点
		- 良好的扩展性、高可用性
		- 对多种类型的应用程序进行统一管理和调度
		- 自带了多种多用户调度器，适合共享集群环境
		
## 分布式计算框架MapReduce
	- 特点
		- 良好的扩展性
		- 高容错性
		- 适合PB级以上的海量数据的离线处理
		
## hadoop生态链
	- Hive：数据仓库
	- pig：Pig-Latin/MR编译器
	- Mahout：基于MR的算法库
	- HBase：列式的分布式NoSQL
	- ZooKeeper：分布式协同调度系统
	- Sqoop：ETL工具
	- Flume：数据采集系统
	- Oozie：工作流引擎
	- RPC的基础概念
	- 如何使用RPC
	- RPC应用实例

## RPC 远程过程调用
	- 特点:
		- 透明性：远程调用其他机器上的程序，对用户来说就像是调用本地方法一样
		- 高性能：RPC Server能够并发处理多个来自Client的请求
		- 可控性：jdk中已经提供了一个RPC框架—RMI，但是该PRC框架过于重量级并且可控之处比较少，
			所以Hadoop RPC实现了自定义的PRC框架
		

		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		

