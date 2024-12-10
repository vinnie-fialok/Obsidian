Hey Azi, hope you're well ![Smile](https://statics.teams.cdn.office.net/evergreen-assets/personal-expressions/v2/assets/emoticons/smile/default/20_f.png?v=v82), was busy last week with other WI's so just now looking into this.

The data in the WiseRatse db seems to be used in CW1 as `Universal` data. E.g. the `Ref_WTG_Commidity` table is used in CW1 to represent '**Universal Commodity Group**' (see [here](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FProduct%2FOperations%2FRating%2FGUI%2FWiseRates%2FWiseRatesFilterStrip%2FRateFilterStripBusinessObject.cs&version=GBmaster&line=446&lineEnd=448&lineStartColumn=1&lineEndColumn=97&lineStyle=plain&_a=contents "https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FProduct%2FOperations%2FRating%2FGUI%2FWiseRates%2FWiseRatesFilterStrip%2FRateFilterStripBusinessObject.cs&version=GBmaster&line=446&lineEnd=448&lineStartColumn=1&lineEndColumn=97&lineStyle=plain&_a=contents")) and the `Ref_WTG_ServiceLevel` table is used to represent '**Universal Service Leve**l' (see [here](https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FProduct%2FOperations%2FRating%2FGUI%2FWiseRates%2FWiseRatesFilterStrip%2FRateFilterStripBusinessObject.cs&version=GBmaster&line=711&lineEnd=714&lineStartColumn=1&lineEndColumn=28&lineStyle=plain&_a=contents "https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev?path=%2FEnterprise%2FProduct%2FOperations%2FRating%2FGUI%2FWiseRates%2FWiseRatesFilterStrip%2FRateFilterStripBusinessObject.cs&version=GBmaster&line=711&lineEnd=714&lineStartColumn=1&lineEndColumn=28&lineStyle=plain&_a=contents")). We could just migrate all of the WiseRates tables and add the prefix `Universal` to make this distinction clear, (e.g. `Ref_WTG_ServiceLevel` becomes `RefUniversalServiceLevel` in refdata).

_This would solve our problem and enable us to use RefData_, but I have concerns with this as below.

I find that this solution may be confusing since tables `RefContainer` and `RefUniversalContainer` (_which in WiseRates is_ `Ref_WTG_Container`) would represent the same entity, that is, container types. There would also be some duplicated data across these tables such as the `20GP` container.

My understanding is that these two table are different because `Ref_WTG_Container` only requires the container types used by `CargoSphere`/ `CargoGuide`? Because of this `RefContainer` has container types `Ref_WTG_Container` does not have (e.g. `AAA` container type) and `Ref_WTG_Container` has container types `RefContainer` does not have (e.g. `42G2` container type). (ee)

To me it seems like the original design for this may have been benefited by using CW1 `RefContainer` and migrating CargoGuide/ CargoSphere ref data to the CW1 schema rather than making another table `Ref_WTG_Container` but I'm likely missing some background here. This would be non-trivial now as `RefContainer` has more columns with more details compared to `Ref_WTG_Container` (e.g. weight, capacity, width, height ...).

I wanted to ask if you knew why it is the case that WiseRates has its own reference tables at all?

I believe that having duplicate data in tables as would be required if we were to do my proposed solution would not be ideal, but we have had this quirk (I'm not sure if this is an issue or a design decision I don't understand yet) for a long time.

==========================================
- `wiserates/carrierwhitelist` used by UniversalRateService

So far `ChargeCodeMapping` and `CommodityGroups` are unique to Wiserates.
