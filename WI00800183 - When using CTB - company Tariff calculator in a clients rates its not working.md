---
tags:
  - Rating
  - WI
  - Defect
---
## Whats wrong
When Company Tariff rate is setup based on WKP calculator, Results Checkbox on client rate based on CTB calculator does not show the correct values.

Take note that autorating works correctly. only UI in client rates does not show the correct values.

## Investigation
**Change needs to be made in `WarehousePackCalculator.cs`**. Specifically at `WarehousePackCalculator::CloneLineItemsUpdatingRateValue(`