# Traceability Matrix

| Requirement / US | Origin in Data Model (`data-model.md`) | Implementation Marker |
|---|---|---|
| **US-01** (Sale) | [§1], [§2.3], [§2.4], [FK-2], [ADR-004] | **engine** (`ck_product_stock_non_negative`) and **domain-only** (freezing) |
| **US-02** (Catalog) | [§1], [§2.2], [§4] | **domain-only** (`price > 0`, pending push-down to engine in T-20) |
| **US-03** (Logical Deletion) | [§2.2], [§3 `deleted_at`], [§7.1], [FK-3] | **engine** (T-09 completed) |
| **US-04** (Reporting) | [§1], [D-06], [§11.1 H-1] | **domain-only** (read port) |
| **NFR-01** (Indexes) | [§6.1], [§6.2] | **pending** (T-13 for partial and composite indexes) |
| **NFR-02** (Privacy)| [§7], [D-09], [DP-02] | Via Hexagonal design (hashing port) and policies |
| **NFR-03** (Retention)| [§5], [§7.1] | **engine** (FK-1, FK-2, FK-3) |
