---
tags:
  - Rating
  - WI
pr: https://devops.wisetechglobal.com/wtg/CargoWise/_git/Dev/pullrequest/264036?path=%2FEnterprise%2FProduct%2FCore%2FUniversalDataBuss%2FUniversalDataBuss.DataObjects%2FDataObjects%2FUniversalCommon%2FOrganisationData.cs
---
## What remains to be done here is:

- [ ] **a) Add potential creditor field to USXML for BWQ/QB  (not visible in HLD but is required) both export and import** <- I think this is already done?
- [ ] **b) Add creditor field to USXML for BWQ/QB (not visible in HLD but is required) both export and import**
- [x] **c) Add a creditor filter  (not visible in HLD but is required)**
- [x] **d) Change modules to have a new column for the creditor (check if OOQ/BWQ/QB has it or not (not visible in HLD but is required)**
	- OOQ already has creditor column **but NOT** a creditor filter
		- [x] Creditor filter in OOQ
	- [x] Creditor column in QB
	- [x] Creditor column in BWQ
- [ ] **e) Add a potential carrier filter (not visible in HLD but is required)**
- [ ] **f) Add a potential creditor filter (not visible in HLD but is required)**

## Solution

> [!Question]
> 1. Are all of these to be done in this wi?
> 	1. yes
> 2. Does OOQ module also require potential carrier and potential creditor columns??

### a) b)
![[Pasted image 20240819121709.png]]
**[[USXML|What is UXML?]]**

```xml
<collection>
...
</collection>
<collectionwiithcarrier>
	<potentialcarrier>
		<code />
		<name />
		<creditor>
			...
		</creditor>
	</potentialcarrier>
</collection>
```

Need to add ability for USXML serialization of **PotentialCreditor** and **Creditor** in *Booking With Quote* and *Quoted Booking (???)*. No idea how USXML is used/ works.

```csharp
#region ImportFromXML

void OnImportFromXml_Click(object sender, EventArgs e)
{
	NewXmlDataTransferDirector(new QuotedBookingValueObjectDataAdapter()).PromptUserAndImport(BillingInterfaceName.QuotedBookingXmlImport); // Interface name for billing purposes
}
```

```csharp
QuotedBookingEDocsViaUniversalXmlSupport
```

### d)
Add creditor column for the module view off:
1. Booking with quote
2. One off quote
3. Quick booking

### c) e) f)
![[Pasted image 20240819120852.png]]

Don't know how to do it, but need to add **creditor**, **potential creditor** and **potential carrier** filters as above in the bookings module. *D* is required to be completed before this.

> [!info] What do these filters refer to?? (on the UI, not the DB)
>  ![[Pasted image 20240819122422.png]]
> 1. Creditor filter
> 2. Potential Carrier && potential creditor filters

#### Where is the Potential Creditor and Potential Carrier fields stored???

```sql
VB_TH IN (SELECT TH_PK FROM dbo.RatingHeader WHERE TH_PK IN (SELECT TT_TH FROM dbo.RateOneOffShipment WHERE TT_PK IN (SELECT TTC_TT FROM dbo.RateOneOffCarrier WHERE TTC_OH_Carrier = 'fb7dfa80-f180-4ba1-bbe2-35051b2f573a')))
```
