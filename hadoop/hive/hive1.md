# hive应用篇:
## 开启hive应用:
### ........................
	- 首先hiveserver2 将hive作为服务允许进行访问
	-  再开一个链接窗口:用beeline开启链接窗口
	- 在beeline状态下,输入  !connect jdbc:hive2://ip:端口号(10000)
		- 输入用户名和密码:
	- hive为什么要有元数据库(mysql)?
		- 因为hive表所对应的数据是存储在hdfs上的,这个关联关系我们需要继续记录

## hive:
	- dll操作:
	
- [lala](https://camo.githubusercontent.com/769ebcb8834f30cbb863621e9b3cf94b3332ff1c/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31343436363537372d383434356133333064306437363637622e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)
		
		- 内部表:
			create table mytable(id int,name string) 
			row format delimited fields terminated by '\t';
		- 外部表:
			- create external table mytable(id int,name string)
			  row format delimited fields terminated by '\t'
			  location '/路径';
	- 内部表与外部表的区别	
		- 外部表用于引用外部数据，也不希望对原始数据继续破坏，配合location来指定外部表要读取数据；
		    删除内部表或者清空会将数据真正删除；外部表只是将表关联信息删除掉，对指定位置内容没有影响
	- 分区表:
		- 相当于表中又分了小表，减少查询量，提升查询效率
			分区不是只能有一层的，可以创建多层
			创建好分区后，将数据导入要指定好对应分区
		- create table mytable(id int,name string)
			partitioned by(分区的依据) 	row format delimited fields terminated by '\t';
	- 桶表:
		- create table mytable(id int,name string)
			clustered by(分表依据) into 2 buckets
			row format delimited fields terminated by '\t'
			stored as textfile;
		- 当向桶表添加数据时,会出现的情况:

- [lalalas](https://camo.githubusercontent.com/efc63f2d7a049b548139c82419ced558806fa72b/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31343436363537372d363561613363643432343732346565642e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)
			
		- 在分桶以前,一定要设置允许分桶:
			- set hive.enforce.bucketing=true;
			
		- 设置成功后:

- [gfsd](https://camo.githubusercontent.com/eb15677f91f1deaf656cfb23c6426eb806119d3a/68747470733a2f2f75706c6f61642d696d616765732e6a69616e7368752e696f2f75706c6f61645f696d616765732f31343436363537372d623064663533336335646666646630382e706e673f696d6167654d6f6772322f6175746f2d6f7269656e742f7374726970253743696d61676556696577322f322f772f31323430)		
	
### ddl操作:
	- 查询分区
		- show partitions;
	- 增加/删除分区:
		- alter table 表名 add partition(添加的分区) 
			localtion /user/hive/warehouse/mydb.db/表名/属性;
		- alter table 表名 drop if exists partition(删除的分区);
	- 修改表名:
		- alter table student rename to student_mdf
	- 增加或删除或替换列:
		- alter table 表名 add columns (属性);
		
		- alter table 表名 change 属性;
		
		- alter table 表名 replace columns (属性);
	- 删除表数据,保留结构:
		- truncate table 表名;
	- 删除表:
		- drop table 表名;

### dml 插入数据:
	- 插入一条数据;
		- insert into table 表名 values();
	- 导入数据:
		- 从hdfs上导入数据:load data inpath 'hdfs上的文件路径' into table 表名;
		- 从本地上导入数据:load data local inpath '文件路径' into table 表名;
		- load相当于剪切,把文件复制到表中,源文件路径下不再存在
	- 导出数据:
		- insert overwrite[local] directory'路径'
			select * from 表;
			
### 动态分区:
	- 使用场景：当我们想要对数据分区的时候，拿到的数据未必是已经分好区的，并不能直接load进来，
				这个时候使用动态分区来结果.
		- 步骤:
			1.将混乱数据传入一个表.
			2.创建对应分区表
			3.向分区表动态分区的插入数据
				- 如果要进行动态分区,就不要在partition(属性)给分区设置固定值
				- 默认是严格格式,不允许使用动态分区
				- 使用动态分区会将查询结构最后一个作为分区条件
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			
			