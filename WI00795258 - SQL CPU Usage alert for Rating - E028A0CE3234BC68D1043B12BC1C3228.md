## The Query
```sql
SELECT ti_pk,
       ti_aircrafttype,
       ti_buyersconsolratemode,
       ti_cartagedeliveryaddresspostcode,
       ti_cartagepickupaddresspostcode,
       ti_contractnumber,
       ti_contractnumberlinked,
       ti_creationsource,
       ti_datachecked,
       ti_destinationlrc,
       ti_firstloadlrc,
       ti_firstroutesetloadportlrc,
       ti_fmctariffid,
       ti_frequency,
       ti_frequencyunit,
       ti_fromid,
       ti_fromtablecode,
       ti_gatewayagenttype,
       ti_gc_publisher,
       ti_hbldeliverymode,
       ti_iscrosstrade,
       ti_isexcludedfromautorating,
       ti_istact,
       ti_isvalid,
       ti_lastdischargelrc,
       ti_lastroutesetdischargeportlrc,
       ti_lineorder,
       ti_matchcontainerrateclass,
       ti_mode,
       ti_oa_cartagedeliveryaddressoverride,
       ti_oa_cartagepickupaddressoverride,
       ti_oh_agentoverride,
       ti_oh_consignee,
       ti_oh_consignor,
       ti_oh_controllingcustomer,
       ti_oh_supplier,
       ti_oh_transportprovider,
       ti_originlrc,
       CASE
         WHEN ti_pageclosingtext IS NULL THEN NULL
         WHEN Datalength(ti_pageclosingtext) < 1024 THEN ti_pageclosingtext
         ELSE Char(0) + Char(0)
       END AS TI_PageClosingText,
       ti_pageheading,
       CASE
         WHEN ti_pageopeningtext IS NULL THEN NULL
         WHEN Datalength(ti_pageopeningtext) < 1024 THEN ti_pageopeningtext
         ELSE Char(0) + Char(0)
       END AS TI_PageOpeningText,
       ti_parentid,
       ti_parenttablecode,
       ti_paymentterm,
       ti_planneddischargelrc,
       ti_plannedloadlrc,
       ti_pl_nkcarrierservicelevel,
       ti_quotepageincoterm,
       ti_ratecategory,
       ti_ratedestination,
       ti_rateenddate,
       ti_rateorigin,
       ti_ratestartdate,
       ti_rc,
       ti_rh_nkcommoditycode,
       ti_rs_nkgatewayservicelevel,
       ti_rs_nkservicelevel_ni,
       ti_rx_nkcurrency,
       ti_shipmentconsolidationstatus,
       ti_systemcreatetimeutc,
       ti_systemcreateuser,
       ti_systemlastedittimeutc,
       ti_systemlastedituser,
       ti_th,
       ti_toid,
       ti_totablecode,
       ti_transittime,
       ti_tz_destinationzone,
       ti_tz_originzone,
       ti_vialrc
FROM   dbo.rateentry
WHERE  ( ( ti_rateenddate >= @CWO1_
            OR ti_rateenddate IS NULL )
         AND ti_ratestartdate < @CWO2_ )
       AND ti_cartagedeliveryaddresspostcode = @CWO3_
       AND ti_cartagepickupaddresspostcode = @CWO4_
       AND ti_destinationlrc = @CWO5_
       AND ( ti_firstroutesetloadportlrc = @CWO6_
             AND ti_firstroutesetloadportlrc <> '' )
       AND ti_fmctariffid = @CWO7_
       AND ti_gc_publisher = @CWO8_
       AND ti_iscrosstrade = @CWO9_
       AND ( ti_lastroutesetdischargeportlrc = @CWO10_
             AND ti_lastroutesetdischargeportlrc <> '' )
       AND ti_matchcontainerrateclass = @CWO11_
       AND ti_mode = @CWO12_
       AND ti_oh_transportprovider IS NULL
       AND ti_originlrc = @CWO13_
       AND ti_ratecategory = @CWO14_
       AND ti_rc = @CWO15_
       AND ti_rh_nkcommoditycode = @CWO16_
       AND ti_rs_nkservicelevel_ni = @CWO17_
       AND ti_th = @CWO18_ 
```

## Tracing

> [!info] query logs available [here](https://eye.wtg.ws/s/wisecloud-support/app/dashboards#/view/d4dcf11e-519d-54b1-b56f-b5b040744eca?_g=(filters:!(),refreshInterval:(pause:!t,value:60000),time:(from:now-1y%2Fd,to:now)))

 - Query is built in `RatingCriteria.GetTimeLocationCarrierCategoryFilter`.
 - Reading the query I don't see any obvious issues, some stats:
	 - Average in top 150: 0.349s
	 - Slowest in top 150 (averaged across 17 queries for the row): 0.8215s
 - Is used in `CW1RatesProvider.LoadRateEntries`.
The `foreach` loop in `CW1RatesProvider.LoadRateEntries` may be problematic:
```c#
foreach (var header in distinctRatingHeaders)
{
	var entriesFilter = header.IsIntercompanyTariff()
		? timeLocationCarrierFilterCategoryForIntercompanyTariff
		: timeLocationCarrierCategoryFilter;

	criteria.ApplyBBK_BLK_RORRateModeFilter(entriesFilter);
	criteria.AddContractNumberFilterIfNeeded(entriesFilter, isCosting);

	var entries = header.LoadRateEntriesForAutoRater(entriesFilter).ToArray();
```
`distinctRatingHeaders` is passed in as an argument. I don't see any obvious way around this at first glance.