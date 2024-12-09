`IsManualCostSelect` should return false.
```csharp
bool IsManualCostSelect(bool isCosting, RatingCriteria criteria)
{
	return isCosting && RatingContext.IsManualCostSelectMode && !criteria.IsServicesOnly && criteria.SupportsManualRateSelection;
}
```