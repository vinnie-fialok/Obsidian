## Design
UI goes in `AccChargeCodeForm.Designer.cs` in `MasterFiles.GUI` solution.
We want to add 3 more inputs to the `UniversalChargeCodeMappingsGrid`.
**1. Type input**
- This should be either `UCC` or `CAR`, by default it should be `UCC`
- make a new lookup in `AccChargeCodeUniversalCodeMappingLookups.cs` in the Rating solution that is of type CodeDescriptionPairList

**2. Charge code input**
- make a lookup in 