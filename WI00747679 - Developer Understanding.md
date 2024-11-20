As part of this WI I need to:
## For all container modes/ Transport Modes
Fill in the `Containers` tab of the *Consol* form. Do this using the details input in the Cargo details modal on C3.

**The main fields from the HLD:**
(the below screenshot is from the `SEA FCL` table, however the other container modes have the same or less fields, and)
![[Pasted image 20241120153157.png]]
We get the **Transport Mode, Container Mode, Origin, Destination, Effective On** from the search inputs tab on C3:
![[Pasted image 20241120153627.png]] 

We get the **Number of Container, Commodity Code, Container Type** from the cargo details modal:
![[Pasted image 20241120153431.png]]
becomes
![[Pasted image 20241120153443.png]]
- **WQ**: KG by default
- **Gross Weight**: based upon container type

We get the **Carrier, Service Level, Payment Term** we get from the `RateResultDto` returned when searching for rates:
![[Pasted image 20241120154522.png]]