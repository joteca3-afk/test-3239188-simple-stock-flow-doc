# Entities and Business Rules

The bounded context consists of five entities with invariants strictly divided between the database engine and the domain **[§2]**.

## `Category` (Reference Entity)

* **Rule:** The name is required, unique, and non-empty **[§2.1]**.
* **Constraint:** The repository is read-only; data is created during the initial migration **[§2.1, §9.1]**.

## `Product` (Aggregate Root)

* **Stock Invariant:** `stock >= 0`, enforced by the database engine (`ck_product_stock_non_negative`) **[§2.2]**.
* **Price Invariant:** `price > 0`, enforced **only at the domain level** by `Product.ChangePrice` (T-20) **[§2.2]**.
* **Soft Deletion:** Products are never physically deleted. The shadow property `deleted_at` is used instead **[§2.2]**.

## `Sale` (Aggregate Root)

* **Immutability:** Once recorded, a sale cannot be edited or deleted **[§2.3]**.
* **Composition:** A sale must contain at least one line item (`Sale.EnsureConfirmable`) **[§2.3]**.
* **Transactionality:** Deducting stock and inserting the sale item constitute a single operation coordinated by `Sale.AddItem` **[§2.3]**.

## `SaleItem` (Internal Entity)

* **Isolation:** A sale item cannot exist independently of its sale (foreign key `ON DELETE CASCADE`) **[§2.4, FK-2]**.
* **Snapshot Preservation:** The product name, price, and category are copied at the time of the sale to isolate historical records from catalog changes **[§2.4]**.

## `User` (Identity Aggregate Root)

* **Normalization:** The `username` is stored in lowercase and trimmed (`User.NormalizeUsername`) **[§2.5]**.
* **Roles:** A closed set of roles (`admin`, `seller`) validated by the domain **[§2.5]**.
