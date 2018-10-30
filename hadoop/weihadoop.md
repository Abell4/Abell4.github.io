# hadoop的伪分布式配置
## 伪分布式的配置是基于单点hadoop的配置的.所以要先配好单点hadoop
### 准备工作:
	- 配置好单点hadoop

### 修改配置文件:
	- 首先回到根目录下
	- vim hadoop-2.7.3/etc/hadoop/hadoop-env.sh 打开此文件,修改javahome:  把自己的java配置路径写进去
	
### vim hadoop-2.7.3/etc/hadoop/core_cite.xml 打开此文件
	- <configuration></configuration>中添加下面的语句
		- <property>
			<name>hadoop.tmp.dir</name>
			<value>你自己的根目录文件路径</value>
		</property>
		<property>
			<name>fs.defaultFS</name>
			<value>hdfs://自己的ip:9000</value>
		</property>
		
### vim hadoop-2.7.3/etc/hadoop/hdfs-site.xml 打开此文件
	- <configuration></configuration>中添加下面的语句
		- <property>
			<name>dfs.namenode.name.dir</name>
			<value>file:/home/baitian/dfs/name</value>
		</property>
		<property>
			<name>dfs.datanode.data.dir</name>
			<value>file:/home/baitian/dfs/data</value>
		</property>

### vim hadoop-2.7.3/etc/hadoop/yarn-site.xml打开此文件
	- <configuration></configuration>中添加下面的语句
		- <property>
			<name>mapreduce.framework,name</name>
			<value>yarn</value>
		</property>
		<property>
			<name>yarn.nodemanager.aux-services</name>
			<value>mapreduce_shuffle</value>
		</property>

### vim hadoop-2.7.3/etc/hadoop/mapred-site.xml打开此文件
	- <configuration></configuration>中添加下面的语句
		- <property>
			<name>mapreduce.framework.name</name>
			<value>yarn</value>
		</property>

### 配置好以后 :
	- hadoop namenode -format  执行此命令
	
![TIM截图20181030200631.png](https://upload-images.jianshu.io/upload_images/14477271-2e7343c59cc13820.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- 出现此命令就说明成功
	- start-all.sh 执行此命令
	- 用  jps来验证是否成功
	
![TIM截图20181030200847.png](https://upload-images.jianshu.io/upload_images/14477271-c15c645f24a7a0a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	
- [下一篇:Hadoop完全分布式配置](https://abell4.github.io/hadoop/fenhadoop)
- [返回主目录](https://abell4.github.io/)
- [百度搜索](http://baidu.com) 

