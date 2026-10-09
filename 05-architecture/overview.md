# Architecture Overview — Simple Stock Flow

The *Simple Stock Flow* system is designed according to the principles of **Domain-Driven Design (DDD)** and a Ports and Adapters architecture.

Based on the rules derived from the data model **[§1, §2]**, the system strictly isolates the business domain from infrastructure details. The persistence model is centralized in a single relational schema (`sales`) in PostgreSQL 16.14 **[§0]**.

**Inferred Global Design Decisions:**

* **Single-Currency by Design:** No currency column exists in any table, ensuring that the system operates using a single currency (D-05) **[§1, §3]**.
* **Absence of Passive Auditing:** The system does not implement `created_at` or `updated_at` columns (a fixed rule). Only timestamps associated with business events, such as `sold_at` and `deleted_at`, are persisted **[§8]**.
* **Distributed Protection:** The database engine is responsible for enforcing final structural invariants (e.g., preventing negative stock through `ck_product_stock_non_negative`), while the domain validates process rules (e.g., rejecting negative monetary amounts) **[§2.2, ADR-002]**.
