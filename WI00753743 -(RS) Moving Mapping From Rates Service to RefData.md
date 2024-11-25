```
PAI 07-Nov-24 13:55 GMT+11:00:

**Feedback:**

1. We don't need SQL Function of "IsValidUnitOrNull". It's no longer applicable. It was for Charge table which is already deleted.

2. I'm not sure if WiseRate CodeMapping can be count as a general CW Reference Data?!

3. Why datat converted into json. It can be exported from DB in excel or csv file. === Nam's note: if we do this, we can get a backup file from production, restore it in local db, and extract the data.
```

1. Remove this
2. `CodeMapping` isn't used in CW1, the places where it is referenced is dead code:
	1. 