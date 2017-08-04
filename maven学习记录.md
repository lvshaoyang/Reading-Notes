2017/6/27 13:53:10 
参考网址：[http://www.yiibai.com/maven/](http://www.yiibai.com/maven/ "Maven学习参考")

	1、怎么理解pom.xml中的groupId和artifactId?
		groupId和artifactId被统称为“坐标”，是为了保证项目的唯一性。
		groupId一般可分为多个段，第一段为域，第二段为公司名称。域又分为org,com,cn,其中org为非盈利性，
		com为商业性。例如：tomcat项目，这个项目groupId为org.apache,公司名称是apache，artifactId是tomcat.
		GroupID 是项目组织唯一的标识符，实际对应JAVA的包的结构
		ArtifactID是项目的唯一的标识符，实际对应项目的名称，就是项目根目录的名称。
		version 
		指定了myapp项目的当前版本，SNAPSHOT意为快照，说明该项目还处于开发中，是不稳定的版本。 

	2、何为mave坐标
		maven的世界中拥有数量非常巨大的构件，也就是平时用的一些jar,war等文件。
		maven定义了这样一组规则： 
		世界上任何一个构件都可以使用Maven坐标唯一标志，maven坐标的元素包括groupId, artifactId, version，package，classifier。 
		只要在pom.xml文件中配置好dependancy的groupId，artifact，verison，classifier, 
		maven就会从仓库中寻找相应的构件供我们使用。
		那么，"maven是从哪里下载构件的呢？" 
		答案很简单，maven内置了一个中央仓库的地址（http://repol.maven.org/maven2）
		,该中央仓库包含了世界上大部分流行的开源项目构件，maven会在需要的时候去那里下载
	
	3、如果我新建一个项目，我怎么知道要向pom.xml中加哪些包呢？
		这个问题有点弱了。这说明对jar包很不了解。


Windows7上安装maven
	
	安装jdk1.8,
	从apache官网上下载maven，***.bin.zip下载，解压到一个文件夹下，
	设置环境变量M2_HOME,MAVEN_HOME指向MAVEN文件夹，例如：M2_HOME=E:\Maven\apache-maven-3.5.0，在PATH中添加变量，%M2_HOME%\bin
	打开DOS窗口，输入mvn -v即显示版本安装成功。
	
更换本地仓库位置
	
	修改{M2_HOME}\conf\setting.xml文件
	<!-- localRepository
	The path to the local repository maven will use to store artifacts.
	Default: ${user.home}/.m2/repository
	<localRepository>/path/to/local/repo</localRepository>
  	-->
	<localRepository>E:\Maven\repository</localRepository><!--添加本地仓库-->
	修改完位置后，可以在DOS窗口中，新建一个maven项目了
	使用命令：
	mvn archetype:generate -DgroupId=com.yibai -DartifactedId=NumberGenerator -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false;
	执行完后，可以看到依据快照在本地修改完的仓库中下载了好多jar包。
	由于我是在c:\Users\adm下执行的上述命令，而我上面的命令是建立一个项目，因此在User/adm下多了一个文件夹，NumberGenerator文件夹，目录结构是严格的maven的目录结构，根目录NumberGenerator下面有src和pom.xml
	src下有main和test两个文件夹，main和test下面分别是java文件夹，下面放的是相应的包名，即groupId对应的
	com文件夹，yibai文件夹下放的是*.java文件.

Maven中的项目模板并且创建Java项目：		
	
	maven 使用 Archetype 概念为用户提供不同类型的项目模板，它是一个非常大的列表（614个数字）。 maven 使用下面的命令来帮助用户快速开始构建一个新的 Java 项目。
	mvn archetype:generate
	那么什么是Archetype呢？
	Archetype 是一个 Maven 插件，其任务是按照其模板来创建一个项目结构。在这里我们将使用 quickstart 原型插件来创建一个简单的 Java应用程序
	（例如更换本地仓库位置中的mvn命令一样）
	使用mvn archetype:generate命令的话得按提示，输入groupId,artifactId,模板的话，直接使用了quictstart,可能跟我本地库有关。不跟我已经有了的本地库有关，删除后，
	再执行mvn archetype:generate的话，又建了一个repositpory
	默认还是选了（203：maven-archetype-quickstart）,只是创建了一个java项目，不是web项目。
	
	这个只是创建了项目，有了java代码，并且pom.xml中有junit3.8测试包。
	
	使用Maven构建和测试Java项目：
	进入到项目的根路径下，E:\Maven\consumerBanking在DOS命令中执行：
		mvn clean package
		但是提示错误：No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?
		因为我的path变量中没有添加jdk,在path环境变量中添加C:\Java\jdk1.8.0_131\bin有了java和javac了。
		虽然java命令和javac命令都没有问题，但是mvn clean还是不能编译，出现上述错误：根本原因就是没有JAVA_HOME,只要在系统变量中添加一个JAVA_HOME=C:\Java\jdk1.8.0_131指向jdk就可以了，
		如图：
		![](http://www.yiibai.com/uploads/tutorial/20151125/1-151125203211622.png)
	介绍一下命令：	
		现在我们已经建立项目，并创建生成最终 jar 文件，以下是主要一些概念介绍：

		我们给 maven 两个目标，首先要清理目标目录（clean），然后打包项目生成 JAR（包）输出 ；
		
		打包的 JAR 可在 consumerBanking\target 文件夹找到文件：consumerBanking-1.0-SNAPSHOT.jar ；
		
		测试报告在 consumerBanking\target\surefire-reports 文件夹中；
		
		Maven 编译的源代码文件，然后测试源代码文件；
		
		然后 Maven 运行测试用例；
		
		最后 Maven 创建包；

		可以使用java命令直接运行class文件。并且打包consumerBanking-1.0-SNAPSHOT.jar包。
		如果想要添加自己的java那么在根目录下新加类，并且修改java文件进行调用，
		当然还要重新编译：
		命令为：mvn clean compile
		先清除，再编译

Maven创建一个Java Web项目（Spring MVC）：
	
	1、从Maven模板创建Web项目	
		您可以通过使用Maven的maven-archetype-webapp模板来创建一个快速启动Java Web应用程序的项目。在终端(* UNIX或Mac)或命令提示符(Windows)中，导航至您想要创建项目的文件夹
		使用命令：
		mvn archetype:generate -DgroupId=com.yibai -DartifactId=CounterWebApp -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false

		创建了如图所示的目录结构：
			.|____CounterWebApp
			||____pom.xml
			||____src
			|||____main
			||||____resources
			||||____webapp
			|||||____index.jsp
			|||||____WEB-INF
			||||||____web.xml
			没有java包这时候。

	那么怎么把它导入到Eclise中呢？这也是我最关心的。
		要导入这个项目到Eclipse中，需要生成一些 Eclipse 项目的配置文件：

		3.1、在终端，进入到 “CounterWebApp” 文件夹中，键入以下命令
			mvn eclipse:eclipse -Dwtpversion=2.0
			执行完命令后多了.project,.classpath文件和.settings文件夹

			注意，此选项 -Dwtpversion=2.0 告诉 Maven 将项目转换到 Eclipse 的 Web 项目(WAR)，而不是默认的Java项目(JAR)。为方便起见，以后我们会告诉你如何配置 pom.xml 中的这个 WTP 选项。
		3.2 导入到 Eclipse IDE – File -> Import… -> General -> Existing Projects into workspace.project structure
		图像说明: 在 Eclipse 中，如果看到项目顶部有地球图标，意味着这是一个 Web 项目。
		4、更新POM
		在Maven中，Web项目的设置都通过这个单一的pom.xml文件配置。

		添加项目依赖 - Spring, logback 和 JUnit
		添加插件来配置项目
		重新执行mvn eclipse:eclipse不行没有出现相应的结构目录。
	
		2017/6/28 9:20:23 
		如下：
		.
		|____pom.xml
		|____src
		| |____main
		| | |____java
		| | | |____com
		| | | | |____yiibai
		| | | | | |____controller
		| | | | | | |____BaseController.java
		| | |____resources
		| | | |____logback.xml
		| | |____webapp
		| | | |____WEB-INF
		| | | | |____mvc-dispatcher-servlet.xml
		| | | | |____pages
		| | | | | |____index.jsp
		| | | | |____web.xml
		问题是：虽然按照网站中的教程执行了，但是依然没有java目录，等一些相关的文件。
		百度一下：有这样一句话：
		使用maven原生态命令生成的web app没有java源文件夹, 要自己手动添加！！！！
		2017/6/28 13:49:58 
		问题依然是：这个项目怎么导入到eclipse中去，并且部署到相应的tomcat下，并且运行？？
		
Eclispe安装m2eclipse插件用来集成Maven
	
		点击eclipse菜单栏Help->Eclipse Marketplace搜索到插件Maven Integration for Eclipse 并点击安装即可。

问题来了：那么使用Maven创建项目是使用原生的命令呢，还是使用Eclipse等IDE呢？
		
		感觉还是得用maven创建项目。
		但是依然也要导入一个Maven项目，那么导入的话步骤是什么呢？？

重点：使用Eclipse创建java web项目！！！
	
		参考网址：http://blog.csdn.net/smilevt/article/details/8215558/
		自己在创建时候，因为使用这个模板并没有创建src/main/java和src/test/java目录，但是我自己新加SourceFolder的时候，报了一个错误：
		The folder is already a source folder
		解决办法：右键build path -> configure build path -> source ，选择 src/main/java、src/test/java删除，然后再新建。在buildpath中看到了这两个目录状态为missing.
		然后在pom.xml中添加依赖，然后maven--update project，然后相应的依赖的jar包就搞到中央仓库了。

2017/8/4 9:27:29 

#关于maven web项目的目录组织结构#
参考下图：

![](http://i.imgur.com/jIkh5wR.jpg)

	
	