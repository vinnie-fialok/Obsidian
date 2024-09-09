---
tags:
  - NCN
  - Reporting
  - WI
---
## TODO:
- [x] figure out how to find everyone in a release group [[Queries and Entity Framework]]
- [x] how to open winform through blazor component
- [x] figure out how to work with delivery instructions and how to actually deliver something to an email
- [x] finish the example, try to get it running perhaps and then submit for review
![[Pasted image 20240716135507.png]]

> [!info] Remember to change database name, see: [Testing winzor with ZClientEDI](https://devops.wisetechglobal.com/wtg/CargoWise/_wiki/wikis/CargoWise.wiki/5097/Testing-winzor-with-ZClientEDI)
## User Flow
### 1. Select report

> [!success] Done

I need to run:
```sql
SELECT   
	SI_SU  
 FROM StmMenuTemplatePivot  
 JOIN StmMenuItem ON SU_PK = SI_SU
 order by StmMenuItem.SU_MenuName
```

```c#
> [!success] Done
var stmMenuTemplatePivotQuery = new ZDBOnlyQuery(typeof(StmMenuTemplatePivot));
var nameSubQuery = new ZDBOnlySubQuery(typeof(StmMenuItem), StmMenuItemSchema.PK);
nameSubQuery.AddToFilter(StmMenuItemSchema.SU_MenuName, SQLComparisonOperator.Like, $"%{query}%");
stmMenuTemplatePivotQuery.AddSubQuery(StmMenuTemplatePivotSchema.SI_SU, nameSubQuery, JoinCondition.And);

return Factory.Load<StmMenuTemplatePivot>(stmMenuTemplatePivotQuery).Select((pivot) => Factory.Load<ReportCommand>(pivot.SI_SU).GetReport());
```
### 2. Display fields of report for user to fill

> [!success] Done

### 3. Select release group

```html
<label>Report Filter To Substitute</label>
<input type="text" list="report-filters" @bind-value=@ReportFilterToSubstitute />
<datalist id="report-filters">
	@foreach (var option in SelectedReportFilters)
	{
		<option value=@option>@((option as FilterField).DisplayName)</option>
	}
</datalist>
```
### 4. Select which fields to fill in for the report based on each member of the release group


### 5. Deliver reports to each member
See: [Attempted implementation](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FClientExtensions%2FEDI%2FZClientEDI%2FZClientEDI%2FIncidentManager%2FModule%2FReports%2FPerformanceReportCreator.cs&version=GC3022272d8a6877b9219c4c052bb03ba74c999085)

## Testing for `Visual Board Performance - Board.xls`
**Filters**

| Name               | Type     | Details        |       |
| ------------------ | -------- | -------------- | ----- |
| Stats Date Range   | Type     | Date Range     |       |
|                    | Required |                |       |
| Aggregation Period | Type     | MultipleChoice |       |
|                    | Style    | DropDown       |       |
|                    | Option   | H              | Hour  |
|                    |          | D              | Day   |
|                    |          | M              | Month |
|                    | Required |                |       |
## Feedback
- make it clearer what report filter to substitute is
- we have 2 release groups, could be confusing
- don't show delivery forms over and over again