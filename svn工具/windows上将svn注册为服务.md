## 安装svn服务端 ##

遇见问题汇总：

1、将svn注册为服务时，提示OpeenSCManager failed 失败5？

 解决：
 
 1  原因是当前用户权限不足,需要在注册表中
 	运行中：regedit打开注册表.
 HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\EnableLUA 的值改为0,把这个值改成0.但是改完后也不行.用第2个方法.
 
 2.在打开dos窗口时,使用管理员身份运行.然后运行注册的服务的命令:
 
 sc create svnservice binpath= "\"C:\Program Files\TortoiseSVN\bin\svnserve.exe\" --service -r F:\share\FQA" displayname= "svnservice" depend= Tcpip start= auto
 
 其中C盘下是安装路径,F盘下是仓库路径.
 属性值和等号之间要有一个空格.


