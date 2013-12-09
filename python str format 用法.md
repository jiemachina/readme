
# python 常用收集

# python str format()

有次逛oschina看到：

	>>>s = '1234567.78'
	"{:,}".format(float(s))
	>>>1,234,567.78

这段代码，所以看看format不仅仅是在使用连接字符串这么简单的使用还有其他更好玩的用法；

* tuple，数组参数
* dict参数
* 对象属性传参
* %s 和 %r的使用
* 文字左右对齐，填充
* %x 和%o 十进制，八进制转换使用
* 格式化日期（一般使用时间格式化我们基本都在用time.strptime或者time.strftime）


{:特殊位}， 简单的使用是不需要特殊位；但是有了特殊位就就有一些高级的东西；看下语法

	
	format_spec ::=  [[fill]align][sign][#][0][width][,][.precision][type]
	
看上面这语法的意思就是说特殊位，有[左右填充][正负符号位][,][小数点保留][type]等等都是下列类型,这些类型可以单独使用，也可以组合使用；


	fill        ::=  <any character> #普通字符
	align       ::=  "<" | ">" | "=" | "^" #位置
	sign        ::=  "+" | "-" | " " # 数字的符号位
	width       ::=  integer
	precision   ::=  integer
	type        ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%" # 各种输出类型，比如字符，整数，指数，8进制，16进制，还有百分比


[参考1](http://docs.python.org/2/library/string.html#format-examples)  
[参考2](http://blog.csdn.net/xiaofeng_yan/article/details/6648493)

