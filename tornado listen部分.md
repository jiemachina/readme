在上一篇文章中简单介绍了tornado是异步非阻塞是web服务；那从main方法中很简单的看到只有2行代码；
这篇就简单的翻译和理解一下listen(8888)这段代码；主要是在[httpserver.py](https://github.com/facebook/tornado/blob/master/tornado/httpserver.py)中；
看的
		
		commit: 03d41df74711f98e8a6785a53e389077389e6924

首先是一个非阻塞，单线程的HTTPSever.
这个Server定义一个reqeust callback, 也就说，在Tornado中这个reqeuset`不仅是`Request，还有担当着Response的作用；

中间有一大段来说Http的一些基本头信息； 比如长连接(keep-alive)，安全(ssl);

其实在httpserver.py是看不到listen实现的；主要实现是在[tcpserver.py](https://github.com/facebook/tornado/blob/master/tornado/tcpserver.py)


        import tornado.httpserver
        import tornado.ioloop

        def handle_request(request):
           message = "You requested %s\n" % request.uri
           request.write("HTTP/1.1 200 OK\r\nContent-Length: %d\r\n\r\n%s" % (
                         len(message), message))
           request.finish()

        http_server = tornado.httpserver.HTTPServer(handle_request)
        http_server.listen(8888)
        tornado.ioloop.IOLoop.instance().start()
     
  1. `~tornado.tcpserver.TCPServer.listen`: simple single-process::

            server = HTTPServer(app)
            server.listen(8888)
            IOLoop.instance().start()

       In many cases, `tornado.web.Application.listen` can be used to avoid
       the need to explicitly create the `HTTPServer`.

  2. `~tornado.tcpserver.TCPServer.bind`/`~tornado.tcpserver.TCPServer.start`:
       simple multi-process::

            server = HTTPServer(app)
            server.bind(8888)
            server.start(0)  # Forks multiple sub-processes
            IOLoop.instance().start()

       When using this interface, an `.IOLoop` must *not* be passed
       to the `HTTPServer` constructor.  `~.TCPServer.start` will always start
       the server on the default singleton `.IOLoop`.

  3. `~tornado.tcpserver.TCPServer.add_sockets`: advanced multi-process::

            sockets = tornado.netutil.bind_sockets(8888)
            tornado.process.fork_processes(0)
            server = HTTPServer(app)
            server.add_sockets(sockets)
            IOLoop.instance().start()

       The `~.TCPServer.add_sockets` interface is more complicated,
       but it can be used with `tornado.process.fork_processes` to
       give you more flexibility in when the fork happens.
       `~.TCPServer.add_sockets` can also be used in single-process
       servers if you want to create your listening sockets in some
       way other than `tornado.netutil.bind_sockets`.

