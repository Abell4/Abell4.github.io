# 在Linux中安装jdk-java
## 首先下载jdk文件
	-可以现在.tar.gz的压缩包文件
	-也可以下载 .prm文件
## 第二步
	-下载并打开文件传输软件(有多种.)
	
![38AB`D588TE6~QG16.png](https://upload-images.jianshu.io/upload_images/14477271-0432a3bfbc708812.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
	-把你所下好的文件放进 /usr 根目录下
	-开启你的Linux系统

## 第三步
	- 来到Linux系统usr根目录下
	- 用ls 查看是否有你刚才传送的文件
	-  (1):如果是.tar.gz的文件   就用 -tar  -xvf  文件名 
	   (2):如果是.prm文件  就用 -prm -i 文件名
	- 然后配置环境变量... 在/etc/profile中配置环境变量
	
![image.png](https://upload-images.jianshu.io/upload_images/14477271-4b46ead4770c7916.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- :wq  保存就好了

## 验证
	- java -version   验证是否配置成功
	
![imffage.png](https://upload-images.jianshu.io/upload_images/14477271-7f45ad0ff2a875c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- [返回主目录](https://abell4.github.io/)
- [百度搜索](http://baidu.com)	