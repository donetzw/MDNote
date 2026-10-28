#### 18.0执行以下语句重新对应科目
```sql
if (select value from EAP_AccInformation where name='MultiAccountingOrgan')=1 and (select COUNT( distinct accountingyear) from AA_Account )>1

begin

delete from AA_AccountAssociation

declare @IdMarketingOrgan int
declare cur2 cursor for
select distinct IdMarketingOrgan from AA_Account order by IdMarketingOrgan
open cur2
fetch next from cur2 into @IdMarketingOrgan
while @@FETCH_STATUS=0
begin
print @IdMarketingOrgan
set nocount on
DECLARE @CurrentYear INT --当年  
DECLARE @NewAccountId int--本年科目ID  
DECLARE @PreAccountId int--上年科目ID  
DECLARE @NewAccountCode NVARCHAR(50)--本年科目编码  
DECLARE @PreAccountCode NVARCHAR(50)--上年科目编码
DECLARE @NewisEndNode tinyint--本年科目是否末级  
DECLARE @PreisEndNode tinyint--上年科目是否末级 
DECLARE @PreYear INT --上年
declare cur cursor for
select a.accountingyear currentaccountingyear,b.accountingyear preaccountingyear from (
select distinct top 100  accountingyear from AA_Account where IdMarketingOrgan=@IdMarketingOrgan order by
accountingyear desc) a left join (
select  distinct top 100 accountingyear from AA_Account where IdMarketingOrgan=@IdMarketingOrgan order by
accountingyear desc) b on a.accountingyear >=b.accountingyear 
open cur
fetch next from cur into @CurrentYear,@PreYear
while @@FETCH_STATUS=0
begin
	--年度一致直接插入
	if @CurrentYear<>@PreYear 
		begin
		--科目完全一致且都是末级
			INSERT INTO AA_AccountAssociation(currentaccountingyear,idcurrentaccountDTO,idpreaccountDTO,preaccountingyear)  
				select @CurrentYear,a.id,b.id,@PreYear from aa_account a left join AA_Account b on a.code=b.code  where a.accountingyear=@CurrentYear and b.accountingyear =@PreYear and
				a.isEndNode=1 and b.isEndNode=1 and a.IdMarketingOrgan=@IdMarketingOrgan and b.IdMarketingOrgan=@IdMarketingOrgan
				--科目一致其中一个年度不是末级，对应下级科目第一个
				declare curNoEnd cursor for
				select a.id,b.id,a.isEndNode,b.isEndNode,a.code,b.code from aa_account a left join AA_Account b on a.code=b.code  where a.accountingyear=@CurrentYear and b.accountingyear =@PreYear and
				((a.isEndNode=1 and b.isEndNode=0) or (a.isEndNode=0 and b.isEndNode=1)) and a.IdMarketingOrgan=@IdMarketingOrgan and b.IdMarketingOrgan=@IdMarketingOrgan
				open curNoEnd
				fetch next from curNoEnd into @NewAccountId,@PreAccountId,@NewisEndNode,@PreisEndNode,@NewAccountCode,@PreAccountCode
				while @@FETCH_STATUS=0
				begin
					if @NewisEndNode =0
						select top 1 @NewAccountId=id from AA_Account where accountingyear=@CurrentYear and code like ''+@NewAccountCode+'%' and isEndNode=1 and IdMarketingOrgan=@IdMarketingOrgan order by code
					if @PreisEndNode =0
						select top 1 @PreAccountId=id from AA_Account where accountingyear=@PreYear and code like ''+@PreAccountCode+'%' and isEndNode=1 and IdMarketingOrgan=@IdMarketingOrgan order by code
						
						INSERT INTO AA_AccountAssociation(currentaccountingyear,idcurrentaccountDTO,idpreaccountDTO,preaccountingyear)  
						values(@CurrentYear,@NewAccountId,@PreAccountId,@PreYear)
						 fetch next from curNoEnd into @NewAccountId,@PreAccountId,@NewisEndNode,@PreisEndNode,@NewAccountCode,@PreAccountCode
				end
				close curNoEnd
				deallocate curNoEnd
		end
		else
		begin
		INSERT INTO AA_AccountAssociation(currentaccountingyear,idcurrentaccountDTO,idpreaccountDTO,preaccountingyear)  
				select @CurrentYear,id,id,@CurrentYear from aa_account where accountingyear=@CurrentYear  and IdMarketingOrgan=@IdMarketingOrgan
		end
	fetch next from cur into @CurrentYear,@PreYear
end
close cur
deallocate cur

fetch next from cur2 into @IdMarketingOrgan
end
close cur2
deallocate cur2
end

```
