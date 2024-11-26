```
PAI 07-Nov-24 13:55 GMT+11:00:

**Feedback:**

1. We don't need SQL Function of "IsValidUnitOrNull". It's no longer applicable. It was for Charge table which is already deleted.

2. I'm not sure if WiseRate CodeMapping can be count as a general CW Reference Data?!

3. Why datat converted into json. It can be exported from DB in excel or csv file. === Nam's note: if we do this, we can get a backup file from production, restore it in local db, and extract the data.
```

1. Remove this
2. `CodeMapping` isn't used in CW1, the places where it is referenced is dead code:
	1. The only place where Rating gets `CodeMappings` from RatesService is in [RatesServiceClient](https://devops.wisetechglobal.com/wtg/_search?text=GetChargeCodesWithMappingInfo&type=code&pageSize=25&filters=ProjectFilters%7BCargoWise*RatesService%7D&action=contents&result=DefaultCollection%2FCargoWise%2FDev%2FGBmaster%2F%2FEnterprise%2FProduct%2FOperations%2FMasterFiles%2FBusiness%2FMasterFiles.Business%2FRating%2FAccChargeCodeUniversalCodeMapping%2FAccChargeCodeUniversalCodeMappingLookups.cs). This is used in [here](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FProduct%2FOperations%2FMasterFiles%2FBusiness%2FMasterFiles.Business%2FRating%2FAccChargeCodeUniversalCodeMapping%2FAccChargeCodeUniversalCodeMappingLookups.cs&_a=contents&version=GBmaster) in `AccChargeCodeUniversalCodeMappingLookups.cs`.
	2. 