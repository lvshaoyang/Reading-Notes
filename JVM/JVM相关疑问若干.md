## JVM到底是什么？ ##

## JVM到底要学习什么？ ##

	
	参考网址：http://developer.51cto.com/art/201112/305830.htm

	JVM全称JAVA virtual machine.
	
	当启动一个Java程序时，一个JVM实例就产生了，任何一个拥有main()函数的class都可以作为JVM实例运行的起点。
	
	JVM实例的运行：main()作为该程序初始线程的起点，任何其他线程均由该线程启动。
	
	-JVM的体系结构
	粗略来分：JVM的内部体系结构分为三部分，分别为类装载器（ClassLoader）子系统，运行时数据区和执行引擎。
	
	问题：JVM中多次提到的本地方法是什么？
	JIT又是什么？即时编译（JIT）
	
	运行时数据区：
	  方法区和堆由所有线程共享
		堆:存放所有程序在运行时创建的对象。
		
		方法区：当JVM的类装载器加载.class文件，并进行解析，把解析的类型信息放入方法区（难道对反射有用？）
	   Java栈和PC寄存器由线程独享，在新线程创建时间里。
	   本地方法栈：存储本地方法调用的状态。
	   
	static变量作为类型信息的一部分保存，这样说来，这个变量是存储在方法区喽？
	
	本地方法栈：依赖于本地方法的实现，如某个JVM实现的本地方法借口使用C连接模型，则本地方法栈就是C栈，可 以说某线程在调用本地方法时，就进入了一个不受JVM限制的领域。
	
#### JAVAEE和JVM有啥关系？？  ####
问题来了，j2se和j2ee到底是啥？？

参考网址：http://www.cnblogs.com/pugang/p/4619912.html

	Java编程语言一共有四个官方平台，Java SE，
	Java EE Java ME JavaFX。
	所有的Java平台都由一个JVM和一组应用程序编程接口组成。JVM是一个程序，一般以C/C++编写。
	Java SE:
	Java SE API提供Java编程语言的核心功能，定义了Java编程语言的一切从基本的类型和对象到更高级的被用于网络和安全及数据库的类，GUI,XML等。
	
	Java EE:
	Java EE平台构建于Java SE平台之上，JavaEE平台提供一组API和运行环境来开发和运行大规模的，多层的可扩展的，可靠的和安全的网络应用程序。
	其核心在于网络

2017/7/24 9:06:00 
《JAVA虚拟机精读》
	
	1、Java之所以能够实现“一次编译，处处运行”（Write Once,Run Anywhere），首先就是因为字节码。虽然不同平台的Java虚拟机的内部实现机制不尽相同，但是它们共同解释出的字节码却是一样的。因为编译是javac命令，是虚拟机来完成 的?
	2、Java基础中的Java EE规范中的RMI(Java Remote Method Invocation，Java远程方法调用)、JMS（Java Message Service,Java消息服务）等技术.
	3、机器指令:可以被计算机CPU直接进行识别并执行,其表现形式为二进制.机器指令通常由操作码和操作数两部分组成,操作码负责指令需要完成的操作,操作数负责数据运算,以及运算结果所存放的位置等.
	4、Java技术的核心就是Java虚拟机，因为所有Java程序都运行在Java虚拟机内部。

2017/8/1 11:54:52 
	
## JVM之类装载器 ##
	
	重要的参考网址：
	http://qinbin.blog.51cto.com/11773640/1951312
	此网址是一个博客，里面详细的介绍了JVM。
	
	
![](http://i.imgur.com/mqCZnRe.png)