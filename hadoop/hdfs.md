# hdfs篇
## 基础篇:
### 特性与概念
#### (一)
	- 首先，它是一个文件系统，用于存储文件，通过统一的命名空间——目录树来定位文件

	- 其次，它是分布式的，由很多服务器联合起来实现其功能，集群中的服务器有各自的角色

#### 特性:
	- HDFS中的文件在物理上是分块存储（block），块的大小可以通过配置参数( dfs.blocksize)来规定，默认大小在hadoop2.x版本中是128M，老版本中是64M

	- HDFS文件系统会给客户端提供一个统一的抽象目录树，客户端通过路径来访问文件，形如：
	
	- [eg](hdfs://namenode:port/dir-a/dir-b/dir-c/file.data)

	- 目录结构及文件分块信息(元数据)的管理由namenode节点承担
		- namenode是HDFS集群主节点，负责维护整个hdfs文件系统的目录树，以及每一个路径（文件）所对应的block块信息（block的id，及所在的datanode服务器）
		
	- 文件的各个block的存储管理由datanode节点承担
		- datanode是HDFS集群从节点，每一个block都可以在多个datanode上存储多个副本（副本数量也可以通过参数设置dfs.replication）

	- HDFS是设计成适应一次写入，多次读出的场景，且不支持文件的修改
	
### 基础操作:
#### 命令黄客户端支持的命令参数
		-  [-appendToFile <localsrc> ... <dst>]
		- [-cat [-ignoreCrc] <src> ...]
        - [-checksum <src> ...]
        - [-chgrp [-R] GROUP PATH...]
        - [-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
        - [-chown [-R] [OWNER][:[GROUP]] PATH...]
        - [-copyFromLocal [-f] [-p] <localsrc> ... <dst>]
        - [-copyToLocal [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
        - [-count [-q] <path> ...]
        - [-cp [-f] [-p] <src> ... <dst>]
        - [-createSnapshot <snapshotDir> [<snapshotName>]]
        - [-deleteSnapshot <snapshotDir> <snapshotName>]
        - [-df [-h] [<path> ...]]
        - [-du [-s] [-h] <path> ...]
        - [-expunge]
        - [-get [-p] [-ignoreCrc] [-crc] <src> ... <localdst>]
        - [-getfacl [-R] <path>]
        - [-getmerge [-nl] <src> <localdst>]
        - [-help [cmd ...]]
        - [-ls [-d] [-h] [-R] [<path> ...]]
        - [-mkdir [-p] <path> ...]
        - [-moveFromLocal <localsrc> ... <dst>]
        - [-moveToLocal <src> <localdst>]
        - [-mv <src> ... <dst>]
        - [-put [-f] [-p] <localsrc> ... <dst>]
        - [-renameSnapshot <snapshotDir> <oldName> <newName>]
        - [-rm [-f] [-r|-R] [-skipTrash] <src> ...]
        - [-rmdir [--ignore-fail-on-non-empty] <dir> ...]
        - [-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
        - [-setrep [-R] [-w] <rep> <path> ...]
        - [-stat [format] <path> ...]
        - [-tail [-f] <file>]
        - [-test -[defsz] <path>]
        - [-text [-ignoreCrc] <src> ...]
        - [-touchz <path> ...]
        - [-usage [cmd ...]]
		
#### 常用命令参数:
	- -help             
		功能：输出这个命令参数手册
	-ls                  
		功能：显示目录信息
		示例： hadoop fs -ls hdfs://hadoop-server01:9000/
		备注：这些参数中，所有的hdfs路径都可以简写
		-->hadoop fs -ls /   等同于上一条命令的效果
		
	-mkdir              
		功能：在hdfs上创建目录
		示例：hadoop fs  -mkdir  -p  /aaa/bbb/cc/dd
		
	-moveFromLocal            
		功能：从本地剪切粘贴到hdfs
		示例：hadoop  fs  - moveFromLocal  /home/hadoop/a.txt  /aaa/bbb/cc/dd
		
	-moveToLocal              
		功能：从hdfs剪切粘贴到本地
		示例：hadoop  fs  - moveToLocal   /aaa/bbb/cc/dd  /home/hadoop/a.txt
		
	--appendToFile  
		功能：追加一个文件到已经存在的文件末尾
		示例：hadoop  fs  -appendToFile  ./hello.txt  hdfs://hadoop-server01:9000/hello.txt
		可以简写为：
			Hadoop  fs  -appendToFile  ./hello.txt  /hello.txt

	-cat  
		功能：显示文件内容  
		示例：hadoop fs -cat  /hello.txt

	-tail                 
		功能：显示一个文件的末尾
		示例：hadoop  fs  -tail  /weblog/access_log.1
			
	-text                  
		功能：以字符形式打印一个文件的内容
		示例：hadoop  fs  -text  /weblog/access_log.1
		
	-chgrp
	
	-chmod
	
	-chown
		功能：linux文件系统中的用法一样，对文件所属权限
		示例：
		hadoop  fs  -chmod  666  /hello.txt
		hadoop  fs  -chown  someuser:somegrp   /hello.txt
		
	-copyFromLocal    
		功能：从本地文件系统中拷贝文件到hdfs路径去
		示例：hadoop  fs  -copyFromLocal  ./jdk.tar.gz  /aaa/
		
	-copyToLocal      
		功能：从hdfs拷贝到本地
		示例：hadoop fs -copyToLocal /aaa/jdk.tar.gz
		
	-cp              
		功能：从hdfs的一个路径拷贝hdfs的另一个路径
		示例： hadoop  fs  -cp  /aaa/jdk.tar.gz  /bbb/jdk.tar.gz.2

	-mv                     
		功能：在hdfs目录中移动文件
		示例： hadoop  fs  -mv  /aaa/jdk.tar.gz  /
		
	-get              
		功能：等同于copyToLocal，就是从hdfs下载文件到本地
		示例：hadoop fs -get  /aaa/jdk.tar.gz
		
	-getmerge             
		功能：合并下载多个文件
		示例：比如hdfs的目录 /aaa/下有多个文件:log.1, log.2,log.3,...
		hadoop fs -getmerge /aaa/log.* ./log.sum
		
	-put                
		功能：等同于copyFromLocal
		示例：hadoop  fs  -put  /aaa/jdk.tar.gz  /bbb/jdk.tar.gz.2

	-rm                
		功能：删除文件或文件夹
		示例：hadoop fs -rm -r /aaa/bbb/

	-rmdir                 
		功能：删除空目录
		示例：hadoop  fs  -rmdir   /aaa/bbb/ccc
		
	-df               
		功能：统计文件系统的可用空间信息
		示例：hadoop  fs  -df  -h  /

	-du 
		功能：统计文件夹的大小信息
		示例：
			hadoop  fs  -du  -s  -h /aaa/*

	-count         
		功能：统计一个指定目录下的文件节点数量
		示例：hadoop fs -count /aaa/

	-setrep                
		功能：设置hdfs中文件的副本数量
		示例：hadoop fs -setrep 3 /aaa/jdk.tar.gz
	
## 原理篇:
### hdfs的工作机制:
	- HDFS集群分为两大角色：NameNode、DataNode  (Secondary Namenode)
	- NameNode负责管理整个文件系统的元数据
	- DataNode 负责管理用户的文件数据块
	- 文件会按照固定的大小（blocksize）切成若干块后分布式存储在若干台datanode上
	- 每一个文件块可以有多个副本，并存放在不同的datanode上
	- Datanode会定期向Namenode汇报自身所保存的文件block信息，而namenode则会负责保持文件的副本数量
	- HDFS的内部工作机制对客户端保持透明，客户端请求访问HDFS都是通过向namenode申请来进行
	
### hdfs的写数据流程
	- 客户端将要读取的文件路径发送给namenode，namenode获取文件的元信息
		返回给客户端，客户端根据返回的信息找到相应datanode逐个获取文件的block并
		在客户端本地进行数据追加合并从而获得整个文件
	- 示例图:
	
	![图片1.png](https://upload-images.jianshu.io/upload_images/14477271-d965275414492922.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- 跟namenode通信查询元数据，找到文件块所在的datanode服务器
		挑选一台datanode（就近原则，然后随机）服务器，请求建立socket流
		datanode开始发送数据（从磁盘里面读取数据放入流，以packet为单位来做校验）
		客户端以packet为单位接收，现在本地缓存，然后写入目标文件

### NameNode工作机制:
	- 职责
		- 负责客户端请求的响应
		- 元数据的管理（查询，修改）
	- namenode对数据的管理采用了三种存储形式：
		内存元数据(NameSystem)
		磁盘元数据镜像文件
		数据操作日志文件（可通过日志运算出元数据
	- 查看元数据
		可以通过hdfs的一个工具来查看edits中的信息
			bin/hdfs oev -i edits -o edits.xml
			bin/hdfs oiv -i fsimage_0000000000000000087 -p XML -o fsimage.xml
	- checkpoint的详细过程

![图片2.png](https://upload-images.jianshu.io/upload_images/14477271-e2e85f19ee2098fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
		
checkpoint操作的触发条件配置参数
dfs.namenode.checkpoint.check.period=60  #检查触发条件是否满足的频率，60秒
dfs.namenode.checkpoint.dir=file://${hadoop.tmp.dir}/dfs/namesecondary
#以上两个参数做checkpoint操作时，secondary namenode的本地工作目录
dfs.namenode.checkpoint.edits.dir=${dfs.namenode.checkpoint.dir}

dfs.namenode.checkpoint.max-retries=3  #最大重试次数
dfs.namenode.checkpoint.period=3600  #两次checkpoint之间的时间间隔3600秒
dfs.namenode.checkpoint.txns=1000000 #两次checkpoint之间最大的操作记录
checkpoint的附带作用
namenode和secondary namenode的工作目录存储结构完全相同，所以，当namenode故障退出需要重新恢复时，可以从secondary namenode的工作目录中将fsimage拷贝到namenode的工作目录，以恢复namenode的元数据

5.2.4 元数据目录说明
在第一次部署好Hadoop集群的时候，我们需要在NameNode（NN）节点上格式化磁盘：
$HADOOP_HOME/bin/hdfs namenode -format
格式化完成之后，将会在$dfs.namenode.name.dir/current目录下如下的文件结构
current/
|-- VERSION
|-- edits_*
|-- fsimage_0000000000008547077
|-- fsimage_0000000000008547077.md5
`-- seen_txid
其中的dfs.name.dir是在hdfs-site.xml文件中配置的，默认值如下：
<property>
  <name>dfs.name.dir</name>
  <value>file://${hadoop.tmp.dir}/dfs/name</value>
</property>

hadoop.tmp.dir是在core-site.xml中配置的，默认值如下
<property>
  <name>hadoop.tmp.dir</name>
  <value>/tmp/hadoop-${user.name}</value>
  <description>A base for other temporary directories.</description>
</property>

dfs.namenode.name.dir属性可以配置多个目录，
如/data1/dfs/name,/data2/dfs/name,/data3/dfs/name,....。各个目录存储的文件结构和内容都完全一样，相当于备份，这样做的好处是当其中一个目录损坏了，也不会影响到Hadoop的元数据，特别是当其中一个目录是NFS（网络文件系统Network File System，NFS）之上，即使你这台机器损坏了，元数据也得到保存。
下面对$dfs.namenode.name.dir/current/目录下的文件进行解释。
1、VERSION文件是Java属性文件，内容大致如下：
#Fri Nov 15 19:47:46 CST 2013
namespaceID=934548976
clusterID=CID-cdff7d73-93cd-4783-9399-0a22e6dce196
cTime=0
storageType=NAME_NODE
blockpoolID=BP-893790215-192.168.24.72-1383809616115
layoutVersion=-47


其中
　　（1）、namespaceID是文件系统的唯一标识符，在文件系统首次格式化之后生成的；
　　（2）、storageType说明这个文件存储的是什么进程的数据结构信息（如果是DataNode，storageType=DATA_NODE）；
　　（3）、cTime表示NameNode存储时间的创建时间，由于我的NameNode没有更新过，所以这里的记录值为0，以后对NameNode升级之后，cTime将会记录更新时间戳；
　　（4）、layoutVersion表示HDFS永久性数据结构的版本信息， 只要数据结构变更，版本号也要递减，此时的HDFS也需要升级，否则磁盘仍旧是使用旧版本的数据结构，这会导致新版本的NameNode无法使用；
　　（5）、clusterID是系统生成或手动指定的集群ID，在-clusterid选项中可以使用它；如下说明

a、使用如下命令格式化一个Namenode：
$HADOOP_HOME/bin/hdfs namenode -format [-clusterId <cluster_id>]
选择一个唯一的cluster_id，并且这个cluster_id不能与环境中其他集群有冲突。如果没有提供cluster_id，则会自动生成一个唯一的ClusterID。
b、使用如下命令格式化其他Namenode：
 $HADOOP_HOME/bin/hdfs namenode -format -clusterId <cluster_id>
c、升级集群至最新版本。在升级过程中需要提供一个ClusterID，例如：
$HADOOP_PREFIX_HOME/bin/hdfs start namenode --config $HADOOP_CONF_DIR  -upgrade -clusterId <cluster_ID>
如果没有提供ClusterID，则会自动生成一个ClusterID。
　　（6）、blockpoolID：是针对每一个Namespace所对应的blockpool的ID，上面的这个BP-893790215-192.168.24.72-1383809616115就是在我的ns1的namespace下的存储块池的ID，这个ID包括了其对应的NameNode节点的ip地址。
　　
2、$dfs.namenode.name.dir/current/seen_txid非常重要，是存放transactionId的文件，format之后是0，它代表的是namenode里面的edits_*文件的尾数，namenode重启的时候，会按照seen_txid的数字，循序从头跑edits_0000001~到seen_txid的数字。所以当你的hdfs发生异常重启的时候，一定要比对seen_txid内的数字是不是你edits最后的尾数，不然会发生建置namenode时metaData的资料有缺少，导致误删Datanode上多余Block的资讯。

3、$dfs.namenode.name.dir/current目录下在format的同时也会生成fsimage和edits文件，及其对应的md5校验文件。		
		
		
		
		
		
		
		
		
		
		
		
		
