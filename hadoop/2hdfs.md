# datanode机制与hdfs的应用
## DATANODE的工作机制
### 基础:
	- 概述:
		- 存储管理用户的文件块数据
		定期向namenode汇报自身所持有的block信息（通过心跳信息上报）
		（这点很重要，因为，当集群中发生某些block副本失效时，集群如何恢复block初始副本数量的问题）
		
	- <property>
		<name>dfs.blockreport.intervalMsec</name>
		<value>3600000</value>
		<description>Determines block reporting interval in milliseconds.</description>
	- </property>
	- Datanode掉线判断时限参数
		- datanode进程死亡或者网络故障造成datanode无法与namenode通信，namenode不会立即把该节点判定为死亡，
		要经过一段时间，这段时间暂称作超时时长。HDFS默认的超时时长为10分钟+30秒。
		如果定义超时时间为timeout，则超时时长的计算公式为：
		timeout  = 2 * heartbeat.recheck.interval + 10 * dfs.heartbeat.interval。
		而默认的heartbeat.recheck.interval 大小为5分钟，dfs.heartbeat.interval默认为3秒。
		需要注意的是hdfs-site.xml 配置文件中的heartbeat.recheck.interval的单位为毫秒，
		dfs.heartbeat.interval的单位为秒。所以，举个例子，如果heartbeat.recheck.interval设置为5000（毫秒），
		dfs.heartbeat.interval设置为3（秒，默认），则总的超时时间为40秒。
	- <property>
			<name>heartbeat.recheck.interval</name>
			<value>2000</value>
	- </property>
	- <property>
			<name>dfs.heartbeat.interval</name>
			<value>1</value>
	- </property>
	
### 观察验证DATANODE功能
	- 上传一个文件，观察文件的block具体的物理存放情况：

在每一台datanode机器上的这个目录中能找到文件的切块：

## hdfs应用开发:
### 环境的创建:
	
![TIM截图20181101192446.png](https://upload-images.jianshu.io/upload_images/14477271-977ec4b515f5c886.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 获取api中的客户端对象:
	
![TIM截图20181101192802.png](https://upload-images.jianshu.io/upload_images/14477271-0654361498148830.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 实现简单的增删改查:(本文我以一个为例)
		
![TIM截图20181101193010.png](https://upload-images.jianshu.io/upload_images/14477271-57c98cc1717987d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181101193053.png](https://upload-images.jianshu.io/upload_images/14477271-2680ccaf97e7f25d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 实现对blck块的读取

![TIM截图20181101193449.png](https://upload-images.jianshu.io/upload_images/14477271-67fc9fccd280e18a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181101193527.png](https://upload-images.jianshu.io/upload_images/14477271-d4073ae66168aee5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181101193552.png](https://upload-images.jianshu.io/upload_images/14477271-917ebcbe36a5bd86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	