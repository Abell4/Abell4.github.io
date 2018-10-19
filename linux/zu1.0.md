# 用户和组
## 了解用户与组的配置文件
### 保存用户信息的文件：/etc/passwd
### 保存密码的文件：/etc/shadow
### 保存用户组的文件：/etc/group
### 保存用户组密码的文件：/etc/gshadow
### 用户配置文件：/etc/default/useradd

#### 用户与组在Linux配置文件中的默认知识
	- 用户在系统中是分为角色的，通过UID来识别角色
		-root用户，系统唯一，可以操作系统任何文件和命令，拥有最高权限，UID=0 
		-虚拟用户(系统账户)，不具有登录系统能力，但却是系统运行不可缺少的用户。如：bin、daemon、ftp、mail等，UID为1---499之间
		-普通真实用户，可以登录系统，权限有限，靠管理员创建，UID为500—60000之间

#### 系统账户
	- 1 – 99：由distributions自行创建的系统账号
	- 100 – 499：若用户有系统账号需求时，可以使用的UID
	- 为了更好的管理用户，出现组group的概念
	- 基本组（私有组） 当用户创建文件和文件夹时，默认属于私有组
	- 附加组（公共组）
	
![BTP6.png](https://upload-images.jianshu.io/upload_images/14477271-9ec0b810902908af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
### 用户管理命令
#### 添加用户命令
	- useradd   -u 指定组ID（uid）
				-g 指定所属的组名（gid）
				-G 指定多个组，用逗号“，”分开（Groups）
				-c 用户描述（comment）
				-e 失效时间（expire date）
	-eg(如图所示)

![TT%UEGW_XGJ7Z21Q~Y$Q.png](https://upload-images.jianshu.io/upload_images/14477271-343d443f01f03538.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 设置密码
	- passwd [选项] <用户名>
				-d：清空用户的密码，使之无需密码即可登录
				-l：锁定用户帐号
				-S：查看用户帐号的状态（是否被锁定）
				-u：解锁用户帐号
				-x:    最大密码使用时间（天）
				-n:    最小密码使用时间（天）

#### 修改用户属性
	- usermod [选项]: -l 修改用户名 （login）usermod -l a b（b改为a）
					  -g 添加组 usermod -g sys tom
					  -G添加多个组 usermod -G sys,root tom
					  –L 锁定用户账号密码（Lock）
					  –U 解锁用户账号（Unlock） 删除用户命令：userdel（user delete）
					  -r 删除账号时同时删除目录（remove）
					  
#### 删除用户账户
	- userdel [选项] 用户名
				-r :把用户的宿主目录一并删除
				
### 组的管理
	- 如图所示: 组的配置文件路径及各部分作用
	
![%`Q0{S}R6}_ESJFDP~4PF$S.png](https://upload-images.jianshu.io/upload_images/14477271-4cf9c361269fb58f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![H6`7083HGIT5HGIIN$7OFF.png](https://upload-images.jianshu.io/upload_images/14477271-da460782b7a5f933.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 组命令管理
#### 添加组账号
	- groupadd -g  (指定组id)
	
#### 修改组
	- groupmod [选项] 新组名 旧组名
			-n (更改组名) 新组名 旧组名
			-g：设置想要使用的GID
			-o：使用已经存在的GID

				
#### 删除组
	- groupdel <组账号名>
	
#### 显示当前用户所有的所属组
	- groups [用户名]
	
## (最核心)文件及目录权限管理
	- 权限 :
		- 读(r) :读取文件的内容;列出目录里的对象
		- 写(w) :允许修改文件;在目录里面新建或者删除文件
		- 执行(x) :允许执行文件;允许进入目录
	
![IR9L751VGK5~`Q.png](https://upload-images.jianshu.io/upload_images/14477271-b0f7b19b3e843171.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 数字权限
		除了用字母rwx来表示权限，还可以使用3位数字来表 达文件或目录的权限
			读：4
			写：2
			执行：1

### 修改权限
		- chmod [ugoa] [+-=] [rwx] file/dir 或 chmod nnn 修改的文件
			-参数解释:
				u:属主  g:属组  o:其他用户  a:所有用户
				+:添加权限  -:删除权限  =:赋予权限
				nnn:三位八进制的权限
			-常用命令选项:
				-R 递归修改指定目录下的所有子文件及文件夹的权限
				-f 强制改变文件访问特权；如果是文件的拥有者，则得  不到任何错误信息
				


	
- [下一篇](https://abell4.github.io/)
- [返回主目录](https://abell4.github.io/)
- [百度搜索](http://baidu.com)		
	
	
				
