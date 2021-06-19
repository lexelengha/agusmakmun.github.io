---
layout: post
title:  "Command `JOIN` in MySQL"
date:   2021-06-18 19:15:00 +0200
categories: []
---

Let's take a look at the command `JOIN` in MySQL.

* `JOIN` - operation of joining two or more tables, which can be beat, is indicated in the FROM clause.
The syntax for a predicate join is:

```sql
FROM <table 1>
 {LEFT | RIGHT | FULL} JOIN <table 2>
[ON <predicate]
```

In examples `table 1` is Product, `table 2` - PC, where in table `Product` we have colums, like `id, maker, model`, and in table `PC` - `id, model, price`. 

An outer join is defined by its type - LEFT (left), RIGHT (right) or FULL (full), and simply JOIN will mean an inner join. 
* The outer join LEFT JOIN means that in addition to the rows for which the predicate condition is satisfied, all other rows from the first table (left) will be included in the result set, and the missing column values from the right table will be replaced with NULL values.

```sql
SELECT maker, price
FROM Product LEFT JOIN 
 PC ON PC.model = Product.model
```

* The RIGHT JOIN is the opposite of the LEFT JOIN, that is, all rows from the second table will be included in the result set, which will only be joined to those rows from the first table for which the join condition is met.    

```sql
SELECT maker, price
FROM Product RIGHT JOIN 
 PC ON PC.model = Product.model
```

* With a full join (FULL JOIN), the resulting table will contain not only those rows that have the same values in the matched columns, but also all other rows of the source tables that do not have corresponding values in the other table. In these rows, all columns of the table in which no match was found are filled with NULL values. That is, a full join is a combination of the left and right outer joins.

```sql
SELECT maker, price
FROM Product FULL JOIN 
 PC ON PC.model = Product.model
 ```

 But what to do if it happened that you have the same column names but with different variables in the tables that need to be displayed when merging them? This issue can be solved very easily. When specifying a column, you need to write the name of the table from which you want to enter data, for example:
 
 ```sql
 SELECT Product.id, PC.id, price
 FROM Product JOIN 
 PC ON PC.model = Product.model
 ```