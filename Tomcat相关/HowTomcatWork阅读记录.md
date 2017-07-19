Chapter 1   
        
		1、在使用Response类时，在请求index.html时，浏览器一直提示无响应？
		是因为在Response中使用socket获取outputStream在out.write(string.getBytes())时，虽然根据路径获取了文件，并且也读取了文件内容，但是在其调用write方法时，应该先输出响应的头部，否则浏览器不会识别这个响应。
	添加如下代码
	`public static final String SUCCESS_MESSAGE =
	 "HTTP/1.1 200 OK\r\n"
			+"\r\n"
			;`
	一定要加上这些
	HTTP响应包括的三部分：
		协议-状态码-描述
		响应头
		响应实体段
	以上的读取的文件内容为响应实体，响应头可以为空，但是要使用\r\n隔开。

2、Socket的作用

    一旦成功创建了Socket实例，那么就可以使用该实例发送或接收字节流。要发送字节流可以调用Socket类的getOutputStream()方法，或取OutputStream对象，如果发送文本，通常本根据OutputStream对象创建PrintWriter对象。如果想从连接的另一端接收字节流，需要调用Socket类的getInputStream方法（）。

Chapter 2
    
    可以好好学习一下Servlet了。
    2017-06-28
    1、在eclipse中建立了一个JAVA项目，但是在学习第二章时，要建立一个继承HttpServerletRequest的类，但是这个类在j2ee包中，那么怎么引入这个相应的jar包呢？
    解决办法：从maven的中央仓库下载一个javaee的jar包。
    https://mvnrepository.com
    2、在写Request类时，使用input获取输入流，即HTTP请求，在使用read(byte[] b)方法时，设置了byte数组最长为2048，那么HTTP请求规定了长度没有呢？
    HTTP协议并没有规定请求长度有限制，但是浏览器却有。另外web服务器也会对请求长度有所限制。
时间：2017-06-30
    
    问题1：
     在处理Servlet时需要一个类载入器，而我不是太懂类载入器。
     问题2：
        eclispe中有时候就是找不到class文件 ，那么这个class的文件放置有什么讲究没有呢？在eclispe中怎么指定输出class文件的位置呢。
时间：2017-07-03
    
1、错误记录：
        
        Exception in thread "main" java.lang.ClassFormatError: Absent Code attribute in method that is not native or abstract in class file
    网上答案一：是运行时环境与编译时环境版本不对应的问题所导致的，要求是运行时环境的版本要大于等于编译时环境的版本。不过没有验证。
    网上答案二：
    参考网址：
    http://blog.csdn.net/beyond667/article/details/8366010
    这个网址不错，应该是我添加的javaee-api-6.0.jar包引起的。答案 ：
    When these artifacts were published to the Maven repository, Sun stripped out the code from classes that are classified as"implementations". So all the interfaces are code complete, yet any abstract class or implementation class has no code in it. Any attempt to ： class will likely blow up. That's why Arquillian is failing.
    
2、ServletResponse的输出流到底是输出到哪的？？
    
        有这样一句代码 
        PrintWriter out = response.getWriter();
        out.write("hello world");
        但是这一句话没有输出到浏览器？？这个是一个继承了Servlet的类，重写了service方法。但是没有浏览器输出。
3、URLClassLoader loader = null;
    
        此类中loader.loadClass(className);
        此处的className为类的全包名。
2017-07-04
问题1：
    
    请求一次servlet竟然调用了两次service（）方法？为什么 呢？？ 这个问题还不是太知道。
    原因1:关于调用servlet时,调用其service()方法，使用response获取PrinterWriter对象，out.print(String s)时，页面提示locallhost无效响应，是因为使用的这个javaee包里的response应该是没有封装响应头信息：
    响应头："HTTP/1.1 200 OK\r\n"
			+"\r\n"
	和前面的静态资源输出一样的错误 。
	应该是javaee包有关。在使用PrinterWriter输出对象前，应该先输出响应头，表示我要输出了。浏览器准备接收。
	原因2：当你也输出了响应头，但是浏览器 还是提示localhost无响应，这个时候要调用 ，PrintWriter的flush()方法，不然页面还是没有任何内容。

程序2：给了一个Request和Response两个外观类，但是我还是不太明白 这样有什么用？、
第三章：
    
    1.连接器总是会提供HttpServletRequest和HttpServletResponse实例。
    2.连接器可能做了HTTP解析的工作。
    3.单例模式的话：这个类的构造函数需要是私有的，外部就不能通过关键字new来实例化他们了。
	
2017-07-11
    
    又是啥也没看
2017-07-13
    
    1、HttpRequest中需要连接器解析的值包括：URI、查询字符串、参数、Cookie、和其它一些请求头信息。
    2、问题：java中的Socket是ServerSocket什么时候创建的呢？ServerSocket这个服务器套接字只能等待HTTP请求吗？那么qq这个请求怎么弄的？
    3、eclipse出现问题：一直卡在wait "populate auto detected configs"?去stack overflow找答案。
    有可能是spring plugin的问题。
    把spring tools那个卸载完就行了。
    参考网址：https://stackoverflow.com/questions/26089282/eclipse-crashes-not-responding-while-populate-auto-detected-configs/45084392#45084392
    4、我的jdk里没有SocketInputStream这个类竟然？？
        答：我去，SocketInputStream竟然是自己定义的。不是jdk里的
        tomcat4之前的版本，org.apache.catalina.connector.http.SocketInputStream是有的。这个类可以在%tocmat%/lib/catalina.jar中找到，把这个jar导入进来即可。tomcat6以后，org.apache.catalina.connector.http.SocketInputStream就被移除了。%tocmat%/lib/catalina.jar中就找不到这个类了，你导入进来这个包当然“怎么也导不进去”SocketInputStream
2017-07-15
    
    问题：关于tomcat是怎么使用jvm的呢？有一个启动类，Bootstrap.java,这个方法中有一个main()方法，通过这个main()方法来启动tomcat。跟j2se的说法一样，就是一个项目只有一个入口那就是main()方法。
2017-07-16（第三章49页）
    
    问：关于HttpRequest和HttpResponse是什么呢?这个问题引起我对java中对象的理解。其实现在还是不太了解或者说不是很清晰的描述java对象是什么呢？
    答：HttpRequest就是一个封装了HTTP请求的并且解析HTTP相关的信息，封装到HttpRequest对象的属性里。解析的话，还是解析字符串，使用的String相关的方法。HttpRequest是使用的相关流来获取字符串，这个流是用Socket对象获取的。
    HttpResponse因为要向页面返回内容，因此要有一个输出流，而这个输出流也是Socket对象获取的。嗯，这又涉及到网络编程了，Socket是怎么知道有一个流并且获取呢。
    问：书里为HttpRequest和HttpResponse都包了一层外观类，HttpRequestFacade和HttpResponseFacade还是不理解有什么用，虽然书中给了解释。
    说明：在servlet/JSP编程中，参数名jsessionid用于携带一个会话标识符。这是为什么有时候在地址栏中会看到一个jseesionid。但是具体用法什么的还不清楚。这个会话标识是谁生成的？还要解析一下设置到HttpRequest中。
    问：在HttpHead中，怎么知道请求头的头和尾，参数和值呢，？没有源码。
    答：null
    问：关于Cookie？Cookie是怎么产生的，谁产生的呢？
    答：Cookie是由浏览器作为HTTP请求头的一部分发送的。但是Cookie是什么时候在哪产生的？
    问：如果用户使用GET方法请求servlet，则所有的参数都会在查询字符串中，那么如果用户使用POST方法请求servlet，那么参数在哪里?怎么解析？
    答：所有的名/值都会存储在一个HashMap对象中。那么，如果用表单POST提交的话，是怎么提交的呢，不像GET那么直观，直接在地址栏中就可以看到。参数可以存在于查询字符串或HTTP请求体中。HTTP请求体是什么玩意？？如果用户使用POST方法提交请求时，请求体会包含参数，则请求头“content-length”的值会大于0，content-type的值为"application/x-www-form-urlencoded"
    问：如果不用IDE工具，但是建立的项目引入了jar包，自己的项目就只引入了一个jar包，那么在windows下只输入java来运行的话，肯定会不能找到某个Class，可以在java命令执行时，引入jar，比如这样：
    java -classpath ./lib/servlet.jar;./ex03.pyrmont.startup.Bootstrap
2017-07-18

Chapter 4
    tomcat的默认连接器
    
    1、tomcat中使用的连接器的功能：
    实现org.apache.catalina.Connector接口
    负责创建实现了org.apache.catalina.Request接口的request对象；
    负责创建实现了org.apache.catalina.Reponse接口的response对象；
    
    
    