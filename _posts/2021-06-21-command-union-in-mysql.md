---
layout: post
title:  "Command `UNION` in MySQL"
date:   2021-06-21 14:00:00 +0200
categories: []
---

Let's take a look at the command `UNION` in MySQL.

* `UNION` - is used for integrating queries:

```sql
< query 1 >
UNION [ALL]
< query 2 >
```

The `UNION` clause combines the results of two SELECT statements into a single result set. If the ALL parameter is given, all the duplicates of the rows returned are retained; otherwise the result set includes only unique rows. Note that any number of queries may be combined. Moreover, the union order can be changed with parentheses.

The following conditions should be observed:

* The number of columns of each query must be the same;

* Result set columns of each query must be compared by the data type to each other (as they follows);

* The result set uses the column names from the first query;

In examples `query 1` is query from table PC where colums: `model, price`, `query 2` is query from table Laptop where colums: `model price` too:

```sql
SELECT model, price
FROM PC
UNION
SELECT model, price
FROM Laptop
ORDER BY price DESC;
```

Within the standard of  SQL language there are clauses of SELECT statement for fulfillment of operations of intersection and subtraction of the queries. Such queries are INTERSECT  [ALL] (intersection) and EXCEPT [ALL] (subtraction), which operate analogously to UNION statement. Into the resulting set only those rows are included that are in the both queries (INTERSECT) and only those rows of the first query, that are absent in the second one (EXCEPT). Thereby both of the queries, that are involved in the operation, should be characterized by the same number of columns, and the corresponding columns should have the same (or implied) data types. The titles of the columns of the resulting set are formed from the titles of the first query.

If the key word ALL is not used, that the duplicate strings should be automatically canceled during fulfillment of the operation. If ALL is indicated, than the number of duplicate rows is subject to the following rules (n1 – the number of duplicate rows of the first query, n2 – the number of duplicate rows of the second query):

INTERSECT ALL: min(n1, n2)
EXCEPT ALL: n1 - n2, if n1>n2.

In examples `query 1` is query from table Ships where column: `name`, `query 2` is query from table Outcomes where colums: `ship`:

```sql
SELECT name FROM Ships
INTERSECT
SELECT ship FROM Outcomes;
```
and
```sql
SELECT ship FROM Outcomes
EXCEPT
SELECT name FROM Ships;
```