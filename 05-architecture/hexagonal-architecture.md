# Hexagonal Architecture (Ports and Adapters)

The data model demonstrates a clear separation between the application core and its external dependencies.

## Outbound Adapters (Driven)

1. **Persistence Adapter (Entity Framework):**
   * Responsible for mapping plural collections in the C# code (e.g., `DbSet<Product> Products`) to the singular table names required by PostgreSQL governance (e.g., `product`) **[§0]**.
   * Manages shadow properties such as `deleted_at` to isolate soft deletion from the pure domain model (D-03) **[§3]**.

2. **External Storage Port (Files):**
   * The system stores only an opaque key (`image_key`) in the `product` table. Binary data and direct file paths are not stored, delegating image retrieval to an external storage adapter (D-08) **[§1, §3]**.

3. **Cryptography Port (Hashing):**
   * The domain never handles the plaintext password. It delegates the generation of an irreversible hash (`password_hash`) to an external port (D-09) **[§1, §2.5]**.

## Inbound Adapters (Driving)

1. **Reporting Read Port:**
   * The sales report is not persisted in tables. It is generated through a specialized read port that aggregates data directly within the database engine, grouping results by the frozen category label (D-06, H-1) **[§1, §11.1]**.
