# Tomcat 性能调优之 JVM 调优 #

参考网址：
http://zhuanlan.51cto.com/art/201707/545721.htm

Tomcat、Jetty、GlassFish 等等这系列 Web容器/应用服务器，虽然做为容器，提供的是一个 Java Web 的运行时环境，以支持Servlet/JSP 等等这些内容的运行，但我们都很清楚，其本质上还是一个 Java 应用程序。 每次对于 容器的启动运行，都是把这个 Java 程序跑起来，来实现 Web 容器的能力。
做为一类“特殊”的 Java 应用程序，和任务其他的 Java 应用一样，需要使用到JVM，会有堆，会使用到垃圾回收，会涉及到不同的堆分区比例...


#### tomcat就是一个java应用程序。用了socket网络编程的java程序。 ####