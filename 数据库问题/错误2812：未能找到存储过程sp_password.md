### 问题分析
因 SQL 服务器中毒，导致病毒篡改 SQL 用户的密码，病毒为了阻止操作员手工修正（修改）SQL用户的密码，而把存储过程 'sp_password' 删除，导致的问题。
### 解决思路
在系统中重新建一个 'sp_password' 的存储过程，以便更新现有的 SQL用户密码

此方法只是能解决因为没有存储过程而不能修改密码的问题，出现此问题，大多数情况下操作系统已经中毒，这也是为什么更改密码之后，过段时间又不能使用的原因。
**注：本文以 SQL SERVER 2000 界面演示解决此问题的步骤**

**重建 sp_passsword存储过程代码**

```sql
-- SQL代码开始
sp_configure 'allow updates', 1
RECONFIGURE WITH OVERRIDE
go
use master
go
if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[sp_password]') and OBJECTPROPERTY(id, N'IsProcedure') = 1)
drop procedure [dbo].[sp_password]
go
create procedure sp_password
@old sysname = NULL, -- the old (current) password
@new sysname, -- the new password
@loginame sysname = NULL -- user to change password on
as
-- SETUP RUNTIME OPTIONS / DECLARE VARIABLES --
set nocount on
declare @self int
select @self = CASE WHEN @loginame is null THEN 1 ELSE 2 END
-- RESOLVE LOGIN NAME
if @loginame is null
select @loginame = suser_sname()
-- CHECK PERMISSIONS (SecurityAdmin per Richard Waymire) --
IF (not is_srvrolemember('securityadmin') = 1)
AND not @self = 1
begin
dbcc auditevent (107, @self, 0, @loginame, NULL, NULL, NULL)
raiserror(15210,-1,-1)
return (1)
end
ELSE
begin
dbcc auditevent (107, @self, 1, @loginame, NULL, NULL, NULL)
end
-- DISALLOW USER TRANSACTION --
set implicit_transactions off
IF (@@trancount > 0)
begin
raiserror(15002,-1,-1,'sp_password')
return (1)
end
-- RESOLVE LOGIN NAME (disallows nt names)
if not exists (select * from master.dbo.syslogins where
loginname = @loginame and isntname = 0)
begin
raiserror(15007,-1,-1,@loginame)
return (1)
end
-- IF non-SYSADMIN ATTEMPTING CHANGE TO SYSADMIN, REQUIRE PASSWORD (218078) --
if (@self <> 1 AND is_srvrolemember('sysadmin') = 0 AND exists
(SELECT * FROM master.dbo.syslogins WHERE loginname = @loginame and isntname = 0
AND sysadmin = 1) )
SELECT @self = 1
-- CHECK OLD PASSWORD IF NEEDED --
if (@self = 1 or @old is not null)
if not exists (select * from master.dbo.sysxlogins
where srvid IS NULL and
name = @loginame and
( (@old is null and password is null) or
(pwdcompare(@old, password, (CASE WHEN xstatus&2048 = 2048 THEN 1 ELSE 0 END)) = 1) ) )
begin
raiserror(15211,-1,-1)
return (1)
end
-- CHANGE THE PASSWORD --
update master.dbo.sysxlogins
set password = convert(varbinary(256), pwdencrypt(@new)), xdate2 = getdate(), xstatus = xstatus & (~2048)
where name = @loginame and srvid IS NULL
-- UPDATE PROTECTION TIMESTAMP FOR MASTER DB, TO INDICATE SYSLOGINS CHANGE --
exec('use master grant all to null')
-- FINALIZATION: RETURN SUCCESS/FAILURE --
if @@error <> 0
return (1)
raiserror(15478,-1,-1)
return(0) -- sp_password
GO
sp_configure 'allow updates', 0
RECONFIGURE WITH OVERRIDE
-- SQL代码结束
```
