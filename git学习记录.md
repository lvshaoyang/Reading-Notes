## 2017/8/3 9:36:47  ##
# Git学习记录 #
参考网址：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000

	理解几个概念：
	
	版本库：版本库又名仓库，英文名repository,可以理解为一个简单目录。这个目录里的所有文件都可以被Git管理起来，每个文件的修改、删除,Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时候还原。linux下在当前目录使用git init命令，会多出个.git文件，windows下可以使用git bash here创建。
	
	.git文件夹：如下图
![](http://i.imgur.com/BUP0vMj.png)
	
	是一个管理git仓库的文件夹，这里包含git操作所需要的东西。是Git的版本库。
	
	问题：如果我把文件直接放到仓库的目录中去不就行了吗？为什么还要执行add和commit命令呢？
	
	命令理解：
	add：主要用于把我们要提交的文件的信息添加到索引库中。
	git status：查看状态
	git log:显示从最近到最远的提交日志
	git reflog:用来记录你的每一次命令

## 重点学习： ##
#### 版本回退 ####
	
	1、使用git log命令查看历史提交记录。显示最近到最远的提交日志。
	如果嫌输出信息太多，使用git log --pretty=oneline
	
	需要友情提示的是，你看到的一大串类似3628164...882e1e0的是commit id（版本号）,是一个SHA1计算出来的一个非常大的数字，用十六进制表示
	
	2、首先，在Git必须知道当前版本是版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^,上上一个版本就是HEAD^^，往上100个版本就是HEAD~100.
	如果要回退到上一个版本的话，可以使用
	git reset --hard HEAD^
	
	3、如果回退完以后，再回去呢？
	使用git reflog查看版本id
	git reset --hard [commit id],只要写版本号的前几位就可以了比如前7位

#### 工作区和暂存区 ####

	前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

	第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

	第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的
![](http://i.imgur.com/Ujud1sC.jpg)
	
	Git的版本库里存了好多东西，其中最重要的就是称为stage（index）的暂存区，还有Git为我们自动创建的第一个分支master,以及指向master的一个指针叫HEAD.
![](http://i.imgur.com/5ts5Kct.jpg)
	
	Git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中
	
#### 删除文件 ####
	
	先把工作区的文件删除，
	然后使用使用命令：git rm 删除版本库中的文件，再使用git commit
	
	另一个情况，版本库里有，但是工作区没有了那么可以使用
	**git checkout**：其实是用版本库里的版本替换工作区的版本。
	
## 远程仓库： ##
	
	使用GitHub帐号，由于你本的Git仓库和GitHub仓库之间的传输是SSH加密的，需要一点设置。
	#### 步骤： ####
	1、创建SSH Key。在用户主目录下，windows下目录C:\Users\adm\.ssh，看是否有.ssh目录 ，如果有再看是否有id_rsa和id_rsa.pub这两个文件。SSH密码保存在id_rsa_pub下，可用nottpad打开 。如果没有怎么创建？
	Windows下打开Gib Bash,
	输入命令：ssh-keygen -t rsa -C "984800179@qq.com"
	2、在GitHub下添加SSH Keys
	
	
	现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得
	
	添加远程库：
	
	首先，登录GitHub，创建一个仓库。
	
	目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
	
	把本地库与GitHub上库相关联的命令：
	git remote add origin git@github.com:lvshaoyang/Reading-Notes.git
	
	添加后，远程库的名字就是origin，这是Git默认的叫法。
	
	git push命令推送到远程库。
	
	### 小结： ###
	
	要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；

	关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

	此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；