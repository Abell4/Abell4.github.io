# hive的介绍:
## hive的特点:
	- 可扩展性：Hive可以自由的扩展集群的规模，而且一般情况下不需要重启服务
	- 延展性：Hive支持自定义函数UDF，用户可以根据自己的需求来实现自己的函数
	- 容错：良好的容错性，可以保障即使有节点出现问题，SQL  语句仍可完成执行

	![image.png](https://upload-images.jianshu.io/upload_images/14477271-b34237826eb939c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 用户接口
	- CLI：shell终端命令行，最常用（学习，调试，生产）
	- JDBC/ODBC：是Hive的基于JDBC操作提供的客户端，用户通过这连接至 Hive Server Web UI，通过浏览器访问Hive
	- 容错：良好的容错性，可以保障即使有节点出现问题，SQL语句仍可完成执行

## 元数据存储:
	- 通俗的讲，元数据就是存储在Hive中的数据的描述信息，Hive中的元数据通常包括：表的名字，
	表的列和分区及其属性，表的属性（内部表和 外部表），表的数据所在目录。Metastore默认存在自
	带的Derby数据库中。缺点就是不适合多用户操作，并且数据存 储目录不固定。数据库和Hive所在的机器绑定，
	极度不方便管理。通常将元数据存储在我们自己创建的MySQL数据库（本地或远程）当中
	
## hive的组成:
	- 解释器，编译器，优化器，执行器
		- 这四大组件完成 HQL 查询语句从词法分析，语法分析，编译，优化，以及生成查询计 划的生成。
			生成的查询计划存储在 HDFS 中，并随后由MapReduce 调用执行
	- 执行流程:
		- HiveQL通过命令行或者客户端提交，经过Compiler编译器，运用 Metastore中的元数据进行类型检测和语法分析，
		生成一个逻辑方案(logical plan)，然后通过的优化处理，产生一个MapReduce任务

## Hive的数据存储
	- Hive中所有的数据都存储在HDFS中，没有专门的数据存储格式
	- 只需要在创建表的时候告诉 Hive数据中的列分隔符和行分隔符，Hive 就可以解析数据
	- Hive 中包含以下数据模型：
	- db：在hdfs中表现为${hive.metastore.warehouse.dir}目录下一个文件夹
	- table：在hdfs中表现所属db目录下一个文件夹
	- external table：与table类似，不过其数据存放位置可以在任意指定路径
	- partition：在hdfs中表现为table目录下的子目录
	- bucket：在hdfs中表现为同一个表目录下根据hash散列之后的多个文件 桶
	
## Hive环境搭建
	- 上传hive压缩包到虚拟机中;
	- 用tar进行解压缩;
	- 在/etc/porfiles中配置hive到path中
	- . /sh 执行 /etc/porfiles文件
	- hive启动测试:看是否配置成功
	- 安装mysql
		- sudo yum -y install wget
		- wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
		- sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
		- sudo yum -y install mysql-server
		- sudo yum -y install mysql
		- systemctl start mysqld
	- mysql -u root -p(没密码) 登录mysql
	- 设置密码 一定要设置...
	-修改密码：
		UPDATE mysql.user SET authentication_string=PASSWORD('6757DUgu') where USER='root';
		MySQL> UPDATE mysql.user SET Password=PASSWORD('新密码') where USER='root’;
		MySQL> flush privileges;
		MySQL> exit


上传一个mysql的驱动jar包到hive的安装目录的lib中
	- 设置mysql能够远程访问
	- 在 hive(就是你解压缩hive的文件夹)hive/conf目录下创建hive-site.xml,并加入链接mysql数据库的配置
		- <configuration>
	        <property>
	                <name>javax.jdo.option.ConnectionURL</name>
	                <value>jdbc:mysql://127.0.0.1:3306/hivedb?createDatabaseIfNotExist=true</value>
	        </property>
	        <property>
	                <name>javax.jdo.option.ConnectionDriverName</name>
	                <value>com.mysql.jdbc.Driver</value>
	        </property>
	        <property>
	                <name>javax.jdo.option.ConnectionUserName</name>
	                <value>root</value>
	        </property>
	        <property>
	                <name>javax.jdo.option.ConnectionPassword</name>
	                <value>123</value>
	        </property>
		</configuration>
	- 在你的datanode与namemnode中插入如下:
		-  <property>

    <name>yarn.resourcemanager.address</name>

    <value>172.18.24.195:8032</value>

  </property>

  <property>

    <name>yarn.resourcemanager.scheduler.address</name>

    <value>172.18.24.195:8030</value>

  </property>

  <property>

    <name>yarn.resourcemanager.resource-tracker.address</name>

    <value>172.18.24.195:8031</value>

  </property>

	- 启动看是否正常使用






	
	
	
	
	
	
