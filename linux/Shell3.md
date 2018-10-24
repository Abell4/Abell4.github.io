# shell函数应用:
## shell自定义函数
	-  [ function ] funname [()]
	{
		action;
		[return int;]
	}

## 注意:
	1.必须在调用函数地方之前，先声明函数，shell脚本是逐行运行。不会像其它语言一样先预编译
	2.函数返回值，只能通过$? 系统变量获得，可以显示加：return 返回，
		如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)
		
# 练习实例

![TIM截图20181024204918.png](https://upload-images.jianshu.io/upload_images/14477271-ea0895dfc2ba2740.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024204852.png](https://upload-images.jianshu.io/upload_images/14477271-3596d3e684d11b7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024204930.png](https://upload-images.jianshu.io/upload_images/14477271-7353e2eb30a07e28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024205020.png](https://upload-images.jianshu.io/upload_images/14477271-5052ca17809e5cae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![TIM截图20181024205034.png](https://upload-images.jianshu.io/upload_images/14477271-29f65dba1f2421ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


