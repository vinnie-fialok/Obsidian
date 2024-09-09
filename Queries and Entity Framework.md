## Examples
**Making a simple query with `where` statement**
```csharp
 Factory.Load<GlbGroup>(new ZQuery(GlbGroupSchema.GG_Type, "STF"));
```
Notice that in the query we have `GlbGroupSchema` and in the `Factory.Load` type we have `GlbGroup`.

See: **[[WI - Automate Performance Reports#1. Select report|Making a query with joins (sort of)]]**

> [!info] See [Cargowise Entity Framework](https://devops.wisetechglobal.com/wtg/CargoWise/_wiki/wikis/CargoWise.wiki/6858/Cargo-Wise-Entity-Framework?anchor=the-following-options-are-available-on-zquery)
