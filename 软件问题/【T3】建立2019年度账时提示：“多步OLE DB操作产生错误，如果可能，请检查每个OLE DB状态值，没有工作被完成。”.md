点击确定后提示：“INSERT语句中列的数目大于VALUES子句中指定的值的数目，VALUES子句中值的数目必须与INSERT语句中指定的列的数目匹配”，建立年度账失败。

### 思路分析
将这个报错拖动，显示出新建年度账的界面，可以发现是传输到基础表Customer表（客户档案）时报错，通过跟踪发现是Customer表中updatetime日期字段数值溢出导致的。

### 解决方案
在查询分析器执行以下语句，将updatetime日期字段修改为NULL值即可。
以账套001，2018年度为例：执行完成后，再新建2019年度账即可成功。
```sql
use UFDATA_001_2018
update customer set updatetime=null
```
