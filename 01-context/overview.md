# Overview — Simple Stock Flow

*Simple Stock Flow* is a centralized inventory management and sales recording system. Based on its data model **[§1, §2]**, it is designed to ensure the integrity of accounting history and the consistency of available stock.

The system relies heavily on the database engine (PostgreSQL) to protect its critical invariants, such as physically preventing negative inventory **[§2.2]**, while delegating the validation of in-flight business process rules to the application domain, ensuring that business operations proceed without entering invalid states **[§4]**.
