# User Stories (Functional)

The following user stories are derived directly from the invariants and business rules of the data model **[§2]**.

## US-01: Recording an Immutable Sale

**As a** internal operator (`seller` or `admin`) **[§1, §2.5]**,  
**I want to** record a sale with at least one product line **[§2.3]**,  
**So that** I can record a commercial transaction that cannot be edited or deleted **[§1, §7.1]**.

**Acceptance Criteria:**

* **Scenario: Successful sale with stock deduction**
  * **Given** that an active product exists with sufficient stock (e.g., `stock >= 2`) **[§2.2]**
  * **And** the product has a unit price of `50.00` **[§2.2, §3]**
  * **When** the operator records a sale of 2 units **[§2.4]**
  * **Then** the system deducts the stock within a single database transaction **[§2.3]**
  * **And** the sale line stores a snapshot of the exact product name, category, and price at that moment (`unit_price = 50.00`) to preserve historical accuracy **[§1, §2.4, ADR-004]**.

* **Scenario: Sale rejected due to insufficient stock**
  * **Given** that a product has `stock = 1` **[§2.2]**
  * **When** the operator attempts to sell 2 units **[§2.2]**
  * **Then** the domain operation (`Product.Withdraw`) fails **[§2.2]**
  * **And** the database-level safeguard (`ck_product_stock_non_negative`) prevents the stock from becoming negative **[§4]**.

## US-02: Catalog Management (Product Creation/Modification)

**As an** administrator **[assumption: logical deduction from the role, §2.5]**,  
**I want to** manage the product catalog by assigning products to one of the five predefined categories **[§2.1, FK-1]**,  
**So that** products are available for sale.

**Acceptance Criteria:**

* **Scenario: Invalid price**
  * **Given** a new or existing product **[§2.2]**
  * **When** an amount of `0` or a negative amount is assigned **[§2.2]**
  * **Then** the `Money` value object and the domain (`Product.ChangePrice`) reject the operation (requires `price > 0`) **[§1, §2.2]**.

## US-03: Soft Deletion of a Product

**As an** administrator **[assumption]**,  
**I want to** deactivate a product without physically deleting it from the database **[§2.2, §7.1]**,  
**So that** historical sales reports are not corrupted **[FK-3, §5]**.

**Acceptance Criteria:**

* **Scenario: Hiding a product after soft deletion**
  * **Given** an active product in the catalog **[§2.2]**
  * **When** the product is deleted **[§7.1]**
  * **Then** the database sets the shadow property `deleted_at` to the current date and time (T-09) **[§3]**
  * **And** the product no longer appears in active search results due to the global query filter **[§2.2]**.
  * **And** if an image exists, the external binary file is physically deleted while the catalog row remains in the database **[§7.1, D-08]**.

## US-04: Aggregated Sales Report

**As an** operator or administrator **[assumption]**,  
**I want to** view a sales report for a specified date range **[§1, Q9]**,  
**So that** I can see totals grouped by product without persisting a report table **[D-06]**.

**Acceptance Criteria:**

* **Scenario: Grouping by frozen category label after recategorization**
  * **Given** a product that was sold under the category "Tools" in September **[§11.1]**
  * **And** the product was recategorized to "Paints" in October **[§11.1]**
  * **When** a report covering both months is requested **[§1]**
  * **Then** the report does not select a single winning category; instead, it returns totals grouped strictly by the frozen `category_name`, displaying two separate rows **[§11.1, H-1]**.
