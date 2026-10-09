# Non-Functional Requirements (NFRs)

Derived from the constraints, data types, and indexes of the *Simple Stock Flow* data model.

## NFR-01: Performance and Access Patterns

* **NFR-01.1 (Partial Indexes):** Catalog searches (Q1, Q3) must operate only on active products by filtering through the partial index on the shadow property `deleted_at` being null (T-13) **[§3, §6.2]**.
* **NFR-01.2 (Optimized Reporting):** The aggregated sales report (Q9) must leverage an index-only scan using the unique index `IX_sale_item_sale_id_product_id`, which includes `quantity` and `unit_price`, so that expensive aggregation operations do not need to access the table itself (T-13) **[§6.2]**.

## NFR-02: Privacy and Security

* **NFR-02.1 (Credential Protection):** The system must never persist, index, or expose plaintext passwords in logs or endpoints. An irreversible `password_hash`, managed through an external port, will be stored (D-09) **[§1, §7]**.
* **NFR-02.2 (Personal Data):** The username (`username` and the nominal sales field `sold_by`) is classified as personal data **[§7]**. Access must be restricted, and this information must not appear in anonymous responses (DP-02) **[§7, §7.1]**.

## NFR-03: Data Retention and Immutability

* **NFR-03.1 (Indefinite Retention):** Sales (`sale`) and their line items (`sale_item`) must never be edited or physically deleted under any circumstances **[§5, §7.1]**.
* **NFR-03.2 (Soft Deletion):** Products must never be physically deleted. Any manual attempt to do so must fail explicitly due to the `ON DELETE RESTRICT` constraint (FK-3) **[§5, §7.1]**.

## NFR-04: Concurrency

* **NFR-04.1 (Optimistic Concurrency):** The system will use PostgreSQL's hidden system column `xmin` as an optimistic concurrency token for catalog updates to prevent lost updates when stock is being deducted (D-04, T-10) **[§3, §6.1]**.
