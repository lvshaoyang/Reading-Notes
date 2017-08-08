### ---异地备份到本地--- ###
--需要一条一条执行，并且

1、

--启用xp_cmdshell
use master;
--重新设置配置参数
EXEC sp_configure 'show advanced options', 1
RECONFIGURE WITH OVERRIDE
--启用xp_cmdshell
EXEC sp_configure 'xp_cmdshell', 1
RECONFIGURE WITH OVERRIDE

2、--在sql中建立映射
exec master..xp_cmdshell 'net use \\192.168.1.143\share' "eplg"  /user:192.168.1.143\adm
 
--说明：\\192.168.1.143\share 为本地的一个共享文件

--eplg 为adm用户的密码

--/user:192.168.1.143\adm  为本地ip和计算机用户


4、--将备份好的文件复制到本地
exec master..xp_cmdshell 'copy E:\SQL2008_Bakup\123.bak \\192.168.1.143\share'

--说明：E:\SQL2008_Bakup\123.bak 为服务器上备份的文件
--\\192.168.1.143\share 为本地的一个共享文件

5、关闭xp_cmdshell
USE master 
EXEC sp_configure 'show advanced options', 1 
RECONFIGURE WITH OVERRIDE 
EXEC sp_configure 'xp_cmdshell', 0 
RECONFIGURE WITH OVERRIDE 
EXEC sp_configure   'show advanced options', 0
RECONFIGURE WITH OVERRIDE 
