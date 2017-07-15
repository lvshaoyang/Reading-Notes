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
    配置Git：
        输入ssh -keygen -t rsa -C "984800179@qq.com"直接配置就行。不用输入其它东西。默认就行，生成的一个id_rsa.pub中有一个SSH密码，将这个key设置到github网站上去，不然不让你提交。
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
        
    