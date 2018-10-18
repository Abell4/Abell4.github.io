# linux第一天
## 上午
#### 1.虚拟机的配置
##### 因为linux是开源的 所以有许多版本 本文用的CentOS
![[NOQ]2MR7IURB4KLRR}WP6.png](https://upload-images.jianshu.io/upload_images/14477271-326f4315a259f914.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 2.在安装好后设置好root的密码和其他设置后就可以操作了
### linux的文件功能 根目录下:
#### 1. bin : 存放二进制可执行文件
#### 2. etc : 存放系统配置文件
#### 3. home : 存放用户文件的根目录
#### 4. root : 超级用户目录
#### 等等.....
## 下午
###  linux基本操作
#### 新建用户
	- useradd +用户名
	- passwd +用户名 
	- ....(回车后) +密码
	- ....(回车后) +确认密码
	
#### 关/重启
##### 重启
		- reboot 
		- shutdown -r now 立即重启 (root用户使用)now可以换成时间
		- shutdown -c 取消重启
		
##### 关机
		- halt 立即关机
		- poweroff 立刻关机
		- shutdown -h now 立刻关机(root用户使用)now可以换成时间

#### linux运行级别
	- init +(0-6) 切换运行级
	- runlevel 查看运行级  如果结果前面有N,显示未切换过运行级
	
#### 配置
##### 主机名
	- hostname 显示主机名
	- hostname +修改的名字 修改主机名(临时修改)
	- 如果想永久修改主机名, 去找/etc/hostname文件去修改
	
##### ping命令
	- ping [选项] 目标主机(号)  测试网络连通性
##### 查看和设置防火墙状态
	- service firewalld status centos7关闭防火墙
	- systemctl stop firewalld 临时关闭(仅限于本次开机)
	- sysytenctl disable firew 禁止开机启动防火墙
	
##### 本地主机映射文件
	- 在/etc/hosts文件中..此文件记录ip地址的映射

##### SELinux的配置
	- setenforce 该命令可以立即改变SELinuxde运行状态 (临时修改)
	- 修改配置文件/etc/selinux/config，将SELINUX=enforcing修改为SELINUX=disabled重启生效
	

- [下一篇](https://abell4.github.io/linux/jichuoneday)
- [返回主目录](https://abell4.github.io/)
- [百度搜索](http://baidu.com) 

