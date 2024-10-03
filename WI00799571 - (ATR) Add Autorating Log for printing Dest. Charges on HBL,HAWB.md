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