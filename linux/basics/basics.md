# 关于Linux的基本插件与软件的安装
## 在Linux里插件的安装分为两部分
### 在内部调用 Yellow dog Updater, Modified 下载
#### 讲解:
	- 是一个在Fedora和RedHat以及SUSE、CentOS中的Shell前端软件包管理器。
		基於RPM包管理，能够从指定的服务器自动下载RPM包并且安装，
		可以自动处理依赖性关系，并且一次安装所有依赖的软件包，
		无须繁琐地一次次下载、安装。
	- yum [选项] [状态]   需要的版本文件	
		-选项 :-h（帮助）
			   -y（当安装过程提示选择全部为"yes"）
			   -q（不显示安装的过程）
		-状态 :-install 安装
			   -remove  卸载
			   -update  更新
			   
#### 通过命令解压安装包
	- 通过此软件把需要解压的压缩包放到虚拟机中
	- 然后解压

![$TWFPUQ2~DW6`UWP3MCLK.png](https://upload-images.jianshu.io/upload_images/14477271-bf2dc8cda08488d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)	

#### 安装
	- 如果上面传送的是安装包文件
	- 就用rmp命令
	- i：安装应用程序（install）
	- e：卸载应用程序（erase）
	- vh：显示安装进度；（verbose   hash） 
	- U：升级软件包；（update） 
	- qa: 显示所有已安装软件包（query all）
	  结合grep命令使用
	  

- [下一篇:正则表达式(基础)](https://abell4.github.io/)  
- [返回主目录](https://abell4.github.io/)
- [百度搜索](http://baidu.com)
	  

