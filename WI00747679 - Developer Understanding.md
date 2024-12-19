> This document is essentially my understanding of how we source the data for each required field as per the HLD when creating a new consol.

**The main fields from the HLD:**
(the below screenshot is from the `SEA FCL` table, however the other container modes have the same or less fields, and would be filled in similarly.)
![[Pasted image 20241120153157.png]]

We get the **Transport Mode, Container Mode, Origin, Destination, Effective On** from the search inputs tab on C3:
![[Pasted image 20241120153627.png]] 

We get the **Carrier, Service Level, Payment Term** we get from the `RateResultDto` returned when searching for rates:
![[Pasted image 20241120154522.png]]
**Origin, Destination** are
## SEA FCL Containers
We get the **Number of Container, Commodity Code, Container Type** from the cargo details modal, e.g.
![[Pasted image 20241120153431.png]]
becomes
![[Pasted image 20241120153443.png]]
- **WQ**: KG by default
- **Gross Weight**: based upon container type
- **Container #**: Leave blank
## SEA LCL Containers
![[Pasted image 20241121102544.png]]
Fill the above, put the user's org in `Consigner` box. The arrows point to chargeable, weight, and volume as in C3.


![[Pasted image 20241219110351.png]]