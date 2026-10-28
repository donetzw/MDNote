| 表名       | TableName     |
| -------- | ------------- |
| 出入库单据主表  | ST_RDRecord   |
| 主表单据编号   | code          |
| 主表单据id   | ID            |
| 出入库单据子表  | ST_RDRecord_b |
| 对应主表单据id | idRDRecordDTO |

#### 查询单据编号的ID
```sql
SELECT code, id FROM ST_RDRecord WHERE (code = '单据编号')
```

#### 根据ID查询单据明细看是否正确
```sql
SELECT * FROM ST_RDRecord_b WHERE (idRDRecordDTO = 'ID')
```
#### 删除主表记录
```sql
delete FROM ST_RDRecord where (code = '单据编号')
```
#### 删除子表记录
```sql
delete FROM ST_RDRecord_b where (idRDRecordDTO = 'ID')
```
