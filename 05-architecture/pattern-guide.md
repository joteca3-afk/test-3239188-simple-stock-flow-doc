# Domain Patterns Guide (DDD)

The database schema reflects the existence of three main aggregates and one static reference entity.

## 1. Catalog Aggregate (`Product`)
* **Root:** `Product` **[§2.2]**.
* **Behavior:** Manages availability (stock). Invariants requiring a price greater than zero (`Price > 0`) reside **only in the domain**, whereas the guarantee against negative stock is delegated to the database engine as a final safeguard **[§2.2, §4]**.
* **Deletion Pattern:** Implements soft deletion (`deleted_at`), ensuring the row remains to preserve the integrity of historical reports (T-09) **[§2.2, §7.1]**.

## 2. Sales Aggregate (`Sale`)
* **Root:** `Sale` **[§2.3]**.
* **Internal Entity:** `SaleItem` **[§2.4]**. It lacks an independent lifecycle; this is reflected in the physical model via `ON DELETE CASCADE` (FK-2) **[§5]**.
* **Snapshot Pattern:** At the time of sale, the system creates an immutable copy of `unit_price`, `product_name`, and `category_name` within the sale line item. This isolates sales history from any future recategorization or price changes in the catalog (ADR-004) **[§1, §2.4]**.

## 3. Identity Aggregate (`User`)
* **Root:** `User` **[§2.5]**. * **Behavior:** Responsible for validating roles within a closed set (`admin`, `seller`) and ensuring the username is normalized to lowercase **[§2.5, §4]**.

## 4. Value Objects
The physical model avoids secondary tables for properties that lack their own identity (D-07) **[§2]**:
* `Money`: Restricts the price to positive values ​​and standardizes rounding. It maps directly to the `numeric(18,2)` column **[§1, §2.2]**.
* `Quantity`: Ensures strictly positive sales units **[§1, §2.4]**.
