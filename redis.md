#安装Redis

[下载](http://www.redis.io/download)


	$ wget http://download.redis.io/releases/redis-2.8.2.tar.gz
	$ tar xzf redis-2.8.2.tar.gz
	$ cd redis-2.8.2
	$ make
	
启动服务：

	$ src/redis-server

启动客户端：
	
	$ src/redis-cli
	
为了方便，建议把src目录配置到系统的PATH目录下


# [在线教程指导](http://try.redis.io/)


看到一个很简单的界面；从手册指导了解，下面输入：

	》tutorial

这样就很快看到教程了

## tutorial第一页

插入数据：
	
	》set server:name 'fido'

取数据

	》get server:name


## Next

右下角有个Next

	》Next

删除: del 和 增加: incr

	>SET connections 10
    >INCR connections => 11
    >INCR connections => 12
    >DEL connections
    >INCR connections => 1
    
数据过期设置(EXPIRE)和剩余时间(ttl)：

	SET resource:lock "Redis Demo"
    EXPIRE resource:lock 120 # 返回1代表设置缓存时间成功
    TTL resource:lock
    TTL count
    

list操作

	rpush friends 'alice' # 在list尾部添加
	rpush friends 'Bob' 
	lpush friends 'sam' # 在list的头部添加
	lrange friends 0 -1 # 全部
	lrange friends 0 1 #["Sam","Alice"]
	LRANGE friends 1 2 => ["Alice","Bob"]

	LLEN friends => 3 # 长度
	LPOP friends => "Sam" # 左边弹出，最后一个
	RPOP friends => "Bob" #右边弹出，第一个
	
set 操作

	SADD superpowers "flight"
    SADD superpowers "x-ray vision"
    SADD superpowers "reflexes" # 添加
	SREM superpowers "reflexes" # 删除
	SISMEMBER superpowers "flight" => true # 判断是否存在
    SISMEMBER superpowers "reflexes" => false # 判断是否存在
    SMEMBERS superpowers # 以list的形式显示set
    
    # 集合合并操作
    SADD birdpowers "pecking"
    SADD birdpowers "flight"
    SUNION superpowers birdpowers => ["flight","x-ray vision","pecking"]

排序集合

	ZADD hackers 1940 "Alan Kay"
    ZADD hackers 1906 "Grace Hopper"
    ZADD hackers 1953 "Richard Stallman"
    ZADD hackers 1965 "Yukihiro Matsumoto"
    ZADD hackers 1916 "Claude Shannon"
    ZADD hackers 1969 "Linus Torvalds"
    ZADD hackers 1957 "Sophie Wilson"
    ZADD hackers 1912 "Alan Turing"
    
    ZRANGE hackers 2 4 => ["Claude Shannon", "Alan Kay","Richard Stallman"]
	
更对学习

案例学习：http://redis.io/topics/twitter-clone
客户端简命令：http://redis.io/topics/data-types-intro

好了，可以慢慢学习Redis了

