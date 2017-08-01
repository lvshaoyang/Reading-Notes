2017/6/28 11:20:31 
# SqlServer2005数据库收缩日志文件 #
参考网址：http://www.cnblogs.com/blackcore/archive/2010/12/27/1917911.html(收缩日志文件)
		http://blog.csdn.net/yabingshi_tech/article/details/10419797(SQLSERVER三种恢复模型)
		https://docs.microsoft.com/zh-cn/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql(SQLSERVER官方文档)
			
	问题描述：
	吉林警察学院数据库日志文件ldf达到了1.45T，现记录对其进行收缩释放空间的过程。
	注：baseName为数据库名。
	1、关于SqlServer数据库文件有两个以.mdf结尾和以.ldf结尾。以mdf结尾的为数据文件，以ldf文件结尾的为日志文件。
	2、　查看数据库的recovery_model_desc类型(数据库的恢复模型)
		SELECT NAME, recovery_model_desc FROM sys.databases where name='baseName';
		如果是FULL类型，修改为SIMPLE类型　　
		ALTER DATABASE baseName SET Recovery simple;
	3、查看数据库的相关文件的大小：
		Use baseName
		SELECT NAME, size FROM sys.database_files;
		备注：WEIXIN为数据库名。另一个方法，在要查询的数据库上“新建查询”，进入查询分析器执行此SQL.可以看到相应的文件名及其大小。
	4、收缩数据库到指定大小。从3中获取到要收缩的文件。
		DBCC SHRINKFILE (N'epweixinV1_log' , 1024);
		注： 1.N'epweixinV1_log'表示一个Unicode常量。N 代表存入数据库时以 Unicode 格式存储。
			2.1024单位为MB。为指定的恢复后的大小。
			3.如果正在使用的文件大于指定的大小，则不会被恢复到指定的大小。
	5、恢复成FULL类型
		ALTER DATABASE baseName(数据库名) SET Recovery FULL;
		