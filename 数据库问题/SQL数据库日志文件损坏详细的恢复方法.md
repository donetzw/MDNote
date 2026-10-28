>从第1步开始是数据损坏无法分离的解决步骤，如果是无法附加，跳过1,3,4步。

1、停止数据库服务。
2、将需要恢复的数据库文件复制到另外的位置。
3、启动数据库服务。
4、确认要恢复的数据库文件已经成功复制到另外的位置，然后在SQL Server Management Studio中删除要恢复的数据库。
5、新建同名的数据库(数据库文件名也要相同，最好路径和原文件以前的路径也相同)。
6、停止数据库服务,同时删除新建数据库生成的.ldf文件。
7、用第2步中备份的.mdf文件覆盖新数据库的同名文件。
8、启动数据库服务。
9、运行命令 `alter database 要恢复的数据库名称 set emergency`，将数据库设置为紧急模式
10、运行下面的命令就可以恢复数据库：
```sql
use master
declare @databasename varchar(255)
set @databasename='要恢复的数据库名称'
exec sp_dboption @databasename, N'single', N'true' --将目标数据库置为单用户状态
dbcc checkdb(@databasename,REPAIR_ALLOW_DATA_LOSS)
dbcc checkdb(@databasename,REPAIR_REBUILD)
exec sp_dboption @databasename, N'single', N'false'--将目标数据库置为多用户状态
```
注：这个方法是通过.mdf文件恢复数据库，即使大家的log文件丢失也可以进行恢复。
