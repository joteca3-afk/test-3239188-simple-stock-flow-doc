# Product Vision — Simple Stock Flow

*Simple Stock Flow* is a transactional point-of-sale and inventory management system focused on **absolute data integrity and operational simplicity**.

Its product boundaries are intentionally restricted to avoid unnecessary complexity and scope creep:

* **It Is Neither a CRM nor a B2C E-commerce Platform:** The domain deliberately excludes customers. Sales record which internal operator (`seller` or `admin`) performed the transaction, but there is no buyer or end-customer entity **[§1]**.
* **Minimalist Catalog Management:** A product is strictly an item with a name, price, stock quantity, category, and image. It has no description, SKU, or secondary reference code (DP-03) **[§1]**.
* **Single Currency:** The system operates globally with a single currency. There is no complexity involving exchange rates or currency columns (D-05) **[§1, §3]**.
* **Operator Privacy Protection:** Although the system records which user performs each sale for accounting audit purposes **[§7.1]**, aggregated performance reports group results by product and never expose individual salesperson performance (DP-02) **[§6.3, §7]**.

## Inferred Business Assumptions

* **Assumption 1 (Physical Store or Counter-Based Operations):** Since only internal operators (`user`) interact with the system and the categories consist of a fixed, seeded set of five traditional hardware store departments (General, Tools, Electrical, Plumbing, and Paints) **[§9.1]**, it is assumed that the system is designed for a physical hardware store or a closed warehouse counter.
* **Assumption 2 (No Complex Returns):** Since sales are completed, immutable business transactions and there is no port for editing or deleting sales **[§1, §2.3]**, it is assumed that the daily cash-handling workflow does not support cancellations through this system, or that such transactions are handled externally.
