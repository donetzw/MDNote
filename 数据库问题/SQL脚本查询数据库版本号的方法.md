#### 1、版本号查看

 ```sql
SELECT SERVERPROPERTY('ProductVersion') 
 ```
#### 2、bak识别内部版本号

 ```sql
RESTORE HEADERONLY FROM DISK = N'D:\xxxx.bak' 
 ```
查看**Database version**字段。

#### 3、mdf识别内部版本号

```sql
 dbcc checkprimaryfile('D:\xxxx.mdf',2) 
```
查看**Database version**字段。
#### 4、附版本号对应表
|        产品名称        |      版本号      | 内部版本号 |
| :----------------: | :-----------: | :---: |
|  SQL Server 2022   | 16.00.xxxx.xx |  957  |
|  SQL Server 2019   | 15.00.xxxx.xx |  904  |
|  SQL Server 2017   | 14.00.xxxx.xx |  869  |
|  SQL Server 2016   | 13.00.xxxx.xx |  852  |
|  SQL Server 2014   | 12.00.xxxx.xx |  782  |
|  SQL Server 2012   | 11.00.xxxx.xx |  706  |
| SQL Server 2008 R2 | 10.50.xxxx.xx |  661  |
|  SQL Server 2008   | 10.00.xxxx.xx |  655  |
|  SQL Server 2005   | 9.00.xxxx.xx  |  611  |
|  SQL Server 2000   | 8.00.xxxx.xx  |  539  |
|   SQL Server 7.0   | 7.00.xxxx.xx  |  515  |
