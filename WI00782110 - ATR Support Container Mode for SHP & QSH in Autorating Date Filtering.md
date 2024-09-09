---
tags:
  - Rating
  - WI
pr: https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev/pullrequest/265748?path=%2FEnterprise%2FProduct%2FOperations%2FMasterFiles%2FBusiness%2FMasterFiles.Business%2FAccounting%2FRegistry%2FJobConfigurationSelectorReadOnlyHelper.cs
---
## TODO
- [x] **1. Where to find the option list for containers**
There are no options for container here:
![[Pasted image 20240826163558.png]]

**How do enable the options in our code:**
```csharp
// AutoRateDateLookupsHelper.cs
if (!AutoRateDateReadOnlyHelper.EditableJobTypesList.Contains(jobType))
{
	return result;
}
```

I think that the options for `Container Mode` are irrespective of `Job Type`, and as such we do not need to worry about this.

> [!warning] Check the above with Product

- [x] **2. Check the below are true**

> [!note] Perhaps this could help:
```c#
packingModes = new List<ZString>();
var shipment = Factory.New<AgencyShipment>();
foreach (var packingModePair in shipment.Lookups.JS_PackingMode_List.ToArray())
{
	packingModes.Add(packingModePair.Code);
}
```
[JobDeclarationLookups.cs](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FProduct%2FOperations%2FCustoms%2FCH%2FCore%2FBusiness%2FBusiness%2FJobDeclaration%2FJobDeclarationLookups.cs&_a=contents&version=GBmaster)

> [!question] What is the best way of doing this. It seems very difficult to maintain a switch statement :( Ask for assistance tomorrow

> When Autorating Revenue / Costs for Shipment, Autorating Date Filtering registry > Container Mode should be matched against Shipment > Basic Registration > Container Mode to identify the date field to be used for Autorating.

> When Autorating Revenue / Costs for Quick Booking, the Container Mode in Autorating Date Filtering registry setting should match against the Quick Booking > Details > Container Mode to identify which Date field should be used for Autorating.

| Job     | Transport | Container                                                 |
| ------- | --------- | --------------------------------------------------------- |
| FCN     | AIR       | ALL, LSE, ULD, BCN, **SCN**, OTH                          |
| FCN     | SEA       | ALL, FCL, LCL, BLK, LQD, BBK, **SCN**, BCN, ROR, OTH      |
| FCN     | ROA       | ALL, FCL, FTL, LCL, LTL, BCN, **SCN**, GRP, OTH           |
| FCN     | RAI       | ALL, FCL, LCL, BLK, LQD, BBK, BCN, **SCN**, GRP, ROR, OTH |
| *SHP*   | AIR       | ALL, LSE, ULD, CON, BCN, SCN                              |
| *SHP*   | SEA       | ALL, FCL, LCL, BLK, LQD, BBK, BCN, SCN, ROR               |
| *SHP*   | FSA       | ALL, LCL, LSE, ULD                                        |
| *SHP*   | FAS       | ALL, LCL, LSE, ULD                                        |
| *SHP*   | ROA       | ALL, FCL, FTL, LCL, LTL, BCN, SCN                         |
| *SHP*   | RAI       | ALL, FCL, LCL, BLK, LQD, BBK, BCN, SCN                    |
| *SHP*   | COU       | ALL, OBC, UNA                                             |
| **QSH** | AIR       | ALL, LSE, ULD                                             |
| **QSH** | SEA       | ALL, FCL, LCL                                             |
| **QSH** | ROA       | ALL, FCL, LCL, FTL, LTL                                   |
| **QSH** | RAI       | ALL, FCL, LCL                                             |
| **QSH** | COU       | ALL, OBC                                                  |
> [!question] Should we be hard-coding this, would be a pain to maintain??

```csharp
// QuotedBooking.cs
public static CodeDescriptionPairList GetContainerModes(
	string transportMode,
	bool bookingOnly)
{
	var result = RatingFreightModeLists.ContainerModeList(transportMode);
	if (bookingOnly)
	{
		result.RemoveCode(Constants.RateMode.SEA);
		result.RemoveCode(Constants.RateMode.ROA);
		result.RemoveCode(Constants.RateMode.RAI);
		result.RemoveCode(Constants.RateMode.FWL);
		result.RemoveCode(Constants.RateMode.LRO);
		result.RemoveCode(Constants.RateMode.COU);
	}
	else
	{
		result.RemoveCode(Constants.ContainerModes.OnBoardCourier);
	}

	return result;
}
```

- [x] **3. Check what the `SCN` stuff is**
I'm interpreting this as the `Container Mode` dropdown should show show `SCN` as an option when the `FCN` job type is selected, e.g. :

![image](data:image/png;base64,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)

- [ ] **Check that generic things are "All" or ""**

## Dump
In
- **LHS: *Registry > AutoRating > Charge Code Groups > AutoRating Date Filtering**
- **RHS: *System > Details**
- Select override Default
- Choose CUS

Currently the below configuration is:
![[Pasted image 20240826155749.png]]
When a *Job Type* that is not `FCN` is selected, the *Container* column cannot be selected:
![[Pasted image 20240826155925.png]]
We want this to work with *Job Types* `SHP` and `QSH`.

### Extra notes I don't yet understand
> Options of Container Mode could be selected should refer to **Operate > Forwarding > Shipment > Basic Registration > Container Mode** per relevant Transport Mode selected:

> When Autorating Revenue / Costs for Shipment, the Container Mode in Autorating Date Filtering registry setting should match against the **Shipment > Basic Registration > Container Mode** to identify which Date field should be used for Autorating.

> When Autorating Revenue / Costs for Quick Booking, the Container Mode in Autorating Date Filtering registry setting should match against the **Quick Booking > Details > Container Mode** to identify which Date field should be used for Autorating.

Working on writing unit tests and implementation for container mode support registry support WI. DAT test for my xml and search filters WI passed DAT so will continue on that as well