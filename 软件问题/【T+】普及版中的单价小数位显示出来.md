#### 执行完以下语句后重启服务
```sql
--查询记录
select IsVisible,* from EAP_AccInformation 
where Caption in ('数量小数位','舍零','单价整数位','单价小数位')
--修改记录
update EAP_AccInformation set IsVisible=1  where Caption in ('数量小数位','舍零','单价整数位','单价小数位')
```
