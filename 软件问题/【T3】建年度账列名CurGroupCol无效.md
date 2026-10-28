### 原因分析
原年度账套表Rpt_FldDEF中多这两个字段

### 解决方案
打开SQL企业管理器, 在年度数据库中查到 Rpt_FldDEF表设计中删除`CurGroupCol`列，再次年结会提示列名`CurSubTotalCol`无效,返回数据库Rpt_FldDEF表设计中删除，即可解决问题。
