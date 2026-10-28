**SM_Period  会计期间表**


**EAP_AccInformation  选项表**
      Caption = 启用年度  修改Value值
      Caption = 启用期间  修改Value值
      Caption = 总账（或其它模块名称）  修改idEnablePeriod（启用期间）值

**EAP_menu 菜单表**
       Visibility = False 不可见
       Visibility = True   可见
```sql
--查询启用的模块
select * from Eap_accinformation where name in ('PS','CashBankMoneyFlow','Stock','Ledger','TUFO','StdAccount','StdIERP') 
```
