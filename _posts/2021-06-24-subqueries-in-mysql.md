---
layout: post
title:  "Subqueries in MySQL"
date:   2021-06-23 15:00:00 +0200
categories: []
---

# Briefly about subqueries

It should be noted that a query returns generally a collection of values, so a run-time error may occur during the query execution if one uses the subquery in the `WHERE` clause without `EXISTS`, `IN`, `ALL`, and `ANY` predicates, which result in Boolean value.

```sql
SELECT DISTINCT model, price
FROM PC
WHERE price > (SELECT MIN(price) 
 FROM Laptop
 );
```

This query is quite correct, i.e. the scalar value of the price is compared with the subquery which returns a single value. 

However, if in answer to question regarding the models and the prices of PCs that cost the same as a laptop one writes the following query:

 ```sql
 SELECT DISTINCT model, price
FROM PC
WHERE price = (SELECT price
 FROM Laptop
 );
 ```
The following error message will be obtained while executing the above query:

`Subquery returned more than 1 value. This is not permitted when the subquery follows =, !=, <, <= , >, >= or when the subquery is used as an expression.`

This error is due to comparison of the scalar value to the subquery, which returns either more that single value or none.

In its turn, subqueries may also include nested queries.

On the other hand, it is natural that subquery returning a number of rows and consisting of multiple columns may as well be used in the `FROM` clause. This restricts a column/row set when joining tables:

```sql
SELECT prod.maker, lap.*
FROM (SELECT 'laptop' AS type, model, speed
 FROM laptop
 WHERE speed > 600
 ) AS lap INNER JOIN 
 (SELECT maker, model
 FROM product
 ) AS prod ON lap.model = prod.model;
```

And finally, queries may be present in the `SELECT` clause. Sometimes, this allows a query to be formulated in a shorthand form:

```sql
 SELECT (SELECT AVG(price)
 FROM Laptop
 ) -
 (SELECT AVG(price)
 FROM PC
 ) AS dif_price;
```

