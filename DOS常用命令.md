查询端口：

	netstat -ano|findstr "8080"

	netstat -a 显示所有网络连接和端口。
	
	tasklist|findstr "PID号"
	
	netstat -n 可显示已创建的有效连接，并以数字的形式显示本地地址和端口号
	

根据pid杀死进程：
	
	tskill pid  pid要用双引号引起来。
	taskkill /f /t /im soffice.bin
	区别：taskkill比较新，而且可以结束一个或者多个任务或者进程。可以按进程id或图像名结束进程。
	
	TASKKILL [/S system [/U username [/P [password]]]]
	{ [/FI filter] [/PID processid | /IM imagename] } [/F] [/T]
	
	/F:强行终止
	/T: Tree kill:终止指定的进程和任何由此启动的子进程
	/IM image name:指定要终止的进程的映像名称。可使用*通配符。
	例如：TASKKILL /F /IM QQ.exe