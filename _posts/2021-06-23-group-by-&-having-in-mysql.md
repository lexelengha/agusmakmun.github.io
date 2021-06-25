---
layout: post
title:  "`GROUP BY` & `HAVING` in MySQL"
date:   2021-06-23 15:00:00 +0200
categories: []
---

#GROUP BY

The `GROUP BY` clause is used to define the row groups for each of the aggregate functions `(COUNT, MIN, MAX, AVG, and SUM)` that may be applied. When aggregate functions are used without a `GROUP BY` clause, all the columns with names mentioned in `SELECT` clause must be included in the aggregate functions. These functions are then applied to the total set of the rows that fit the query predicate. Otherwise, all columns in the `SELECT` list not included into the aggregate functions must be listed in the `GROUP BY` clause. As a result, all the returned query rows distributed into groups are characterized by the same combinations of these column values. Later on, aggregate functions are applied to each group. It is essential that `NULL` values are considered equal in this case, i.e. when grouping by the column including `NULL` values all rows will be combined in one group.

When a `GROUP BY` clause is used without any aggregate function in the `SELECT` clause, the query will simply return one row from each group. Beside the `DISTINCT` keyword, this opportunity may be used in eliminating the row duplicates from the result set.

```sql
SELECT model, COUNT(model) AS Qty_model, AVG(price) AS Avg_price
FROM PC
GROUP BY model;
```
The number of computers and their average price are defined for each PC model in the query. All rows with the same model value are combined in a group with value count and the average price calculated for each group thereafter. 

There are some particular rules for executing aggregate functions:

 * If none of the rows was returned by the query (or none of the rows for the given group), the source data for any aggregate function to be calculated is missing. In such case, the `COUNT` function returns zero, while other functions return `NULL`.

 * The argument of the aggregate function cannot include aggregate functions itself (the function of function) i.e. no maximum of average values is obtainable.

 * The result of the `COUNT` function is integer. Other aggregate functions inherit the types of processing data.

 * An error occurs where the result is over the maximal value of the used data type while executing `SUM` function.

 In so doing, if the query does not include `GROUP BY` clause, the aggregate functions in the `SELECT` clause process all the result rows of this query. If the query includes the `GROUP BY` clause, each row set with the same value in the column or the same combination of values in several columns given in the `GROUP BY` clause forms a group with the aggregate functions being calculated for each group separately.

#HAVING

While `WHERE` clause gives predicate for filtering rows, the `HAVING` clause is applied after grouping that gives a similar predicate but filtering groups by the values of aggregate functions. This clause is nessesary for checking the values that are obtained by means of an aggregate function not from separate rows of record source in the `FROM` clause but from the groups of these rows. Therefore, this checking is not applicable to the `WHERE` clause.

```sql
SELECT model, COUNT(model) AS Qty_model, AVG(price) AS Avg_price
FROM PC
GROUP BY model
HAVING AVG(price) < 800;
```

Note that the alias `(Avg_price)` for naming values of the aggregate function in the `SELECT` clause may not be used in the `HAVING` clause. This is because the `SELECT` clause forming the query result set is executed last but before the `ORDER BY` clause. Below is the execution order of clauses in the `SELECT` statement:

```sql
1.FROM
2.WHERE
3.GROUP BY
4.HAVING
5.SELECT
6.ORDER BY
```

This order does not correspond to the syntax order of `SELECT` operator, which is more closer to native language and is generally formed as follows:

```sql
SELECT [DISTINCT | ALL] {*
  | [<column expression> [[AS] <alias>]] [,…]}
FROM <table name> [[AS] <alias>] [, ...]
[WHERE <predicate>]
[[GROUP BY <column list>]
[HAVING <aggregate condition>]]
[ORDER BY <column list>]
```

Note that `HAVING` clause can be also used without `GROUP BY` clause. When `GROUP BY` clause is omitted, aggregate functions are applying to all target row set of the query.