## SSH 免密码登陆

* 在~/.ssh/config文件中添加

		Host sname
    	HostName x.x.x.x
    	Port 22
    	User yourname

* 下载ssh-copy-id脚本并设置允许权限
		
		sudo curl "hg.mindrot.org/openssh/raw-file/c746d1a70cfa/contrib/ssh-copy-id" -o /usr/bin/ssh-copy-id
		

* 把本机子的SSH ID 拷贝到目标服务器

		ssh-copy-id x.x.x.x # 默认访问端口是22
		
	或者
	
		ssh-copy-id "-p 3026 yourname@x.x.x.x" # 访问端口不是22的时候

* 连接服务器

		ssh sname

* 省略ssh sname

	每个终端配置的信息都是不一样的；具体查看每个终端第一次打开执行的命令  
	
	例子iterms，
	在iterms --->  preferences ---> General --> send text at start : 输入ssh sname ,搞定， 只要下次新打开item就自动执行ssh sname