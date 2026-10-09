# Business Glossary

Terms derived from the project's ubiquitous language, as defined by the data model **[§1]**.

* **Product:** Catalog item with a name, price, stock quantity, category, and optional image. No SKU or descriptions (DP-03).
* **Category:** Product classification within a fixed set of five categories (D-10). No maintenance operations are supported.
* **Stock:** Available units of a product; must never be negative.
* **Sale:** Completed and immutable business transaction. Once recorded, it cannot be edited or deleted.
* **Sale Item:** A line item that associates the quantity sold with the price and category frozen at the time of the transaction.
* **User:** Authorized internal operator (role `admin` or `seller`). No customer entity exists.
* **Sales Report:** Aggregation by product over a specified date range, calculated without being persisted in database tables.
