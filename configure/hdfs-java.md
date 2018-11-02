# Hadoop Eclipse插件的使用
## 获取并把插件放进eclipse中
	- 首先下载插件: hadoop-eclipse-plugin-1.1.2.jar
	- 把下载后的插件放置到eclipse的plugins目录下，然后重新启动eclipse
	- 重新启动eclipse之后，单击按钮，添加hadoop eclipse插件视图按钮：
		首先选择Other选项，弹出如下图所示的对话框，从中选择Map/Reduce选项，然后单击OK即可。
	
- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221545101112561.jpg)
	
- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221547576426478.jpg)

	- 添加完成后，eclipse中就会多出一个Map/Reduce视图按钮，我们可以点击进入Map/Reduce工作目录视图：
	
- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221551527991504.jpg)

## Hadoop Eclipse插件的基本配置
	- 设置Hadoop的安装目录
		在eclipse中选择Windows→Preference按钮，弹出一个对话框，在该对话框左侧会多出一个Hadoop Map/Reduce选项，
		然后单击此选项，在右侧设置Hadoop的安装目录。

- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221557034086245.jpg)

	- 设置Hadoop的集群信息
		这里需要与Hadoop集群建立连接，在Map/Reduce Locations界面中右击，弹出选项条，
			选择New Hadoop Location选项；
		在弹出的对话框中填写连接hadoop集群的信息，如下图所示：
		
- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221601595496538.jpg)

	- 　接下来，单击Advanced parameters选项卡中的hadoop.tmp.dir选项，修改为你的Hadoop集群中设置的地址，我这里Hadoop集群中设置的地址是/usr/local/hadoop/tmp，
		然后单击Finish按钮（这个参数在core-site.xml中进行了配置）
		
- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221618290648402.jpg)
	
	- 刚刚的配置完成后，返回eclipse中，我们可以看到在Map/Reduce Locations下面就会多出来一个Hadoop-Master的连接项，
		这就是刚刚建立的名为Hadoop-Master的Map/Reduce Location连接，如下图所示：
		
- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221620507052296.jpg)

## 查看HDFS
	- 通过选择eclipse左侧的DFS Locations下面的Hadoop-Master选项，就会展示出HDFS中的文件结构；

- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221622547058946.jpg)

	- 这里在testdir文件夹处右击选择一个指定的文件，如下图所示：
	
- [返回主目录](https://images0.cnblogs.com/blog/381412/201502/221629528309707.jpg)

