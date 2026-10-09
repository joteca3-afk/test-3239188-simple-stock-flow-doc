# Domain Map and Glossary

The domain's ubiquitous language is defined based on functional terms and their technical counterparts **[§1]**.

| Term (Business) | Functional Definition | Technical Implementation |
|---|---|---|
| **Product** | Catalog item with a name, price, stock quantity, category, and optional image (DP-03). | `Product` entity |
| **Category** | Classification of a product. A fixed set of five categories with no maintenance operations (D-10). | `Category` entity |
| **Price** | Current monetary value. Must be strictly positive. | `Money` value object |
| **Stock** | Available units. Must never be negative. | `product.stock` |
| **Sale** | Completed and immutable commercial transaction. | `Sale` aggregate root |
| **Sale Item** | A line item within a sale with a frozen price and category. It cannot exist outside the sale. | `SaleItem` internal entity |
| **Sale Total** | Sum of the subtotals. **Calculated, not stored** (Article VII). | `Sale.Total` (no database column) |
| **User** | Internal operator who authenticates and records sales. There are no customers. | `User` aggregate root |
| **Password Hash** | Irreversible representation of the password (D-09). | `user.password_hash` |
| **Sales Report** | Aggregation by product over a specified date range. It is not persisted (D-06). | Read model |
