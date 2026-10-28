#### 操作步骤：  

1、解压标准版的账套备份文件压缩包  

2、修改xml中的版本配置项，TPlusStd修改为TPlusPop  

3、把解压后的数据库备份文件（除xml外的另一个文件）恢复到sql server  

4、在恢复的数据库中执行脚本：
```sql
update EAP_AccInformation set Value='VFBsdXNQb3AgVUZURGF0YTg5MTU1Ml8wMDAwMDQ=' where Name='ProductFlag'   
```
5、备份数据库，把备份文件和xml文件压缩成一个zip包，然后恢复到普及版版本中  

6、恢复后在账套库中执行脚本：
```sql
update EAP_AccInformation set Value=0 where Name in ('PS','CashBankMoneyFlow','Stock','Ledger','TUFO') 
```
7、登录账套在功能启用中启用普及账表、普及进销存功能
