## Python yield 使用浅析

[转载http://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/](http://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/)

以下是理解归纳

###如何生成斐波那契數列

	def fab(max): 
    	n, a, b = 0, 0, 1 
    	while n < max: 
        	print b 
        	a, b = b, a + b 
        	n = n + 1
        		
        		
 执行
 
	fab(5)
    
问题是print太多了

### 解决问题，用list一次打出来

	def fab(max): 
    	n, a, b = 0, 0, 1 
    	L = [] 
    	while n < max: 
        	L.append(b) 
        	a, b = b, a + b 
        	n = n + 1 
    	return L
 
 
问题是如果max很大，那么L就很大，占内存


### 解决占内存问题（通过 iterable 对象来迭代）


	class Fab(object): 

    	def __init__(self, max): 
        	self.max = max 
        	self.n, self.a, self.b = 0, 0, 1 

    	def __iter__(self): 
       		return self 

    	def next(self): 
        	if self.n < self.max: 
            	r = self.b 
            	self.a, self.b = self.b, self.a + self.b 
            	self.n = self.n + 1 
       			return r 
      		raise StopIteration()
 
 执行
 
 	       
        for n in Fab(5):
        	print n
 
 iterable每次只占next的时候的内存
 
 问题：感觉代码太多，没有第一版的 fab 函数来得简洁
 
### 使用yield，代码就简洁了


	def fab(max): 
    	n, a, b = 0, 0, 1 
    	while n < max: 
        	yield b 
        	# print b 
        	a, b = b, a + b 
        	n = n + 1
        
执行
	
	for n in fab(5):
		print n 	
        
        

###参考博客文字

>简单地讲，yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator，调用 fab(5) 不会执行 fab 函数，而是返回一个 iterable 对象！在 for 循环执行时，每次循环都会执行 fab 函数内部的代码，执行到 yield b 时，fab 函数就返回一个迭代值，下次迭代时，代码从 yield b 的下一条语句继续执行，而函数的本地变量看起来和上次中断执行前是完全一样的，于是函数继续执行，直到再次遇到 yield。
        
## 其他归纳

(iterable和generator)一般不会手动的调用next方法，而使用for循环，如果一直使用next，当到最后一个没有的时候，就会抛异常；  

有yeild的时候，因为yeild相当于另外的返回值，不能直接return具体值(不过在3.0之后的python就可以在yeild之前return一个值)，会抛`SyntaxError: 'return' with argument inside generator `异常

        
        
        
        
        
        
        
        
        
        
        