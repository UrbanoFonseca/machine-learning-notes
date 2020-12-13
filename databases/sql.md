# SQL

### Relational Languages

SQL is actually composed by several languages:

| Language | Tasks |
| :--- | :--- |
| Data Manipulation \(DML\) | Commands that manipulate data stored in DB \(create,w update, delete\) |
| Data Definition \(DDL\) | Create Schemas, define things |
| Data Control \(DCL\) | Control access, users & permissions |
| Data Query \(DQL\) | Query information from DB \(read\) |

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
SELECT COUNT(DISTINCT player) FROM games;

SELECT player, AVG(points) AS avg_points
FROM games
GROUP BY player
HAVING avg_points > 20;

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

**LIKE** allows for string matching. `%` matches any substring \(one or more letters\), while `_` matches any one character.

Concatenation allows to join two or more strings together. It can be achieved with `||` for SQL-92, `+` for Microsoft SQL and `CONCAT(string1, string2)` for MySQL \(or just a space between strings`'fds' 'fdsf'`\).

### Output Redirection

You can store the results from a query into another table with **CREATE** or **INSERT** new tuples from a query into an existing table. Since the last will extend an already existing table, the schema of the query must mtach the table.

```sql
CREATE TABLE NicePlayers (
SELECT DISTINCT player FROM games);

INSERT INTO NicePlayers (
SELECT DISTINCT player FROM draft);
```

### **Output Control**

When previewing the query we can order the results with the **ORDER BY** clause. 

```sql
/* ORDER BY <column*> [ASC|DESC] */
SELECT player, points FROM games
ORDER BY points;

SELECT player, points FROM games
ORDER BY points DESC, player ASC;
```

Additionally, we can limit the number of tuples with **LIMIT**, with or without an **OFFSET.**

```sql
/* LIMIT  <count> [offset] */
SELECT player, points FROM games
ORDER BY points DESC
LIMIT 5 OFFSET 10
```

### Nested Queries

**ALL** guarantees that the condition satisfies the expresion for all rows in the subquery, **ANY** that satisfies for at least one row, **IN** is equivalent to =ANY\(\) and with **EXISTS** at least one row is returned.

```sql
/* get the highest points from a player also in the draft table */
SELECT player, points FROM games
WHERE points => ALL (
    SELECT points FROM draft
);

SELECT player, points FROM games
WHERE points IN (
  SELECT MAX(points) FROM draft
)
```

### **Special Keywords**

A start \* is a keyword that means "all attributes".



