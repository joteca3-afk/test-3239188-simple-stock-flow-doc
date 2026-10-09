# Domain Events

The data model does not declare any *Outbox* tables or transactional event queues. Therefore, interactions between aggregates operate synchronously within the same database transaction.

**Assumption:** Based on the documented domain methods **[§2]**, the following logical business events are inferred to be emitted by the system or handled in memory:

* **`ProductStockWithdrawn` (Assumption):** Logically triggered when `Product.Withdraw` successfully reduces inventory during a sale **[§2.2]**.
* **`ProductPriceChanged` (Assumption):** Invoked by `Product.ChangePrice` when ensuring that the new amount satisfies the `price > 0` invariant **[§2.2]**.
* **`SaleRegistered` (Assumption):** Final event raised when the `Sale` aggregate is confirmed, after validating that it contains line items and that stock has been deducted for each product **[§2.3]**.
* **`ProductDeleted` (Assumption):** Triggered when a product is soft-deleted, which also orchestrates the physical deletion of the external image file **[§7.1]**.
