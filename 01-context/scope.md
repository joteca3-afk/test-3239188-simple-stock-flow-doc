# System Scope

The system's scope is strictly defined by the structure of its five tables and its data retention policies **[§2, §7.1]**.

## What Will Be Built (In Scope)

* **Product Catalog:** Inventory management with strict stock control, assignment to five predefined categories, and support for soft deletion **[§2.2, §7.1]**.
* **Sales Recording:** Confirmation of immutable transactions. The system preserves product prices and category names as snapshots at the time of sale to protect future reports (ADR-004) **[§1, §2.4]**.
* **Operator Management:** Access control through roles (`admin`, `seller`) and irreversible password hashes. Initial administrator creation is handled by the environment (D-09) **[§2.5, §9.2]**.
* **Real-Time Reporting:** Generation of an aggregated sales report queried directly from the database engine, without persisting additional tables (D-06) **[§1]**.

## What Will NOT Be Built (Out of Scope)

* **Customer Relationship Management (CRM):** The system does not record buyer information, only that of the internal operator who performs the transaction **[§1]**.
* **Multi-Currency Support:** The design explicitly supports a single currency; currency conversions are not handled, and currency values are not stored (D-05) **[§1, §3]**.
* **Passive Auditing:** Generic audit logs and auditing columns (`created_at` or `updated_at`) are not implemented **[§8]**.
* **Salesperson Analytics:** Due to the privacy decision (DP-02), reports do not break down performance by individual operator **[§6.3]**.
