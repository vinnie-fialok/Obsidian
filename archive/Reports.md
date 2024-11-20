---
tags:
  - NCN
  - Reporting
---
### Where is the form?
The name is `RuntimeOptionsForm.cs` in `DocumentEngine.GUI.Winzor`. This is a *generic* form that works for all reports.

### How to create and customize reports?
Refer to [WiseTech academy](https://wisetechacademy.com/search?quickstart=90c05a6b-dfbb-4cbf-b48a-2b769f760bb5).

### How are primary filters set?
Filters are part of the `Report report` argument in the public constructor of `RuntimeOptionsForm`. The `Report` class has the field `FilterCollection` which is made up of. Filters are extracted from the filters tab in the `ReportAnalyser.cs` class in method `ReadFiltersFromFiltersTab`. ~~There is a list stored in `ReportAnalyser.DataSourceParameters` that contains the filters???~~
![[Pasted image 20240709165306.png]]

> [!success] There is a filters sheet that must be added for filters to appear.

> [!todo] How is the filter sheet used?????

### How to Show All Reports?
```sql
SELECT
      [SU_MenuName] as [Menu Name]
         ,[SU_BusinessContext] as [Business Context]
         ,replace([SO_ExcelTemplatePath], 'Enterprise\Product\Documents\ExcelTemplates\' , '') as [Excel File]
  FROM [StmTemplate]
  INNER JOIN [StmMenuTemplatePivot] on SI_SO = SO_PK
  INNER JOIN [StmMenuItem] ON SU_PK = SI_SU
  where [StmMenuItem].[SU_MenuName] like '%'
  order by [StmMenuItem].[SU_MenuName]
```

The `SI_SU` column is the one to use when trying to access a report.
