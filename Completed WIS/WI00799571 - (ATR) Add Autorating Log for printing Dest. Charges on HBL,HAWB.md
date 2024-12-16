---
tags:
  - WI
  - Rating
---
## Investigation
Potentially use `IAutoRatingGUIInteractor` in `HouseBillCharges::AutoRateOverseasJob(GlbCompany)`.

## Called from
1. `ChargesCollection.GetFilteredCharges()`. ![[Pasted image 20241003104745.png]] Shouldn't this mean that it doesnt have to call it over and over???
![[Pasted image 20241003111207.png]]

First time build is called
![[Pasted image 20241003114554.png]]****
![[Pasted image 20241003114613.png]]
AWBHeaderManager.cs
![[Pasted image 20241105163557.png]]

Either need to find a way to make a new HouseBillCharges, so maybe a new ShipmentExportAWBHeader

SQL Query: "SD_Name = 'CommunityRegionsForDirectionCalculation' AND CONVERT(NVARCHAR (4000), CONVERT(VARBINARY(8000), SD_BinaryValue)) LIKE '%e4eb6e97-aa78-4c46-bf86-35b7fbb5fb0e%'" [IsNoResultQuery=false]