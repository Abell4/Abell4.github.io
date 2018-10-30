#shell编程
## shell简介:
### 基本格式:
	- 代码写在普通文本中,通常以.sh结尾
	- 脚本的第一行固定为 #!/bin/bash
	- 第一行是用来定义shell文件用哪种解析器

### shell 的执行方式:
	- source命令为称为"点命令",也就是点符号(.),是bash的内部命令

## bash基本功能
### 历史命令
	- history   输入后显示以前所执行的命令

### 选项;
		- c: 清楚历史命令
		- w:把缓存的历史命令写入历史命令保存文件中:  保存位置为 :~/.bash_history

## 命令的别名:
	- alias   (创建别名)
			- eg:
	![TIM截图20181023204807.png](https://upload-images.jianshu.io/upload_images/14477271-4183f87091fce04a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- unalias 加别名 (删除别名)
### 输出输入重定向
	
![TIM截图20181023205229.png](https://upload-images.jianshu.io/upload_images/14477271-e57e7f74568b6e75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

	- 管道符 (|)
		- 用法:将左侧的命令输出结果,作为右侧命令的处理对象
	- 格式
		- cmd1 | cmd2 [ ...| cmdn ]

### 通配符

![TIM截图20181023205602.png](https://upload-images.jianshu.io/upload_images/14477271-7c9ffca563dc189d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 其他特殊符号

![TIM截图20181023205728.png](https://upload-images.jianshu.io/upload_images/14477271-52f55feaa13a5a78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 常用热键
	- ctrl+d  输入已结束
	- ctrl+c  键盘中断请求
	- ctrl+s  暂停屏幕输出     
	- ctrl+q  恢复屏幕输出
	- ctrl+l  清屏,相当于clear
	- tab     自动补完命令行与文件名
	- ctrl+u  删除当前光标前的所以字符
	- ctrl+k  删除当前光标后的所以字符
	
### 截取命令
	- cut <选项> 文件
		- -d:指定分割符
		- -f:依据-d的分隔符将一段信息分割成为数段.用-f取出第几段
		- -c:指定几个分割符
		- eg:........如下
	
	
![TIM截图20181023211240.png](https://upload-images.jianshu.io/upload_images/14477271-9d911be316917287.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## shell编程
### shell中的变量
	- shell中的变量分为"系统变量"与"用户自定义变量".可以通过set命令查看系统变量
	- set (显示所有系统变量)
### 自定义变量
	- 变量名=变量值
	
![TIM截图20181024085832.png](https://upload-images.jianshu.io/upload_images/14477271-c0d50a47b3dded9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024191636.png](https://upload-images.jianshu.io/upload_images/14477271-c5ba920de9cd4b06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### read命令:
	- read -p "内容" -n(字符个数) -t(等待时间)

### 将命令的返回值赋给变量
	- ' '运行里面的命令,并显示或传参
	- $( )等价于单引号

### 取消变量
	- unset 变量名

### 特殊变量
	
![TIM截图20181024192533.png](https://upload-images.jianshu.io/upload_images/14477271-915ab5d0bb11560c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 算数运算
	- expr 变量 运算符 变量
	
![TIM截图20181024192841.png](https://upload-images.jianshu.io/upload_images/14477271-ef782f6a2a304c87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 判断指令
	- test命令
		- 测试特定的表达式是否成立,当条件成立时,测试语句的返回值为0,否则为其他数值
		- 格式: test 表达式

### 文件测试
	- 格式: [ 操作符 文件或目录 ]  注意 :括号的前后要有空格
		- 常用的测试操作符
		
![TIM截图20181024193551.png](https://upload-images.jianshu.io/upload_images/14477271-f03f51a3f14a232a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024193715.png](https://upload-images.jianshu.io/upload_images/14477271-24fc430c43c258d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 文件权限测试
	- 格式: [ 操作符 文件或目录 ]
	
![TIM截图20181024194032.png](https://upload-images.jianshu.io/upload_images/14477271-b8a150afbfe1ed05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024194115.png](https://upload-images.jianshu.io/upload_images/14477271-8cb39238164fac48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
### 数值比较
	- 格式: [ 整数1 操作符 整数2 ]
	
![TIM截图20181024194328.png](https://upload-images.jianshu.io/upload_images/14477271-16751fe25a56eeab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 字符串比较
![TIM截图20181024194502.png](https://upload-images.jianshu.io/upload_images/14477271-6eeef22821ece820.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024194702.png](https://upload-images.jianshu.io/upload_images/14477271-6a29171cdd655432.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



	
	
	
	
- [下一篇:shell(2)的基础应用](https://abell4.github.io/linux/Shell2)
- [返回主目录](https://abell4.github.io/)
- [百度搜索](http://baidu.com)
	