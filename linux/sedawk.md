# sed与awk的用法
## sed用法
### sed [选项] 动作 filename
	- 动作
		- a 新增往下增加
		- i 往上一行添加
		- c 替换:1,5c :会将这块区域整个替换,不会每行替换
		- d 删除
		- p 输出
		- s 替换经常配合正则表达式进行操作
		- w 写入 : sed 'w 被写入的文件名或路径' 读取内容的文件
			- 只要是用w向文件中进行写入就会将目标文件之前的内容全部干掉
		- r 读取 : swd '2r 要读取的文件' 加入数据的文件(目标文件)

### 高级操作:
	- 使用管道符(|)执行多个动作,只是动作之间要用";"分号隔开
	- {}可以让sed执行多个操作,只是动作之间要用";"分号隔开
	- & 替换
		- 相当于占位符的作用,就是我们s/查询条件/要替换的内容中进行使用,实现追加效果
	- \u 转成大写,可以配合我们的&来实现匹配内容转成大写的操作
	- ()分组功能
	
![image.png](https://upload-images.jianshu.io/upload_images/14477271-c277abf8462e9199.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 	awk 行读取
### 基本语法
	- awk options 选项 'command' file(文件)
		- NR 行号
		- NF 列数
		$1,$n
		- F 设置分隔符 默认情况下使用" "空格来进行的切割
		- print 
		- 要使用正则: ~/正则匹配规则/
		如果使用!~就是对正则匹配测内容取非

### awk的完全形式
	- ask options 选项 'BEGIN{} command END{}' file
	- for(int i=0;count;i++)