## 合并列的函数 ##

在sql子查询中，经常遇到查询结果为多行的子句，因此就要用到一些函数来对返回的多个结果进行合并成为一个字符串，做为返回结果。

oracle中使用的函数：

参考网址:http://www.cnblogs.com/qianyuliang/p/6649983.html

	wm_concat(column);
	
sqlserver中使用的函数：

参考网址：http://blog.csdn.net/zengcong2013/article/details/42625745

	select column from table1 for xml path('')