2017-07-15
    
    mac mini系统
    总述：由于在工作或者其它地方做笔记在需要的时候不能同步看到，加上做的笔记有点凌乱，因此想要把笔记同步一下，方便看也方便修改。
    参考网址：
        http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013745374151782eb658c5a5ca454eaa451661275886c6000（这个主要是了解了想着概念）
        http://www.cnblogs.com/ldq2016/p/5468987.html（主要是这个）
    1、添加远程库：
        注意区分本地库，远程库。到底是什么。
        在github上create一个repository。lvshaoyang/Reading-Notes是这个仓库的位置。lvshaoyang为我的用户名。
    2、然后在mac mini上安装了git，然后就是创建本地库。
    很简单：
    1、先创建一个文件夹，创建空目录即可。进入到当前新建文件的路径下（确保路径不要含有中文）。
    2、通过git init命令可以把这个目录变成Git可以管理的仓库。然后可以看到一个.git文件。
    查看生成的ssh key 使用命令进入：cd ~/.ssh
    配置Git：
        输入ssh-keygen -t rsa -C "984800179@qq.com"直接配置就行。不用输入其它东西。默认就行，生成的一个id_rsa.pub中有一个SSH密码，将这个key设置到github网站上去，不然不让你提交。
    配置用户名和邮箱：
        git config —-global user.name "lvshaoyang"
        git config —global user.email "984800179@qq.com"
        使用命令git remote add origin git@github.com:lvshaoyang/Reading-Notes.git。此命令就是为了把本地库跟github上的仓库关联起来。
        本地库同步命令：
        git pull git@github.com:lvshaoyang/Reading-Notes.git
        上传本地库：
        git add .
        意义：增加所有文件 。
        提交到本地仓库命令：
        git commit -m "解释此次都提交了什么，像一个说明"
        推送本地库到远程库：
        git push git@github.com:lvshaoyang/Reading-Notes.git
备注：
	
	如果我在一台电脑上更新了，我另一台电脑怎么弄呢？
	那就需要先在本地Pull命令，跟服务器同步下。
	git pull git@github.com:lvshaoyang/Reading-Notes.git
	注：pull（拉，拔，拖）从服务器上拉下来。
	   push(推，增加，推动)
注：
	
	在公司电脑上安装git客户端，同步新增的repository，Reading-Notes库，步骤：
		1、在E盘建立新文件夹，Reading-Notes,然后右键git bash here,在此文件夹下生成一个.git的隐藏文件。
		2、然后需要再生成一个ssh key,输入命令：git-keygen -t rsa -C "984800179@qq.com",注意，生成的文件id_rsa.pub在/c/Users/adm/.ssh/  这个文件夹下，打开id_rsa.pub,将其中生成的ssh-rsa添加到github中。在github中选择仓库，点击settings下面的Deploy key,add key,输入title和key即可。
		3、使用命令关联本地库和远程库：git remote add origin git@github.com:lvshaoyang/Reading-Notes.git
		4、将远程库的文件弄到本地库：git pull git@github.com:lvshaoyang/Reading-Notes.git
		5、关于使用git config的配置命令，可以尝试着不执行，然后看如果不配置的话，是否可以检下来项目！！！！（答：仍然要配置）
	注意1：这个ssh key生成的，只是read-only,不能提交，这个是什么原因呢？
		原因是在添加key的时候，有一个选项，Allow write access,这个我没有打勾，因此不允许我这个key写入仓库，重新添加一下，选择打勾就可以。
	注意2：使用ssh命令生成key时，可以输入路径，注意一点就可以了。
	注意3：又遇到了新问题，虽然将key设置为Read/Write了，但是仍然提示：
		ssh:Could not resolve hostname github.com:Name or service not known
		fatal(致命的):Could not read from remote repository
		答：又按步骤从3-5来了一遍
	
	另外在github上添加ssh key 在repository上有一个，在用户设置里也有一个Settings->SSH and GPG keys
        
    
