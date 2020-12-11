# SQL

### Relational Languages

SQL is actually composed by several languages:

| Language | Tasks |
| :--- | :--- |
| Data Manipulation \(DML\) | Commands that manipulate data stored in DB |
| Data Definition \(DDL\) | Create Schemas, define things |
| Data Control \(DCL\) | Security authorizations |

> SQL is based on **bags**, instead of lists or set theory

| Collection | Duplicates | Ordered |
| :--- | :--- | :--- |
| Set | ✗ | ✗ |
| Lists | ✓ | ✓ |
| Bags | ✓ | ✗ |

### 

### Aggregations

Functions that return a single value from a bag of tuples.

_Examples: Avg, Min, Max, Sum, Count_

**DISTINCT** enables you to only consider distinct values from the attributes.

**GROUP BY** allows to project the tuples into subsets and calculate aggregations for each subset.

**WHERE** clause allows to filter the tuples before we compute the aggregation. If we want to use the result of the aggregations to subset further the result tuples, we need to use the **HAVING** clause.

```sql
SELECT COUNT(DISTINCT player) FROM games
```

### String Operations

|  | String Case | String Quotes |
| :--- | :--- | :--- |
| SQL-92 | Sensitive | Single |
| Postgres | Sensitive | Single |
| MySQL | Insensitive | Single/Double |
| SQLite | Sensitive | Single/Double |
| DB2 | Sensitive | Single |
| Oracle | Sensitive | Single |

### **Special Keywords**

A start \* is a keyword that means "all attributes".



