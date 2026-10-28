sql server 提供了很多数据库修复的命令，当数据库质疑或是有的无法完成读取时可以尝试这些修复命令。  
1、DBCC CHECKDB  
重启服务器后，在没有进行任何操作的情况下，在SQL查询分析器中执行以下SQL进行数据库的修复，修复数据库存在的一致性错误与分配错误。  
```sql
use master
declare @databasename varchar(255)
set @databasename='需要修复的数据库实体的名称'
exec sp_dboption @databasename, N'single', N'true' --将目标数据库置为单用户状态
dbcc checkdb(@databasename,REPAIR_ALLOW_DATA_LOSS)
dbcc checkdb(@databasename,REPAIR_REBUILD)
exec sp_dboption @databasename, N'single', N'false'--将目标数据库置为多用户状态
```
然后执行 DBCC CHECKDB('需要修复的数据库实体的名称') 检查数据库是否仍旧存在错误。
**注意：修复后可能会造成部分数据的丢失。**

2、DBCC CHECKTABLE  
如果DBCC CHECKDB 检查仍旧存在错误，可以使用DBCC CHECKTABLE来修复。  
```sql
use 需要修复的数据库实体的名称
declare @dbname varchar(255)
set @dbname='需要修复的数据库实体的名称'
exec sp_dboption @dbname,'single user','true'
dbcc checktable('需要修复的数据表的名称',REPAIR_ALLOW_DATA_LOSS)
dbcc checktable('需要修复的数据表的名称',REPAIR_REBUILD)
exec sp_dboption @dbname,'single user','false'
```
3、其他的一些常用的修复命令  
```sql
DBCC DBREINDEX 重建指定数据库中表的一个或多个索引
DBCC DBREINDEX (表名,’’)
```
修复此表所有的索引  
