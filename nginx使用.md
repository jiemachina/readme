Ubuntu下安装Nginx

	sudo apt-get install nigx
	
启动

	sudo nginx
	
在浏览器中输入

	http://localhost:80/
出现	Welcome to nginx! 安装成功

重启 

	sudo service nginx  restart
停止

	sudo service nginx  stop

修改默认静态文件目录，查看/etc/nginx/nginx.conf; 会发现里面有一条`include /etc/nginx/sites-enabled/*;`, 那去`site-enabled`目录看看吧，查看 `/etc/nginx/site-enable/default` 文件；找到 `root /usr/share/nginx/www;` , 去这个目录看看吧,发现有个index.html;那么看看内容发现是’Welcome to nginx!‘

所以修改静态文件夹就是修改root 后面那一串自己的数字就可以了

如果像浏览自己的文件目录

	location /a/ {
         autoindex on;
	}

##Nginx配置

[参考博客http://blog.s135.com/post/306](http://blog.s135.com/post/306)