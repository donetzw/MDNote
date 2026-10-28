### 【1】金蝶升级到T3时的问题
用**金蝶K3转T3报错处理小工具**处理[https://wwsd.lanzoue.com/iPg560k5qgve](https://wwsd.lanzoue.com/iPg560k5qgve)
### 【2】T3升级到T+时的问题
```sql
--修改科目表科目方向
update code set bproperty = '0' where ccode like '%2221%'

--清空凭证表币种字段
update gl_accvouch set cexch_name = NULL 

--清空结算方式表
delete from SettleStyle

--删除00无分类的存货分类
delete from InventoryClass where cInvCCode ='00'
```
