# hadoop的单点配置
## 今天就正式进入hadoop了:

### 首先要将hadoop的压缩包导入根目录下,这样才可以轻松解压缩

### 解压缩后就会生成一个hadoop文件夹,此时就证明解压缩成功:
	- tar -zxvf +你要解压缩的文件
	
### 接下来就要调一下全局的hadoop变量了.
#### 此时就有两种调试方法. (1):是在root根目录的/etc/profile中改
						   (2):是在你此时用户的根目录下的 .absh_profile文件中改
					其实:就是加上以下的几句话

![TIM截图20181030192533.png](https://upload-images.jianshu.io/upload_images/14477271-ac467bae25772b96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 此时在用sh 运行一下你的修改的配置文件就ok了

#### 如果想验证的话,就输入hadoop命令.看是否有反应.

## 注意:在配置hadoop之前一定要配置java的jdk!!!!!!!!					
						