## Stage 1 - Schema update + DB Transformation
The original table looks like this:
![[Pasted image 20241107163125.png]]
The new table looks like this:
![[Pasted image 20241107163135.png]]

The current schema is:
```sql
CREATE TABLE [AccChargeCodeUniversalCodeMapping] (
   [AUP_PK] UNIQUEIDENTIFIER NOT NULL,
   [AUP_AC] UNIQUEIDENTIFIER NOT NULL,
   [AUP_UniversalChargeCode] VARCHAR(10) NOT NULL DEFAULT '',
   [AUP_SystemCreateTimeUtc] SMALLDATETIME NULL,
   [AUP_SystemCreateUser] VARCHAR(3) NOT NULL DEFAULT '',
   [AUP_SystemLastEditTimeUtc] SMALLDATETIME NULL,
   [AUP_SystemLastEditUser] VARCHAR(3) NOT NULL DEFAULT '',
);

-- with constraints:
ALTER TABLE [AccChargeCodeUniversalCodeMapping] WITH NOCHECK
      ADD   CONSTRAINT [AccChargeCodeUniversalCodeMapping_AUP_AC_FK2_AccChargeCode_CRR_120N] FOREIGN KEY
          ( [AUP_AC] )
          REFERENCES [AccChargeCode]
          ( [AC_PK] )
      ON DELETE CASCADE
;
ALTER TABLE [AccChargeCodeUniversalCodeMapping] WITH NOCHECK
ADD CONSTRAINT [Constraint_AUP_UniversalChargeCode] CHECK (AUP_UniversalChargeCode <> '');
```

To match this new table, we will need to 

## Stage 2 - CarrierChargeCode input and module
![[Pasted image 20241107162731.png]]
We want to make a non-persistent business object called `CarrierChargeCodeBizo` that is almost identical to the `UniversalChargeCodeBizo`. We want to make an associate module for it called `CarrierChargeCodeModule`. We can extract most of the logic for this from `UniversalChargeCodeModule` see: [OnCustomGridLoad](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FProduct%2FOperations%2FRating%2FRating.Module%2FUniversalChargeCode%2FUniversalChargeCodeModule.cs&version=GBmaster&line=61&lineEnd=154&lineStartColumn=3&lineEndColumn=4&lineStyle=plain&_a=contents).
## Stage 3 - Linking it all together + Validation
UI goes in `AccChargeCodeForm.Designer.cs` in `MasterFiles.GUI` solution.
We want to add 3 more inputs to the `UniversalChargeCodeMappingsGrid`.
**1. Type input**
- This should be either `UCC` or `CAR`, by default it should be `UCC`
- make a new lookup in `AccChargeCodeUniversalCodeMappingLookups.cs` in the Rating solution that is of type CodeDescriptionPairList

**2. Charge code input**
- make a lookup in `AccChargeCodeUniversalCodeMappingLookups` similar to `UniversalChargeCodeBizoList`. 

A findbox knows to look for a code with the `CodeProperty and DescriptionProperty` attributes, see: [UniversalChargeCodeBizo.cs](https://devops.wisetechglobal.com/wtg/CargoWise/_search?action=contents&text=UniversalChargeCodeBizo&type=code&lp=custom-Project&filters=ProjectFilters%7BCargoWise%7D&pageSize=25&result=DefaultCollection%2FCargoWise%2FDev%2FGBmaster%2F%2FEnterprise%2FProduct%2FOperations%2FMasterFiles%2FBusiness%2FMasterFiles.Business%2FRating%2FUniversalChargeCode%2FUniversalChargeCodeBizo.cs)
```c#
[CodeProperty(UniversalChargeCodeBizo.Schema.UCC_Code)]
[DescriptionProperty(UniversalChargeCodeBizo.Schema.UCC_Description)]
```