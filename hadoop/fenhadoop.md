# hadoop的完全分布式配置
## 想要配置好hadoop完全分布式,首先要确定一台namenode虚拟机
### 确定好namenode后进行 hadoop的伪分布式配置文件改变.

### 配置好后 创建三个单点hadoop的虚拟机 
	- scp -r 你要传的hadoop配置文件 用户名@ip:存放的路径
	- 这样就配置好了三个datanode

### 然后改namedode里的vim hadoop-2.7.3/etc/hadoop/slaves 
	- 在此文件下,添加你的三个datanode的ip地址

### 改一下vim /etc/hosts 
	- 在此文件中添加三个datanode的ip地址和他们的名称,便于记忆
	
### 配置好以后 :
	- hadoop namenode -format  执行此命令
	
![TIM截图20181030200631.png](https://upload-images.jianshu.io/upload_images/14477271-2e7343c59cc13820.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- 出现此命令就说明成功
	- start-all.sh 执行此命令
	- 用  jps来验证是否成功
	
![TIM截图20181030200847.png](https://upload-images.jianshu.io/upload_images/14477271-c15c645f24a7a0a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
### 在查看datanode是否成功:
	- 用  jps来验证是否成功
	
![TIM截图20181030204237.png](https://upload-images.jianshu.io/upload_images/14477271-09521ee6ff4b4f60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

