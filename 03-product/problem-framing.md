# Problem Statement — Simple Stock Flow

Traditional inventory systems often suffer from two major problems that this data model eliminates by design:

1. **Accounting History Corruption:** When a product's price or category changes, historical reports can be altered. *Simple Stock Flow* solves this by preserving the exact product name, price, and category at the time of the sale within the `SaleItem` entity **[§1, §2.4, ADR-004]**. If a product categorized as "Tools" is later reclassified as "Paints," past sales will continue to report it under "Tools" (H-1) **[§11.1]**.

2. **Inventory Inconsistency:** The system addresses the problem of negative stock not only at the application level, but also through a physical, database-level safeguard (`ck_product_stock_non_negative`) that cannot be bypassed by ordinary database operations **[§2.2, ADR-002]**.
