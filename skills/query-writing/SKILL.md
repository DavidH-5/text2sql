---
name: query-writing
description: For writing and executing SQL queries - from simple single-table queries to complex multi-table JOINs and aggregations
---

# Query Writing Skill

## When to Use This Skill

Use this skill when you need to answer a question by writing and executing a SQL query.

## Mandatory Customer Scoping

You MUST scope every SQL query to the active customer.

The active customer ID is provided in runtime context as:

CustomerID = {{context.CustomerID}}

### Rules (NON-NEGOTIABLE)

- Every query MUST include a WHERE clause filtering on customer ID
- You MUST use the literal value from context, not a placeholder
- NEVER omit the customer filter
- NEVER return data for other customers

### Required SQL patterns

For single-table queries:
```sql
WHERE <table_or_alias>.CustomerID = '{{context.CustomerID}}'
```

## Workflow for Simple Queries

For straightforward questions about a single table:

1. **Identify the table** - Which table has the data?
2. **Get the schema** - Use `sql_db_schema` to see columns
3. **Write the query** - SELECT relevant columns with WHERE/LIMIT/ORDER BY. Ensure filter on customer id column
4. **Execute** - Run with `sql_db_query`
5. **Format answer** - Present results clearly

## Workflow for Complex Queries

For questions requiring multiple tables:

### 1. Plan Your Approach
**Use `write_todos` to break down the task:**
- Identify all tables needed
- Map relationships (foreign keys)
- Plan JOIN structure
- Determine aggregations

### 2. Examine Schemas
Use `sql_db_schema` for EACH table to find join columns and needed fields.

### 3. Construct Query
- SELECT - Columns and aggregates
- FROM/JOIN - Connect tables on FK = PK
- WHERE - Filters before aggregation and filter on customer id
- GROUP BY - All non-aggregate columns
- ORDER BY - Sort meaningfully
- LIMIT - Default 5 rows

### 4. Validate and Execute
Check all JOINs have conditions, GROUP BY is correct, then run query.

## Example: My purchase in November
```sql
SELECT
    c.CustomerID,
    c.CompanyName,
    o.OrderID,
    o.OrderDate,
    od.ProductID,
    p.ProductName,
    od.Quantity,
    od.UnitPrice,
    od.Discount,
    (od.Quantity * od.UnitPrice * (1 - od.Discount)) AS LineTotal
FROM Customers c
JOIN Orders o
    ON c.CustomerID = o.CustomerID
JOIN "Order Details" od
    ON o.OrderID = od.OrderID
JOIN Products p
    ON od.ProductID = p.ProductID
WHERE c.CustomerID = 'AESPA'
  AND strftime('%m', o.OrderDate) = '11'
ORDER BY o.OrderDate
LIMIT 5;
```

## Quality Guidelines

- Always query on customerID provided
- Query only relevant columns (not SELECT *)
- Always apply LIMIT (5 default)
- Use table aliases for clarity
- For complex queries: use write_todos to plan
- Never use DML statements (INSERT, UPDATE, DELETE, DROP)