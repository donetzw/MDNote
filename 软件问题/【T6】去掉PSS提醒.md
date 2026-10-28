一、更新最新补丁，然后在工具——风格配置菜单中，勾选取消PSS系统选项
二、找到T6安装目录中的**T6SOFT\Desktop\U8Service.ini**文件，先复制一份备用，然后打开，删除**name=T6servicestime**那一段
三、执行如下语句：
```sql
delete from UFSystem..sysService
delete from UFSystem..UA_Message
```
