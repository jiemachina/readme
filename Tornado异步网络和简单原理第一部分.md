[官网](http://www.tornadoweb.org/en/stable/)第一段话：
>
Tornado is a Python web framework and `asynchronous` networking library, originally developed at FriendFeed. By using non-blocking network I/O, Tornado can scale to tens of thousands of open connections, making it ideal for long polling, WebSockets, and other applications that require a long-lived connection to each user.

翻译理解一下：Tornado是一个python实现的web框架，是`异步网络请求包`，起源于friendFeed; 是`无阻塞网络I/O`, Tornado可以很好处理成千上万的连接（C10K）；处理轮询，webSockets和其他应用长连接等请求

个人理解异步网络等同于无阻塞网络

# 理解异步网络请求

在官网中提供的一个Hello world的这个Demo中，是`不支持异步`的，只有在使用`@tornado.web.asynchronous`这个装饰器的时候才能算的上真正异步；参考[博客](http://www.dongwm.com/archives/shi-yong-tornadorang-ni-de-qing-qiu-yi-bu-fei-zu-sai/)

在blog中还有一个装饰`@tornado.gen.coroutine`这个装饰器是干什么的呢?? 查看tornado源码在gen.py中
For example, the following asynchronous handler::   

                                                                                                            
  	class AsyncHandler(RequestHandler):                                             
        @asynchronous                                                                                                                                       
        def get(self):                                                              
            http_client = AsyncHTTPClient()                                         
            http_client.fetch("http://example.com",                                 
                              callback=self.on_fetch)                               
                                                                                    
        def on_fetch(self, response):                                               
            do_something_with_response(response)                                    
            self.render("template.html")
            self.finish()
           

从上面的代码可以看出在使用`@asynchronous`的时候，而且需要注意的是 Tornado默认在函数处理返回时关闭客户端的连接,但是当你使用@tornado.web.asynchonous装饰器时，Tornado永远不会自己关闭连接，需要显式的`self.finish()`关闭;每次需要一个显示命名一个callBack方法，为了`省略`写这个方法和
`self.finish()`，

就有如下的代码:

###Future方式：###


	class GenAsyncHandler(RequestHandler):                                          
        @asynchronous                                                               
        @gen.coroutine                                                              
        def get(self):                                                              
            http_client = AsyncHTTPClient()                                         
            response = yield http_client.fetch("http://example.com")                
            do_something_with_response(response)                                    
            self.render("template.html") 
 
###Task方式： ###

	class GenAsyncHandler2(RequestHandler):                                      
        @asynchronous                                                            
        @gen.coroutine                                                           
        def get(self):                                                           
            http_client = AsyncHTTPClient()                                      
            http_client.fetch("http://example.com", callback=(yield gen.Callback("key")) # 1                                                                                        
            response = yield gen.Wait("key")                                             # 2
            do_something_with_response(response)                                 
            self.render("template.html")


并且`@gen.coroutine`和`yield`都是配对使用的


###Task方式改良版：###

细看gen.py注释文档我们会发现还有一种方式可以省略装饰器`@asynchronous`和简化#1和#2代码, 使用gen.Task

	@gen.coroutine                                                               
    def get(self):                                                               
         yield gen.Task(AsyncHTTPClient().fetch, "http://example.com")#替换上面的#1和#2



###一次异步多个请求，适用于Future和Task版， 以下是Future版本###

	@gen.coroutine                                                               
    def get(self):                                                               
        http_client = AsyncHTTPClient()                                          
        response1, response2 = yield [http_client.fetch(url1),                   
                                      http_client.fetch(url2)] 


## 初看tornado运作

参考官网Hello world例子

	import tornado.ioloop
	import tornado.web

	class MainHandler(tornado.web.RequestHandler):
    	def get(self):
        	self.write("Hello, world")

	application = tornado.web.Application([
    	(r"/", MainHandler),
	])

	if __name__ == "__main__":
    	application.listen(8888) 
    	tornado.ioloop.IOLoop.instance().start()

从这个例子里面我们可以看出导入是`ioloop`和 `web`两个模块

	application.listen(8888)# socket 启动Socket，这里并没有阻塞
	tornado.ioloop.IOLoop.instance().start() # 事件处理启动， 处理上面接收到的url

import signal: 信号机制  
import posix:Python 有许多使用了 POSIX 标准 API 和标准 C 语言库的模块. 它们为底层操作系统提供了平台独立的接口

web.py.listen() > httpserver.py > tcpserver.py  
HTTPServer的父类是TCPServer;

在httpserver.py中的注释可以看到，HTTPServer是非阻塞

