# linux命令与应用基础篇(二)
## 基础的文件操作与查询
### 查询
	- find <路径> <选项> [表达式]
		-name 根据文件名查找
		-user 根据文件拥有者查找
		-group 根据文件所属组寻找文件
		-perm 根据文件权限查找文件
		-size 根据文件大小查找文件
		-size 根据文件大小查找文件
		-type 根据文件类型查找(f-普通文件,c-字符设备文件,b-块设备文件,l-链接文件,d-目录)
		-o	表达式或
		-and	表达式与
		
### 显示文件内的全部内容
	-cat 文件名/目录

### 全屏分页显示文件的内容
	-more 文件名/目录
		- 按Enter键向下逐行滚动
		- 按空格键向下翻一屏
		- 按b键向上翻一屏
		- 按q键退出

### less 功能与用法同more

### 查看头十行文件内容
	- head -n 同户名   n为定义的行数
	
### 查看文件结尾的内容
	- tail -n 文件名  同上 (看日志文件时多用tail)
	
### 统计文件中的单词数量(Word Count)等信息
	- wc [选项] 目标文件  
		-l 统计行数  -w 统计单词个数  -c 统计字节数
		
### 查找文件里符合条件的字符串
	- grep [选项] <关键字> <文件名/目录>
		-c :计算匹配关键字的行数
		-i :忽略字符大小写的差别
		-n :显示匹配的行及其行号
		-s :不显示不存在或不匹配文本的错误信息
		-h: 查询多个文件时不显示文件名
		-l: 查询文件时只显示匹配字符所在的文件名
		--color=auto: 将找到的关键字部分加上颜色显示
		
### grep的用法

![4KMW40T9PUUZ@YFSQGIFOF.png](https://upload-images.jianshu.io/upload_images/14477271-e7b885747d7d0973.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![XVI7{VOKDH2W9SIDR_KTTQ.png](https://upload-images.jianshu.io/upload_images/14477271-be6a73478064a10d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![10C6AIJ_@)N3X6LAJK%XF.png](https://upload-images.jianshu.io/upload_images/14477271-fb4d78f70166a076.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- [下一篇](https://abell4.github.io/)
- [返回主目录](https://abell4.github.io/)
- [百度搜索](http://baidu.com)


	

