# inventory
# INVENTORY_STRUCTURE_V3

This workbook is a **database design worksheet** for an Inventory Management System (with a second, smaller Employee schema exercise on the side). It lays out the tables, fields, keys, and constraints, and lists the SQL queries/functions/procedures/triggers to be written against that schema. It looks like coursework/practice material for a DBMS (Oracle/SQL) assignment rather than a live database export.

## File / Sheet Overview

| Sheet | Contents |
|---|---|
| **Sheet1** | Primary layout of the Inventory schema — tables, fields, keys, and constraints, plus sample record counts (e.g. "5–10", "15–20") and starting ID values. |
| **Sheet1 (2)** | A rearranged/duplicate version of the same Inventory schema (same tables & constraints, different cell layout) — likely an alternate draft or print layout. |
| **INVENT Queries** | A checklist of SQL tasks to complete against the Inventory schema: constraints, joins, subqueries, functions, stored procedures, triggers, and reports. |
| **Sheet3** | A separate, smaller schema for **EMP** (Employee) and **EMP_SAL** (Employee Salary) tables, with field-level constraints. |
| **One** | The same EMP / EMP_SAL schema as Sheet3, but with **data types** instead of constraints, plus a sample query. |

## Inventory Schema (Sheet1 / Sheet1 (2))

### PRODUCT
| Field | Rule |
|---|---|
| PID | Primary Key (starts at P0001) |
| PDESC | Not Null |
| PRICE | > 0 |
| CATEGORY | One of: IT, HA, HC |
| SID | Foreign Key → SUPPLIER |

### SUPPLIER
| Field | Rule |
|---|---|
| SID | Primary Key (starts at S0001) |
| SNAME | Not Null |
| SADD | Not Null |
| SCITY | Delhi |
| SPHONE | Unique |
| EMAIL | — |

### CUSTOMER (CUST)
| Field | Rule |
|---|---|
| CID | Primary Key (starts at C0001) |
| CNAME | Not Null |
| ADDRESS | Not Null |
| CITY | Not Null |
| PHONE | Not Null |
| EMAIL | Not Null |
| DOB | Before 1-Jan-2000 |

### ORDERS
| Field | Rule |
|---|---|
| OID | Primary Key (starts at O0001) |
| ODATE | — |
| CID | Foreign Key → CUSTOMER |
| PID | Foreign Key → PRODUCT |
| OQTY | ≥ 1 |

### STOCK
| Field | Rule |
|---|---|
| PID | Foreign Key → PRODUCT |
| SQTY | ≥ 0 |
| ROL (Re-Order Level) | > 0 |
| MOQ (Min Order Qty) | ≥ 5 |

### BILL (derived / report view)
Fields: OID, ODATE, CNAME, ADDRESS, PHONE, PDESC, PRICE, OQTY, AMT
— a billing summary combining Orders, Customer, and Product data.

### PURCHASE
Fields: PID, SID, PQTY, DOP (Date of Purchase)
— records stock purchased from suppliers.

**Notation used in the sheet:** "Add_X" marks the add/insert action for a table, values like "5–10" / "15–20" are suggested sample-record counts, and single sample IDs (P0001, S0001, C0001, O0001) mark the starting key value for each table.

## SQL Tasks (INVENT Queries sheet)

A checklist of tasks to implement against the Inventory schema:

**Constraints**
- Customer age must be ≥ 18 years
- Order date should default to the current date

**Joins**
- (join queries to be written — no specifics listed)

**Subqueries**
- Product description for a given order number (e.g. O1001)
- Products supplied by Delhi-based suppliers
- Products purchased by a named customer (e.g. Amit)

**Functions**
- Return details of products supplied by a specified supplier
- Return details of products sold on the current date ("Daily Report")

**Procedures**
- `Daily_Sale` — get the current day's total sales
- Insert data into the respective tables
- Get supplier name, phone, product description, and price for a specified product
- Extend the above procedure to also add stock quantity

**Triggers**
- Update stock automatically as sales occur
- When a product is deleted from PRODUCT, remove its corresponding STOCK record

**Reports**
- Daily sales report: product description, category, quantity, amount
- Products whose quantity is ≤ their re-order level (ROL)
- Sales report for a specified month
- Products grouped by their suppliers
- Description & price of products purchased by a specific customer (e.g. C1)
- Description, price & quantity of products sold today

## Employee Schema (Sheet3 / One)

A separate practice schema, shown twice — once with constraints (Sheet3), once with data types (One).

### EMP
| Field | Constraint (Sheet3) | Data Type (One) |
|---|---|---|
| EMPID | Primary Key | CHAR (e.g. E001) |
| NAME | Not Null | VARCHAR |
| ADDR | No employees from Uttam Nagar | VARCHAR |
| CITY | One of: DEL, GGN, FBD, NOIDA | VARCHAR |
| PHNO | Unique | VARCHAR (e.g. +91-9899245970) |
| EMAIL | Must be Gmail or Yahoo domain | VARCHAR |
| DOB | ≥ 1-Jan-1990 | DATETIME |

### EMP_SAL
| Field | Constraint (Sheet3) | Data Type (One) |
|---|---|---|
| EMPID | Foreign Key → EMP | CHAR |
| DEPT | One of: HR, MIS, OPS, IT ADMIN, TEMP | VARCHAR |
| DESI (Designation) | One of: ASSO, MGR, VP, DIR | VARCHAR |
| BASIC (salary) | ≥ 20000 | DECIMAL |
| DOJ (Date of Joining) | — | DATETIME |
| SMONTH | — | — |

Sample fields for a select query: `EID, NAME, DOB, DEPT, DESI, BASIC`.

The "One" sheet also notes a target database name (**TEST**), expected record counts (15 EMP records, 9 EMP_SAL records), and one sample query:

> Salary details of associates whose salary is greater than or equal to the maximum salary among managers — a self-join style subquery on `EMP_SAL` comparing `DESI = 'ASSOCIATE'` rows against `MAX(SALARY)` where `DESI LIKE '%MANAGER%'`.

## How to Use This Workbook

1. Use **Sheet1** (or **Sheet1 (2)**) as the source schema to create the Inventory tables (PRODUCT, SUPPLIER, CUSTOMER, ORDERS, STOCK, PURCHASE) with the listed keys and constraints.
2. Work through **INVENT Queries** top to bottom to build out the required constraints, joins, subqueries, functions, procedures, triggers, and reports.
3. Use **Sheet3** and **One** together to build the EMP / EMP_SAL tables — Sheet3 for constraints, One for data types — then run the sample salary-comparison query to verify the setup.
