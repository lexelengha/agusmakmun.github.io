---
layout: post
title:  "Getting summarizing values in MySQL"
date:   2021-06-22 11:00:00 +0200
categories: []
---

How to find the number of columns in a table? How to determine their average value? These and many other questions related to some statistical information can be answered using summary (aggregate) functions. The standard preview of the following aggregate functions:


| function | description |
|---|---|
| COUNT(*) | Returns the number of rows of the table |
| COUNT | Returns the number of values in the specified column | 
| SUM |	Returns the sum of values in the specified column |
| AVG |	Returns the average value in the specified column |
| MIN |	Returns the minimum value in the specified column |
| MAX |	Returns the maximum value in the specified column |

All these functions return a single value. In so doing, the functions `COUNT`, `MIN`, and `MAX` are applicable to any data types, while the functions `SUM` and `AVG` are only used with numeric data types. The difference between the functions `COUNT(*)` and `COUNT(<column name>)` is that the second does not calculate NULL values (as do other aggregate functions).

```sql
SELECT MIN(price) AS Min_price, MAX(price) AS Max_price
FROM PC;
```

To use only unique values in calculating the statistic, the parameter `DISTINCT` with an aggregate function argument may be used. `ALL` is another (default) parameter and assumes that all the column values returned (besides NULLs) are calculated.

If we need the number of PC models produced by each maker, we will need to use the `GROUP BY` clause, placed immediately after the `WHERE` clause, if any.