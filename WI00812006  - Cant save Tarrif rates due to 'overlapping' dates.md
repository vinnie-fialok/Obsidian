![[Pasted image 20241008160116.png]]

Perhaps a difference between `CompanyTarrif.cs` and `Costing.cs`

## Problem Tyler brough up

1. It deleted and removes from the collection, but it isn't actually deleted from the db
2. We check against the db

Option 1:
Save then rollback

Object 2:
Remove the new entry from the list of entries checked for duplicacy

What the DB has:
![[Pasted image 20241009153015.png]]
What we see in CW1:
![[Pasted image 20241009153036.png]]