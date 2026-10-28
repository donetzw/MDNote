```sql
--12.0及以前版本 空密码
1B2M2Y8AsgTpgAmY7PhCfg==

--12.1及以上版本 空密码
dL4Wl5cQ1MTnxmR4VgiEVg==

--13.0最新补丁后不能为空密码,设置为123456
FOG2ALH9V59HQzuI6NhSkQ==
```


#### 系统管理员密码修改
替换**系统库**里的**EAP_ConfigPath**的**AdminPassword**字段
#### 账套用户密码修改
替换两张表，一张是**账套库里的EAP_User**，一张是系统库里的**EAP_User**
