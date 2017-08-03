## 使用IDEA创建maven项目并保存到git上 ##

#### 一部分、创建maven web项目 ####
	
	Idea版本为2017.2
	一、file--new--project--maven---勾选create from archetype选中webapp
	如图：

![](http://i.imgur.com/ASLn8qJ.jpg)
	
	填写groupId和artifactId
	如图：

![](http://i.imgur.com/wPIdf1d.png)
	
	然后一直next完成即可。
	
	还有一步设置maven选项的，可以选择本地安装的maven.点击override即可，选择，我公司电脑上安装路径，选择setting.xml
	E:\Maven\apache-maven-3.5.0\conf＼settings.xml
	repository也选择本地配置的仓库
	
	二、此时已是maven的webapp项目了，需要在右侧的Maven project中刷新一下，加载pom中的配置（很重要）。如下图：
	
![](http://i.imgur.com/eIjSp2N.png)
	
	其他到没有什么了！！。

####二部分、 配置Intellij idea中的Git/ GitHub ####

	1、打开Settings-- Version Control--Git。下拉选择Github，填写Host、Login和Password，然后Test是否成功。版本为2.13
	如图：
![](http://i.imgur.com/53sQnXi.png)


	2、创建本地仓库。感觉相当于git bash here命令。
	如图：
![](http://i.imgur.com/wNHbpsv.png)
![](http://i.imgur.com/PA6pX2S.png)
	
	3、提交代码到git，如下图：
![](http://i.imgur.com/vlkR0ao.png)
	
	右键点击src或代码文件，Git -- Add -- Commit（先Add再Commit）
	
	4、项目远程关联和提交。
	在Windows中打开命令行窗口cmd，转到当前项目所在目录，例如： cd "E:\ideaWebProject\hotelmaster"（假定当前项目名称为hotelmaster）；
	
	在此路径下关联远程github，但是提示没有git命令，这个时候就要在环境变量中添加路径以便能git可执行文件能访问。
	
	说明：在进行下面的操作之前，必须设置Path环境变量，使得Git可执行文件能访问，如：;D:\Program Files\Git\bin,如图：
![](http://i.imgur.com/Ka4csg6.png)
	
	5、输入命令：git remote add origin git@github.com:lvshaoyang/hotelmaster.git
	6、再输入命令：git push origin master
	7、然后回到Intellij IDEA环境中在项目上单击右键，选择同步当前项目菜单：Synchronize 'TestProject'；
	8、再次操作Intellij IDEA，在项目上单击右键选择Git相关操作：Git -> Repository -> Branches -> origin/master -> Checkout as new local branch
	
	到此你就可以使用Intellij IDEA的Git插件将本地与远程仓库中的代码进行pull/push的操作了。
	

###### 备注：一直找不到Git在mac上的安装位置，可以在命令行中输入：
######     which git，就会显示git的安装位置了。但是mysql不行。

### idea从电脑上自身检出github上的项目
    
    因为已经配置过了git，参考步骤1.在idea上配置github，即配置上自己的注册的帐户。如下图：
![](http://i.imgur.com/7Tf107q.png)

    
    然后就是VCS-Checkout from Version Control -Github,然后点击clone即可获取。
    记住一定符合maven项目的结构目录：
###### 问题：idea在文件 夹下不能新建Class文件？
###### 要将目录右键：Make Directory as SourceRoot才可以新建。class和interface
    
问题：
    在idea这个IDE中，maven web项目的文件组织结构是什么呢？？

## 2017/8/3 9:24:32  ##

	
	
	遇到了新问题：
	
	昨天使用第二部分的配置步骤在电脑上新建了idea中的maven web项目，并且上传到了github,在家里的电脑上使用github检索下来，并且使用idea对项目进行了编辑，添加了类等。但是今天在idea中使用git--repository--pull时，却一直提示pull failed,然后根据晚上的提示，把报错的两个.iml文件删除了，然后项目又没有了。配置了github又重新从github上检了一遍。
	
    
	