# jar包和war包的介绍和区别  #

	JavaSE程序可以打包成Jar包(J其实可以理解为Java了)，
	而JavaWeb程序可以打包成war包(w其实可以理解为Web了)。
	然后把war发布到Tomcat的webapps目录下，Tomcat会在启动时自动解压war包。
	
	一个WAR文件就是一个Web应用程序，建立WAR文件，
	就是把整个Web应用程序（不包括Web应用程序层次结构的根目录）压缩起来，指定一个.war扩展名