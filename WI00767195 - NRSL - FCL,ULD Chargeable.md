---
tags:
  - Rating
  - WI
pr: https://devops.wisetechglobal.com/wtg/Glow/_git/Glow/pullrequest/271830
---
## TODO
- [x] CLEANUP DTOS ON API
- [x] CLEANUP DTOS ON FRONTEND
- [x] FIX VARIABLE NAMES IN TEST

## Setup that returns results
![[Pasted image 20240917104459.png]]
## What should be enabled:

| Chargeable Unit      | Rate Source | ... | ... | Mode    | ... | ...     | ... | Spot |
| -------------------- | ----------- | --- | --- | ------- | --- | ------- | --- | ---- |
|                      | CW          | CS  | CG  | **FCL** | LCL | **ULD** | LSE |      |
| No. of Container/ULD | Y           | Y   | Y   | **Y**   | N/A | **Y**   | N/A | Y    |
| Weight (KG)          | Y           | Y   | Y   | **N/A** | N/A | **Y**   | Y   | Y    |
| Volume (CBM)         | Y           | Y   | N/A | **N/A** | Y   | **Y**   | Y   | Y    |
> [!INFO] **LSE**
> ![[Pasted image 20240917110649.png]]

> [!INFO] **LCL**
> ![[Pasted image 20240917110723.png]]

## TODO:
- [ ] **This may require API changes to support specifying the chargeable amount on a per-container/per-packline basis - please investigate this.**
	- [ ] Investigate the above
	- [x] **Investigate, or just ask, how to make a rate in the db.**

The **ChargeableOverride** field in the dto is equivalent to the sum of weight or volume:
```csharp
var totalWeightInKG = 
	jobInfo
		.Containers
		.Sum(c => c?.PackLines?.Sum(p => Core.Constants.Weight.Convert(p?.Weight ?? 0, p?.WeightUnit ?? Core.Constants.Weight.Kilograms, Core.Constants.Weight.Kilograms)));

var totalVolumeInM3 = 
	jobInfo
		.Containers
		.Sum(c => c?.PackLines?.Sum(p => Core.Constants.Volume.Convert(p?.Volume ?? 0, p?.VolumeUnit ?? Core.Constants.Volume.CubicMetres, Core.Constants.Volume.CubicMetres)));
```

- [x] **We require "Chargeable weight" and "Chargeable volume" fields to be visible for FCL/ULD containers - see Figma for what this should look like.**
- [x] **Should the chips match the figma? (asked Mitchell)**
- [x] We get charged (`LCL`) for container and weight, which we should not. Perhaps modify the adapter to account for this.
![[Pasted image 20240930140409.png]]

> [!info] HAVE TO SET REGISTRY FOR CHARGAEBLE TO BE USED
```csharp
var roundings = new DefaultRoundingsCollection();
var rounding = roundings.AddNew();
rounding.Code = RatingConstants.RateCategory.LCL;
rounding.RoundingType = RatingRoundingTypes.Chargeable;

using (DataRegistryRating.Instance.DefaultRounding.SetTemporaryValue(Guid.Empty, Guid.Empty, Guid.Empty, roundings))
```