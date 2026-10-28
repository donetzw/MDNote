### 修改表字段长度

```sql
alter table 表名 alter column 列名 类型名(长度)
```
### 删除表数据drop、truncate和delete的用法
```sql
drop table 表名 --删除整个表，不能附带条件
truncate table 表名 --数据量大需要清空整个表里的内容时使用，速度比delete快，不能附带条件
delete from 表名 where 列名 = '条件内容' --删除表里的某些数据，速度比truncate慢，可以附带条件
```
### 数据库可疑检测修复
```sql
dbcc checkdb('DatabaseName ') --检测语句
alter database DatabaseName set single_user --单用户模式
dbcc checkdb ('DatabaseName ',repair_allow_data_loss) --无损修复
dbcc checkdb ('DatabaseName ',repair_rebuild) --有损修复
alter database DatabaseName set multi_user --多用户模式
```
### msdb数据库置疑修复语句
```sql
USE MASTER
GO
SP_CONFIGURE 'ALLOW UPDATES',1
RECONFIGURE WITH OVERRIDE
GO
UPDATE SYSDATABASES SET STATUS =32768 WHERE NAME='msdb'
Go
sp_dboption 'msdb', 'single user', 'true'
Go
DBCC CHECKDB('msdb')
Go
update sysdatabases set status =28 where name='msdb'
Go
sp_configure 'allow updates', 0
reconfigure with override
Go
sp_dboption 'msdb', 'single user', 'false'
Go
```
### 数据库无法打开恢复操作将数据库标记为SUSPECT解决方法
本脚本修复完成之后，再次使用检测修复工具修复，检测修复工具无法直接修复标记为SUSPECT的数据库。
```sql
USE MASTER
GO
SP_CONFIGURE 'ALLOW UPDATES',1 RECONFIGURE WITH OVERRIDE
GO
UPDATE SYSDATABASES SET STATUS =32768 WHERE NAME='置疑的数据库名'
Go
sp_dboption '置疑的数据库名', 'single user', 'true'
Go
DBCC CHECKDB('置疑的数据库名')
Go
update sysdatabases set status =28 where name='置疑的数据库名'
Go
sp_configure 'allow updates', 0 reconfigure with override
Go
sp_dboption '置疑的数据库名', 'single user', 'false'
Go
```
### 修复sa账户被锁定
```sql
alter login sa enable
alter login sa with password='cdcykj' unlock,check_policy=off,check_expiration=off
```
### 替换某一列的字符
```sql
update 表名 set 列名 = REPLACE(列名,'被替换的字符','替换后的字符')
--示例语句
--将科目编码中的-替换成0
select * from code where ccode like '%-%' --查询科目表带有-的科目
select * from gl_accvouch where ccode like '%-%' --查询凭证及明细账表带有-的科目
select * from gl_accsum where ccode like '%-%' --查询科目总账表带有-的科目
select * from gl_accass where ccode like '%-%' --查询辅助总账表带有-的科目
Update code SET ccode=REPLACE(ccode,'-','0') --将科目表科目编码中的-替换成0
Update gl_accvouch SET ccode=REPLACE(ccode,'-','0') --将凭证及明细账表科目编码中的-替换成0
Update gl_accsum SET ccode=REPLACE(ccode,'-','0') --将科目总账表科目编码中的-替换成0
Update gl_accass SET ccode=REPLACE(ccode,'-','0') --将辅助总账表科目编码中的-替换成0
```
### 数据库一直显示正在还原
```sql
Restore Database 数据库名 with Recovery
```
### 导表语句
#### select into 要求目标表不存在
```sql
select * into 表名 from 来源数据库名..表名
```
#### insert into 两个表结构一样
```sql
insert into 表名 select * from 来源数据库名..表名
```
#### insert into 两个表结构不一样
```sql
insert into 表名(列名1,列名2) select 列名1,列名2 from 来源数据库名..表名
```
#### 生成批量dbcc语句
```sql
select ('dbcc checkdb('+name+')') from sysdatabases
```
#### 查看数据库路径及版本
```sql
select name,filename,version from sysdatabases
```
