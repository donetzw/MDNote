#### 此脚本通常用于UFSystem异常导致的账套登录失败（数据库里显示正常）
1、分离UFSystem，修改名字为UFSystem1
2、附加数据库UFSystem1【需要**修改附加为**UFSystem1，修改数据路径名】
3、打开系统管理，自动创建新的UFSystem
4、执行以下脚本：
```sql
--第一步：删除相关表
drop table
UA_Account_sub,UA_log,UA_period,UA_HoldAuth,UA_Task,UA_Tasklog,UA_user,UA_userflow,UA_Account
go

--第二步：导相关表
select * into UA_Account from UFSystem1..UA_Account
go
select * into UA_Account_sub from UFSystem1..UA_Account_sub
go
select * into UA_log from UFSystem1..UA_log
go
select * into UA_period from UFSystem1..UA_period
go
select * into UA_HoldAuth from UFSystem1..UA_HoldAuth
go
select * into UA_Task from UFSystem1..UA_Task
go
select * into UA_Tasklog from UFSystem1..UA_Tasklog
go
select * into UA_user from UFSystem1..UA_user
go
select * into UA_userflow from UFSystem1..UA_userflow
go
```
5、重启软件服务
