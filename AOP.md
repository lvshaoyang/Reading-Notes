## AOP的实现机制 ##

#### 几个概念的理解 ####

	1、AOP各种的实现：
		编译期
		字节码加载前
		字节码加载后
	2、AOP里的公民：
		Joinpoint:拦截点，如某个业务方法
		Pointcut:Joinpoint的表达式，表示拦截哪些方法。一个Pointcut对应多个Joinpoint。
		Advice:要切入的逻辑。
			Before Advice: 在方法前切入
			After Advice:在方法后切入，抛出异常时也会切入
			After Returing Advice:在方法返回后切入，抛出异常时不会切入
			After Throwing Adviec:在方法抛出异常时切入
			Around Advice:在方法执行前后切入，可以中断或忽略原有流程的执行
		他们之间的关系：
			织入器通过在切面中定义pointcout来搜索目标（被代理类）的JoinPoint(切入点)，然后把要切入的逻辑（Advice）织入到目标对象里，生成代理类。
	动态AOP：
			1、机制--动态代理，原理：在运行期，目标类加载后，为接口动态生成代理类，将切面植入到代理类中。
			2、机制--动态字节码生成，原理：在运行期，目标类加载后，动态构建字节码文件生成目标类的子类，将切面逻辑加入到子类中。
			3、机制--自定义类加载器，原理：在运行期，目标加载前，将切面逻辑加到目标字节码里。
			4、机制--字节码转换，原理：在运行期，所有类加载器加载字节码前，进行拦截。
			如图：
![](http://i.imgur.com/nFemSLz.png)
			
		
		如图：
![](http://i.imgur.com/Ox5QotR.png)
	
#### 问题：到底啥是代理？为什么要用代理？到底代理了谁？针对jdk本身的动态代理 ####
	
	为接口生成代理？？
	
	参考网址：https://www.ibm.com/developerworks/cn/java/j-lo-proxy1/
	Java动态代理机制的出现，使得Java开发人员不用手工编写代理类，
	只要简单地指定一组接口及委托类对象，便能动态地获得代理类。
	代理类会负责将所有的方法调用分派到委托对象上反射执行
	
	代理：设计模式
	是一种常用的设计模式，其目的就是为其他对象提供一个代理以控制对某个对象的访问。代理类负责为委托类预处理消息，
	过滤消息并转发消息，及进行消息被委托类执行后的后续处理。