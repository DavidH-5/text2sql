---
name: schema-exploration
description: For discovering and understanding database structure, tables, columns, and relationships
---

# Schema Exploration Skill

## When to Use This Skill

Use this skill when you need to:
- Understand the database structure
- Find which tables contain certain types of data
- Discover column names and data types
- Map relationships between tables
- Answer questions like "What tables are available?" or "What columns does the Customer table have?"

## Workflow

### 1. List All Tables
Use `sql_db_list_tables` tool to see all available tables in the database.

This returns the complete list of tables you can query.

### 2. Get Schema for Specific Tables
Use `sql_db_schema` tool with table names to examine:
- **Column names** - What fields are available
- **Data types** - INTEGER, TEXT, DATETIME, etc.
- **Sample data** - 3 example rows to understand content
- **Primary keys** - Unique identifiers for rows
- **Foreign keys** - Relationships to other tables

### 3. Map Relationships
Identify how tables connect:
- Look for columns ending in "Id" (e.g., CustomerId, ArtistId)
- Foreign keys link to primary keys in other tables
- Document parent-child relationships

### 4. Answer the Question
Provide clear information about:
- Available tables and their purpose
- Column names and what they contain
- How tables relate to each other
- Sample data to illustrate content

## Example: "What tables are available?"

**Step 1:** Use `sql_db_list_tables`

**Response:**
```
The Northwind database has 13 tables:
1. CustomerDemographics - customer ddemographic 
2. CustomerCustomerDemo - bridge table between Customers and CustomerDemographics
3. Customers - customer infomration such as name, address, contact number etc
4. Orders - customer orders
5. Employees - employee information
6. EmployeeTerritories - bridge table between Employees and EmployeeTerritories 
7. Territories - employee territories
8. Regions - employee regions
9. Shippers - shippers
10. Order Details - bridge table between Orders and Products
11. Products - product information
12. Suppliers - product supplier
13. Categories - product category
```

## Example: "What columns does the Customer table have?"

**Step 1:** Use `sql_db_schema` with table name "Customer"

**Response:**
```
The Customer table has these columns:

-	CustomerID (INTEGER) - Primary key, unique customer ID
-	FirstName	(NVARCHAR) - Customer's first name
-	LastName	(NVARCHAR) - Customer's last name
-	Company	(NVARCHAR) - Customer's company name
- Address, City, State, Country, PostalCode - Location fields
- Phone (NVARCHAR) - Phone number
- Fax (NVARCHAR) - Fax number
- Email (NVARCHAR) - Email address
- SupportRepId (INTEGER) - Foreign key to Employee table

Sample data shows customers like:
- Luís Gonçalves from Brazil
- Leonie Köhler from Germany
- François Tremblay from Canada
```

## Example: "How do I find my order by shipper?"

**Step 1:** Identify tables needed
- Customers (has customer ID)
- Orders (has purchase history)
- Shippers (links to orders)
- Order Details (links to orders and products)
- Pruducts (has product information)

**Step 2:** Map relationships
```
Customers (CustomerID)
  ↓ 1:many
Orders (CustomerID, OrderID)
  ↓ 1:1
Shippers (ShipVia, ShipperID)

Orders (CustomerID, OrderID)
  ↓ 1:1
Order Details (OrderID, OrderID)

Order Details (OrderID, ProductID)
  ↓ 1:many
Products (ProductID)
```

**Response:**
```
To find customer orders by shipper, you'll need to:
1. Start with Customers table to filter on customerID
2. JOIN to Orders on CustomerID
3. JOIN to Shippers on ShipVia and ShipperID column
4. JOIN to Order Details on OrderID
5. JOIN to Products on ProductID
6. List result by shipper

This requires the query-writing skill to execute.
```

## Quality Guidelines

**For "list tables" questions:**
- Show all table names
- Add brief descriptions of what each contains
- Group related tables (e.g., customers, orders, products)

**For "describe table" questions:**
- List all columns with data types
- Explain what each column contains
- Show sample data for context
- Note primary and foreign keys
- Explain relationships to other tables

**For "how do I query X" questions:**
- Identify required tables
- Map the JOIN path
- Explain the relationship chain
- Suggest next steps (use query-writing skill)

## Common Exploration Patterns

### Pattern 1: Find a Table
"Which table has customer information?"
→ Use list_tables, then describe Customers table

### Pattern 2: Understand Structure
"What's in the Orders table?"
→ Use schema tool to show columns and sample data

### Pattern 3: Map Relationships
"What are suppliers connected to orders?"
→ Trace the foreign key chain: Suppliers → Products → Order Details → Orders

## Tips

- Table names in Northwind are plural and capitalized (Customers, not customer)
- Use sample data to understand what values look like
- When unsure which table to use, list all tables first