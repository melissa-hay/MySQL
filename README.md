# Definitions
***Composite primary key***<br>
  When a primary key uses two or more columns<br>
  
***Constraint***<br>
  When assigning column attributes, a _constraint_ restricts the type of data that can be stored in a column. For example, **NOT NULL** and **UNIQUE**.<br>
  
***Data type*** <br>
  Determines the type of information that is stored in the column. Try to assign the data type that minimizes the use of disk storage because that improves the performance of     queries later.<br>
- CHAR, VARCHAR
- INT, DECIMAL
- FLOAT
- DATE 

***Foreign key*** <br>
One or more columns in a table that refer to a primary key in another table (one-to-many relationship). 
Note that foreign keys can have duplicates while primary keys cannot !



# column aliases

# Language structure 
## Schema Object Names 
### Identifier Quotes

Certain objects within MySQL, including database, table, index, column, alias, view, stored procedure, partition, tablespace, resource group and other object names are known as **identifiers**. This section describes the permissible syntax for identifiers in MySQL. 

An identifier may be quoted or unquoted. If an identifier contains special characters or is a reserved word, you must quote it whenever you refer to it. (Exception: A reserved word that follows a period in a qualified name must be an identifier, so it need not be quoted.) 

The identifier quote character is the backtick (`):

# Joins 
By default, joins in SQL are “inner joins” which is common one.

Consider the 2 schemas:
-	Product(name , category)
-	Purchase(id , prodName , store)

![image](https://user-images.githubusercontent.com/49015081/137153412-5664435c-f79f-4613-9a84-80aeaadad285.png) <br>

Generally speaking, Joins are based on primary key == foreign key

## Self Join
A self-join is joining a table to itself. When a relation occurs twice in the `FROM` clause we call it a self-join.

**Syntax (2 syntaxes similar to with inner joins)**
| Syntax 1| Syntax 2 |  
| ------------- |-------------| 
| 
~~~~mysql
SELECT column_name(s) 
FROM table1 T1 JOIN table1 T2
ON condition(s); 
~~~~| 
~~~~mysql
SELECT column_name(s) 
FROM table1 T1, table1 T2
WHERE condition;
~~~~ |

- (Note: T1 and T2 are different table aliases for the same table.)
- Self Join is a special case of inner join

### When to use Self join?
-	You use a self join when a table references data in itself.
-	E.g., an Employee table may have a SupervisorID column that points to the employee that is the boss of the current employee.
-	To query the data and get information for both people in one row, you could self join like this:  
![image](https://user-images.githubusercontent.com/49015081/137163831-15e10999-e2e7-4fe1-917f-4ed83329cea5.png) <br>
- It's basically used where there is any relationship between rows stored in the same table. 
**Self Join Example**
~~~~mysql
SELECT DISTINCT v1.vendor_name, v1.vendor_city, v1.vendor_state
FROM vendors v1 JOIN vendors v2
ON (v1.vendor_city = v2.vendor_city) AND (v1.vendor_state = v2.vendor_state) AND (v1.vendor_id <> v2.vendor_id)
ORDER BY v1.vendor_state, v1.vendor_city;
~~~~ 


## Cross Join
`CROSS JOIN` keyword returns all records from both tables; it is used to join two or more unrelated tables <br>
![image](https://user-images.githubusercontent.com/49015081/137162420-c7affc5e-5195-420b-a2cf-a676e6097545.png) <br>

**Syntax for the CROSS JOIN of two tables**
~~~~mysql
SELECT T1.column_name(s), T2.column_name(s)
FROM table1 T1
CROSS JOIN table2 T2;
[ORDER BY column_names(s) DESC];
~~~~
- T1 and T2 are table aliases
-	The order by is optional 

Cross join selects the all the rows from the first table and all the rows from second table and shows the result as a Cartesian product (i.e. cross product) with all possibilities.
- When to use /Purpose of cross join: anytime you want to obtain all possible combinations from 2 tables

Unlike the `INNER JOIN` or `LEFT JOIN`, the cross join does not establish a relationship between the joined tables.

Suppose the T1 table contains three rows 1, 2, and 3 and the T2 table contains three rows A, B, and C.

The `CROSS JOIN` gets a row from the first table (T1) and then creates a new row for every row in the second table (T2). It then does the same for the next row for in the first table (T1) and so on. <br>
![image](https://user-images.githubusercontent.com/49015081/137162798-5be0517d-8960-4628-bfb4-d9af0c3d4206.png) <br>
- In this illustration, the `CROSS JOIN` creates nine rows in total. 
- In general, if the first table has n rows and the second table has m rows, the cross join will result in n x m rows.


**Example**
Consider the schemas for 2 relations:
- Teacher (TchrId, teacherName) 
- Student (StudId, TchrId, studentName)

~~~~mysql
SELECT T.TchrId, T.TeacherName, S.StudentName
FROM Teacher T
CROSS JOIN Student S;
~~~~
![image](https://user-images.githubusercontent.com/49015081/137162965-4b555595-55b4-41b3-ac6e-6d824a129d8f.png)


# Foreign-key constraints 
Also known as a reference constraint. This constraint requires values in one tabvle to match values in another table. Used to define the relationship between tables and to enforce referential integrity.<br>

A foreign key constraint has 2 conditions. For column X in table1 to be a foreign key for table2: 
1.	X and Y may be lists of attributes; must be of same arity (same # of attributes/columns)
2.	Y must be a primary key in table2. 

Constraints can be specified when the table is created with the `CREATE TABLE` statement, or after the table is created with the `ALTER TABLE` statement.<br>


## Column-level foreign key constraint
To create a foreign key constraing at the column level, you code the `REFERENCES` keyword followed by the name of the related table and the name of the related column in parentheses.

Suppose we have a table called `Persons` whose **primary key** is `PersonID`. We now create another table called `Orders` with **primary key** `OrderID` and **foreign key** `PersonID` that references the primary key of `Persons`. 

~~~~mysql
CREATE TABLE Orders (
    OrderID int NOT NULL PRIMARY KEY,
    OrderNumber int NOT NULL,
    PersonID int FOREIGN KEY REFERENCES Persons(PersonID)
);
~~~~

To create a Foreign key constraint on the `PersonID` column when the `Orders` table is already created, use the following SQL:

~~~~mysql
ALTER TABLE Orders
ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
~~~~

## Table-level foreign key constraint
To create a foreign key constraing at the table level, include the `CONSTRAINT` keyword followed by an alias, followed by the `FOREIGN KEY` keywords. This allows you to provide an alias for the foreign key, which is a best practice.<br>

~~~~mysql
CREATE TABLE Orders(
    OrderID int NOT NULL PRIMARY KEY,
    OrderNumber int NOT NULL,
    PersonID int NOT NULL,
    CONSTRAINT orders_fk_persons
      FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
~~~~
# SQL Operators 
- SQL Arithmetic Operators: +, -, *, /, %
- SQL Bitwise Operators: & (and), |(or), ^ (xor)
- SQL Comparison Operators: =, >, <, >=, <=, <> (not equal to)
- SQL Logical Operators: AND, OR, EXISTS, IN, LIKE, SOME…

## Logical Operators

### `LIKE` operator: pattern matching 
`LIKE` is used in a `WHERE` clause to search for a specified pattern in a column.

You can use two wildcards with `LIKE`:
- `%` matches any number of characters: zero, one, or multiple characters 
- `_` matches exactly 1 character

These can be used in combinations! You can also combine any number of conditions using `AND` or `OR` operators.

**Syntax**
~~~~mysql
SELECT * FROM table_name
WHERE column_name LIKE pattern;
~~~~
<br>

**Wildcard descriptions**
![image](https://user-images.githubusercontent.com/49015081/136250727-e1106a5d-787b-4c11-8269-ebcfac4dd1c3.png)

Here are some examples showing different LIKE operators with '%' and '_' wildcards:
![image](https://user-images.githubusercontent.com/49015081/136250456-5c364dd5-21cb-4eb1-b841-a470417c3745.png)

## Bitwise Operators
The operant i.e. input in SQL bitwise must be numbers.
- A bitwise AND is a binary operation that takes 2 equal-length (in binary) integers, converts them (under the hood) into binary representation and then performs the logical AND operation on each pair of the corresponding bits (which is equivalent to multiplying them. Thus, if both bits in the compared position are 1, the bit in the resulting binary representation is 1 (1 × 1 = 1); otherwise, the result is 0 (1 × 0 = 0 and 0 × 0 = 0). 
- Other bitwise operators work in a similar fashion

![image](https://user-images.githubusercontent.com/49015081/137151309-60a542b1-2c03-4c35-89dd-3d9642e7b49c.png) <br>

> What is the difference between logical AND and & (bitwise and)?
> - Logical AND is based on the input datatype: T and F. 
>   - Output is thus also either: T or F
> - Bitwise AND is based on the input datatype: numbers, usually integers; it can be represented in memory as bits (which can be a signed int, unsigned int, char, etc.)
>   - Output is also an int. 
>	Thus output and input datatypes differ

## Comparison Operators
SQL Comparison Operators exceed their normal arithmetic meanings. In other words, there is more flexibility– can compare various different types of values. 

**example**
~~~~mysql
SELECT COUNT(*)
FROM invoices
WHERE invoice_date > '2020 08 01'
~~~~

#NULL Value
Special value that means:
- unavailable
- unassigned
-	unknown
-	inapplicable

`NULL` is Not equivalent to:
-	zero
-	blank space

Whenever we don’t have a value, we can put a `NULL`

For numerical operations, NULL --> NULL: 
-	If x = NULL then 4*(3 - x)/7 is still NULL

For boolean operations, in SQL there are three values:
-	False = 0
-	Unknown = decimal number for example 0.5
  - Unknown could be NULL
-	True = 1

Can test for NULL explicitly:
-	`x IS NULL`
-	`x IS NOT NULL`

**Example**
~~~~mysql
SELECT *
FROM Person
WHERE age <25 OR age >= 25 OR age IS NULL
~~~~

# Agreggate functions 
**Aggregate function** is used to accumulate information from multiple tuples, forming a single tuple summary. It is used to perform calculations on a set of values (multiple values) and return the result as a single scalar value like the average of all values, the sum of all values, and maximum & minimum value among certain groups of values. We mostly use the aggregate functions with the `GROUP BY` and `HAVING` clauses of the `SELECT` statement.

> Note: aggregators only aggregate vertically. If you want to perform a calculation across rows, you would do this with simple arithmetic. <br>

Five basic Built-in aggregate functions in SQL: 
-	`COUNT` – returns # **rows** in a specified table or in a particular column, including rows with NULL values that matches a specified criterion
-	`MIN` – returns the minimum value of non-NULL values in a **column**
-	`MAX` – returns the maximum value of non-NULL values in a **column** 
-	`SUM` – adds together all non-NULL values in a particular **column** 
-	`AVG` – calculates the average of non-NULL values in a group of selected values.


Notice that all aggregate functions above ignore `NULL` values except for the `COUNT` function.

## COUNT
**Syntax**
~~~~mysql
SELECT column_name1, count(*) 
FROM table 
[WHERE conditions]
GROUP BY column_name1;
~~~~
- Used to count # of rows in `column_name1`  if the rows satisfy  the conditions in the `WHERE` clause. 
- Anytime `COUNT()` is placed after a `column_name` in the `SELECT` clause, `COUNT()` is applied to that column. 
- If you name columns to select in addition to the `COUNT()` value, a `GROUP BY` clause should be present that names those same columns. 

**Example:** Use COUNT() if you want to find out how many pets each owner has <br>
![image](https://user-images.githubusercontent.com/49015081/137157044-1ae94ea3-c7be-4827-9860-031dd009ad5c.png) <br>
- The preceding query uses `GROUP BY` to group all records for each owner. 

**Example:** Use count() to get Number of animals per combination of species and sex <br>
![image](https://user-images.githubusercontent.com/49015081/137157244-15dda415-95b4-4c65-9393-1dd6c1c7bc3e.png) <br>

You need not retrieve an entire table when you use COUNT(). For example, the previous query, when performed just on dogs and cats, looks like this:<br>
![image](https://user-images.githubusercontent.com/49015081/137157305-7bf1d54a-96e5-4695-acf1-fae8a054ea05.png) <br>

## SUM
An important thing to remember: aggregators only aggregate vertically. If you want to perform a calculation across rows, you would do this with simple arithmetic. 
-	You don't need to worry as much about the presence of nulls with SUM as you would with COUNT, as SUM treats nulls as 0.

**Examples ** 
~~~~mysql
SELECT SUM(order_qty * unit_price) 
FROM orderdetails, item 
WHERE orderdetails.item_id= item.item_id;
~~~~
## MIN and MAX
-	They're similar to COUNT in that they can be used on non-numerical columns. 
-	Depending on the column type, MIN will return the lowest number, earliest date, or non-numerical value as close alphabetically to "A" as possible. 
-	MAX does the opposite—it returns the highest number, the latest date, or the non-numerical value closest alphabetically to "Z."

**Example ** 
~~~~mysql
SELECT MIN(invoice_total ) AS lowest_invoice_total ,
MAX(invoice_total ) AS highest_invoice_total , 
COUNT(*) AS number_of_invoices
FROM invoices;
~~~~
- Note use of `AS` which saves the results from the aggregate function as an alias. 
-	Aggregate functions only accept 1 parameter (cannot separate parameters by commas) but can use arithmetic operators to include more than 1 parameter (ex: min(column1 – column2) 


**Example:** count all rows in individual columns <br>
The following code will provide a count of all of rows in which the `invoice_number` column is not null.<br>
~~~~mysql
SELECT count( invoice_number ) 
FROM invoices 
WHERE invoice_total >= 300;
~~~~

Can also select distinct entries (ex: distinct invoice numbers). This will return the count w/o repeated values: 
~~~~mysql
SELECT count(DISTINCT invoice_number) 
FROM invoices 
WHERE invoice_total >= 300;
~~~~

## Agreggate functions and JOIN
- An aggregate function performs a calculation on a set of values, and returns a single value.
- A `JOIN` clause is used to combine rows from two or more tables, based on a related column between them. 

We can combine aggregate functions with Inner joins

**Example**
Consider the schemas for 2 relations:
- Product( pid , pname , manufacturer)
- Purchase( id , product_id , price, month)
  - `Product_id` is the foreign key that refers to the primary key, `pid` in the `Product` table

Problem: For each manufacturer, compute how many products with price > $100 they sold
- Step 1– first condition in `WHERE` clause makes sure that the primary key = foreign key; 2nd condition in `WHERE` clause makes sure that price > $100. We want both conditions to be satisfied at same time --> via `AND` logical operator
- Step 2– combines the `count(*)` function with an inner join and uses `GROUP BY` 

![image](https://user-images.githubusercontent.com/49015081/137162168-eae7ea0d-8151-46d6-a489-65598c9e83bd.png) <br>

Problem: For each manufacturer, compute how many products with price > $100 they sold in each month? <br>
![image](https://user-images.githubusercontent.com/49015081/137162241-acfe5c75-4c5b-4c5d-9274-d44a7d134eb9.png) <br>


# `GROUP BY` clause
_**What**_<br>
- The `GROUP BY` statement is used with the `SELECT` statement to arrange identical data into groups.
- The `GROUP BY` statement groups rows that have the same values into summary rows, like "find the number of customers in each country".
- It is often used with aggregate functions (`COUNT()`, `MAX()`, `MIN()`, `SUM()`, `AVG()`) to group the result-set by one or more columns.

**Syntax of `GROUP BY` with 'WHERE' clause**
~~~~mysql
SELECT column_name(s), [aggregate function(column_name)]
FROM table_name
WHERE condition
GROUP BY column_name1 [, column_name2,…] 
ORDER BY column_name [, column_name2,…] 
~~~~
- `GROUP BY column_name1` is the clause that performs the grouping based on `column_name1`.
-	`[,column_name2,…]` is optional; represents other column names when the grouping is done on more than one column.
  - If grouping done on many columns, it first groups by column_name1 and then by column_name2

## Handling of Group-by with aggregate functions 
SQL-92 and earlier does not permit queries for which the `SELECT` clause, `HAVING` condition, or `ORDER BY` clause refer to non-aggregated columns (i.e. columns where you didn’t apply an aggregate function to in the query) that are not named in the `GROUP BY` clause. 
> For example, this query is illegal in standard SQL-92 because the non-aggregated name column in the `SELECT` clause does not appear in the `GROUP BY`: <br>
> ![image](https://user-images.githubusercontent.com/49015081/136260211-ff3938bc-d78c-44e2-9516-80871db5606a.png)
> - `o.custid` is also a non-agreggated column but causes no issues since we `GROUP BY o.custid`
> For the query to be legal in SQL-92, the `name` column **must be omitted** from the `SELECT` clause **or included in** the `GROUP BY` clause.

### General form of Grouping and Aggregation
~~~~mysql
SELECT a1, …, ak, [aggregate function(column_name)]
FROM R1, …, Rn
WHERE C1
GROUP BY a1, …, ak
HAVING C2
~~~~
- C1 : is any condition on the attributes in relations R1, …, Rn
- C2 : is any condition on **aggregate expressions** and on attributes a1, …, ak
- `[HAVING C2]` is optional; it is used to restrict the rows affected by the `GROUP BY` clause. It is similar to the `WHERE` clause.


**Semantics of SQL with `GROUP BY`**
1.	Evaluate `FROM` `WHERE` using Nested Loop Semantics
2.	`GROUP BY` by the attributes a1, …, ak
3.	Apply condition C2 to each group (may have aggregates)
    - `HAVING` is evaluated after `GROUP BY`
5.	Compute aggregates in S and return the result


# `HAVING` clause
_**What**_<br>
 The `HAVING` clause specifies a search condition for a group or an aggregate.
- `HAVING` clause can be used only with the `SELECT` statement.
- It is only applicable if `GROUP BY` is involved
- `HAVING` clause was added to SQL b/c aggregate functions cannot be used with `WHERE` keyword
- Use The `HAVING` clause when: you want to use aggregate functions in conditions

**Syntax**
~~~~mysql
SELECT column_names, [aggregate function(column_name)]
FROM table
GROUP BY column_name1[, column_name2,…] 
[HAVING conditions];
[ORDER BY column_name(s)]
~~~~
- `[HAVING condition]` is optional; it is used to restrict the rows affected by the `GROUP BY` clause. It is similar to the `WHERE` clause.
- In this syntax, the `GROUP BY` clause summarizes the rows into groups and the `HAVING` clause applies one or more conditions to these groups. Only groups that make the conditions evaluate to TRUE are included in the result. In other words, the groups for which the condition evaluates to FALSE or UNKNOWN are filtered out.

Because SQL Server processes the `HAVING` clause after the `GROUP BY` clause, you cannot refer to the aggregate function specified in the `SELECT` list by using its column alias. The following query will fail:
~~~~mysql
SELECT column_name1, column_name2, aggregate function(column_name3) as column_alias
FROM table
GROUP BY column_name1, column_name2 
HAVING column_alias > some_value;
~~~~
Instead, you must use the aggregate function expression in the `HAVING` clause explicitly as follows:
~~~~mysql
SELECT column_name1, column_name2, aggregate function(column_name3) as column_alias
FROM table
GROUP BY column_name1, column_name2 
HAVING aggregate function(column_name3) > some_value;
~~~~

### `WHERE` vs. `HAVING`
| `WHERE` Condition| `HAVING` Condition |
| ------------- |:-------------:| 
| is applied to individual rows | Is applied to an entire group of group |
| The rows may or may not contribute to the aggregate | Only applicable if GROUP BY is involved |
| No aggregates allowed in or after the WHERE clause | Entire group is returned, or removed |
| | May use aggregate functions on the group |

**Example**
~~~~mysql
SELECT vendor_name,
	COUNT(*) AS invoice_qty,                        (1)
    ROUND(AVG(invoice_total), 2) AS invoice_avg     (2)
FROM vendors JOIN invoices
	ON vendors.vendor_id = invoices.vendor_id
GROUP BY vendor_name                                (3)
HAVING AVG(invoice_total) > 500                     (4)
ORDER BY invoice_qty DESC;
~~~~
1. Calculates an invoice count for the vendor_name column (that is listed in the GROUP BY column)
2. Calculates an average invoice amount
3. Groups the invoices in the invoice table by vendor name
4. Limits the groups in the result set to those that have an average invoice total greater than 500

~~~~mysql
SELECT vendor_name,
	COUNT(*) AS invoice_qty,                        (1)
    ROUND(AVG(invoice_total), 2) AS invoice_avg     (2)
FROM vendors JOIN invoices
	ON vendors.vendor_id = invoices.vendor_id
WHERE invoice_total > 500                           (3)
GROUP BY vendor_name                                (4)
ORDER BY invoice_qty;
~~~~
1. Calculates an invoice count for the vendor_name column (that is listed in the GROUP BY column)
2. Calculates an average invoice amount
3. Limits the invoices in the groups to those that have an invoice total greater than 500. This is applied to every row before anything is grouped.
4. The results of the WHERE clause are grouped by vendor_name

**BEST PRACTICE** You can use either the `WHERE` or `HAVING` clause to code non-aggregate clauses, but it makes more sense to include them all in the `HAVING` clause for readability.

# Common table expressions (CTE)
A CTE is a `SELECT` statement that creates one or more named temporary result sets i.e. **aliases** that can be used by the query that follows. Use CTEs to simplify complex queries that use subqueries. The `WITH` clause will compute the aggregation once, give it a name, and allow us to reference it (maybe multiple times), later in the query.

CTEs begin with a `WITH` clause that has one or more comma-separated subclauses. Each subclause provides a subquery that produces a result set, and associates a name (alias) with the subquery. 
> The `WITH` clause is very confusing at first because the SQL statement does not begin with the word `SELECT`. Instead, we use the `WITH` clause to start our SQL query, defining the aggregations, which can then be named in the main query as if they were real tables.

**Syntax with 1 alias**
~~~~mysql
WITH alias 
AS (SELECT column_name(s) FROM table_name)
SELECT column_names(s) FROM alias [,table_name]
[WHERE <join_condition>]
~~~~
- Notice how the outer `SELECT` query uses the alias created in the `WITH` clause. 

Only one `WITH` clause is permitted at the same level so use a single `WITH` clause that separates the subclauses by a comma:
~~~~mysql
WITH cte1 AS (...), cte2 AS (...) SELECT ...
~~~~
**syntax**
~~~~mysql
WITH
  alias1 AS (SELECT column_name(s) FROM table1),
  alias2 AS (SELECT column_name(s) FROM table2)
SELECT b, d FROM alias1 JOIN alias2
WHERE <join_condition>;
~~~~

To specify common table expressions, use a WITH clause that has one or more comma-separated subclauses. Each subclause provides a subquery that produces a result set, and associates a name with the subquery. The following example defines CTEs named cte1 and cte2 in the WITH clause, and refers to them in the top-level SELECT that follows the WITH clause:
~~~~mysql
WITH
  cte1 AS (SELECT a, b FROM table1),
  cte2 AS (SELECT c, d FROM table2)
SELECT b, d FROM cte1 JOIN cte2
WHERE cte1.a = cte2.c;
~~~~

- In the statement containing the WITH clause, each CTE name can be referenced to access the corresponding CTE result set.
- A CTE name can be referenced in other CTEs, enabling CTEs to be defined based on other CTEs.


However, a statement can contain multiple WITH clauses if they occur at different levels:
~~~~mysql
WITH cte1 AS (SELECT 1)
SELECT * FROM (WITH cte2 AS (SELECT 2) SELECT * FROM cte2 JOIN cte1) AS dt;
~~~~


The following example defines CTEs named cte1 and cte2 in the WITH clause, and refers to them in the top-level `SELECT` that follows the `WITH` clause:

_Notes_:
- When you use multiple CTEs, separate them with commas.
- You can use them with `SELECT`, `INSERT`, `UPDATE`, and `DELETE` clauses, but they are most often used with `SELECT`.

**A WITH clause is permitted in these contexts:**
- At the beginning of SELECT, UPDATE, and DELETE statements.
~~~~mysql
WITH ... SELECT ...
WITH ... UPDATE ...
WITH ... DELETE ...
~~~~
- At the beginning of subqueries (including derived table subqueries):
~~~~mysql
SELECT ... WHERE id IN (WITH ... SELECT ...) ...
SELECT * FROM (WITH ... SELECT ...) AS dt ...
~~~~


# Subqueries (nested queries)
A **subquery** is a `SELECT` statement within another statement; it is used to answer multiple part questions. A subquery cannot include an `ORDER BY` clause.


A subquery may occur in :
- A `SELECT`, `INSERT`, `UPDATE` or `DELETE` clause
- A `FROM` clause
-	A `WHERE` clause

A subquery is usually added within the `WHERE` Clause of another `SELECT` statement.<br>
You can use the comparison operators, such as >, <, or =. The comparison operator can also be a multiple-row operator, such as IN, ANY, or ALL.
> The inner query executes first before its parent query so that the results of an inner query can be passed to the outer query.

Subquery can be used to perform the following tasks:
- Compare an expression to the result of the query.
- Determine if an expression is included in the results of the query.
- Check whether the query selects any rows.

**Syntax**
~~~~mysql
SELECT column_names(s) [AS alias] 
FROM table_name
WHERE search_conditions (SELECT column_names(s) from table_name);
~~~~

### guidelines to consider when using subqueries
- A subquery must be enclosed in parentheses. 
- A subquery must be placed on the right side of the comparison operator. 
- Subqueries cannot manipulate their results internally, therefore `ORDER BY` clause cannot be added into a subquery. 
  - You can use an `ORDER BY` clause in the main `SELECT` statement (outer query) which will be the last clause.
- Use single-row operators with single-row subqueries. 
- If a subquery (inner query) returns a `NULL` value to the outer query, the outer query will not return any rows when using certain comparison operators in a `WHERE` clause.

## walkthrough of creating a subquery <br>
![image](https://user-images.githubusercontent.com/49015081/137025216-cd2ed2de-37a1-4eae-9391-d52d1413d725.png)
We have the following two tables `student` and `marks` with common field `StudentID` 

Now we want to write a query to identify all students who get better marks than that of the student who's `StudentID` is `V002`, but we do not know the marks of `V002`.

To solve the problem, we require 2 queries. 
1.	One query returns the marks (stored in `Total_marks` field) of `V002` and 
2.	A second query identifies the students who get better marks than the result of the first query.

![image](https://user-images.githubusercontent.com/49015081/137025316-7461b957-e48c-4c52-b496-d95505ece784.png)
The result of the first query is 80.

Using the result of this query, here we have written another query to identify the students who get better marks than 80. Here is the query : <br>
![image](https://user-images.githubusercontent.com/49015081/137025351-7d9eb8fd-2083-4b58-823f-868053e5c96e.png)

Above two queries identified students who get the better number than the student who's `StudentID` is `V002` (Abhay).

You can combine the above two queries by placing one query inside the other. The subquery (also called the _inner query_) is the query inside the parentheses. See the following code and query result : <br>
![image](https://user-images.githubusercontent.com/49015081/137025428-acb8bdaa-1e41-4cfb-9651-d49605ef3401.png)

_Pictoral representation of Subquery:_  <br>
![image](https://user-images.githubusercontent.com/49015081/137025465-a2a81f32-235b-4633-96e1-de5b7a3705d5.png)

## When to use subqueries (vs JOINS)
Most subqueries can be restated as `JOINS`, and vice versa. When you use a subquery in a `WHERE` clause, its results cannot ve included in the final set because it is not included in the `FROM` clause.

**JOIN** <br>
- THE `SELECT` clause of a join can include columns from both tables.
- A `JOIN` is generally more intuitive when it uses an **existin** relationship between the two tables, such as a primary key to foreign key relationship.

~~~~mysql
SELECT invoice_number, invoice_date, invoice_total
FROM invoices JOIN vendors 
   ON invoices.vendor_id = vendors.vendor_id
WHERE vendor_state = 'CA'
ORDER BY invoice_date;
~~~~

**Subquery**
You can use a subquey to pass an aggregate value to the main query. A subquery tends to be more intuitive when it uses an **ad hoc** relationship between the two tables. Long, complex queries can sometimes be easier to code using subqueries.

~~~~mysql
SELECT invoice_number, invoice_datte, invoice_total
FROM invoices 
WHERE vendor_id IN 
   (SELECT vendor_id       (1)         
   FROM vendors
   WHERE vendor_state = 'CA')
ORDER BY invoice_date;
~~~~
- You cannot include a column name from the `vendors` table because it is not in the `FROM clause`, it is in the subquery.

## 4 ways to introduce a subquery in a `SELECT` statement
1. In a `WHERE` clause as a search condition
2. In a `HAVING` clause as a search condition
3. In the `FROM` clause as a table specification.
4. In the `SELECT` clause as a column specification.

Example <br>
~~~~mysql
SELECT invoice_number, invoice_date, invoice_total
FROM invoices
WHERE invoice_total >               (2)
   (SELECT AVG(invoice_total)       (1)
   FROM invoices)
ORDER BY invoice_total;
~~~~
The suqeury calculates the average of all the invoices
The search condition tests each invoice to see if its `invoice_total` is greater than that average.

## SQL in WHERE clause 

### SQL EXISTS Operator
The EXISTS operator is used to test for the existence of any record in a subquery. Typically used with a correlated subquery.

**Syntax**
~~~~mysql
SELECT column_name(s)
FROM table_name1
WHERE EXISTS
(SELECT column_name FROM table_name2 WHERE condition);
~~~~

**Example 1** <br> 
Consider the schemas:
- Company (cid, cname, city)
- Product (pid, pname, price, cid)

**Problem:** find all companies that make some products with price < 200. <br>
![image](https://user-images.githubusercontent.com/49015081/137119186-12ad948a-6025-4199-abf7-08dea0b84017.png)
![image](https://user-images.githubusercontent.com/49015081/137119203-72ce0648-4ac4-4918-be51-d55b9ccf4517.png)
<br>Let’s use join rather than nested subqueries: <br>
![image](https://user-images.githubusercontent.com/49015081/137119219-30bb071e-b2df-4af0-85fb-b03aae865165.png)

**Example 2** <br>
~~~~mysql
SELECT vendor_id, vendor_name, vendor_state
FROM vendors
WHERE NOT EXISTS                            (2)
   (SELECT *                                (1)
    FROM invoices
    WHERE vendor_id = vendors.vendor_id)
~~~~
1. Selects all invoices that have the same vendor_id as the current vendor in the outer query.
2. Uses `NOT EXISTS` to test whether any invoices were found for the current vendor. If not, then the vendor row is included in the result set.
### SQL IN operator 
The `IN` operator allows you to specify multiple values in a `WHERE` clause
- It tests whetehr an expression is contained in a list of values. You can provide that list of values in a subquery. 
- The IN operator is a shorthand for multiple OR conditions.
- When you use the `IN` operator with a subquery, the subquery must return a single column that provides the list of values. 
- Statements that use the `IN` operator with a subquery can usually be restated as an `OUTER JOIN`.

**Syntax**
~~~~mysql
SELECT column_name (s)
FROM table_name
WHERE column_name IN (value1, value2,…)/ (SELECT STATEMENT);
~~~~

> **Compare IN with EXISTS**
> -	`EXISTS` is much faster than `IN`, when the sub query results is very large.
> -	`IN` is faster than `EXISTS`, when the subquery results is very small.
> -	There is no functional differences between the two 


Example <br>
~~~~mysql
SELECT vendor_id, vendor_name, vendor_state
FROM vendors
WHERE vendor_id NOT IN              (2)
   (SELECT DISTINCT vendor_id       (1)
   FROM invoices)
ORDER BY vendor_id;
~~~~

1. Returns a list of each vendor that is in the invoices table.
2. Returns data about the vendors whose IDs are not in that list.

### Comparison operators and subqueries

When you use a comparison operator to return a single value, you need to use an aggregate function.
~~~~mysql
SELECT invoice_number, invoice_date,
   invoice_total - payment_total - credit_total AS balance_due
FROM invoices
WHERE invoice_total - payment_total - credit_total > 0
  AND invoice_total - payment_total - credit_total < 
      (
         SELECT AVG(invoice_total - payment_total - credit_total)
         FROM invoices
         WHERE invoice_total - payment_total - credit_total > 0
      )
ORDER BY invoice_total DESC;
~~~~

### ALL keyword
Returns a **boolean**. Used to modify the comparison operator so that the condition must be true for all the values retuned by a subquery. If no rows are returned by the subqeuery, a comparison that uses the `ALL` keyword is always true <br>
![image](https://user-images.githubusercontent.com/49015081/137118449-cb213927-64e3-459d-80f9-060447e56468.png)

**Example**: Return invoices larger than the largest invoice or vendor 34 <br>
~~~~mysql
SELECT vendor_name, invoice_number, invoice_total
FROM invoices i JOIN vendors v ON i.vendor_id = v. vendor_id
WHERE invoice_total > ALL       (2)
   (SELECT invoice_total        (1)
    FROM invoices
    WHERE vendor_id = 34)
ORDER BY vendor_name;
~~~~
1. Returns the invoice_total column from vendor 34
2. Where the invoice_total is greater than any value returned in the subquery.

### ANY and SOME keywords
Used to test whether a comparison is true for any of the values returned by a subquery. These keywords are interchangeable.

**Example** <br>
~~~~mysql
SELECT vendor_name, invoice_number, invoice_total
FROM invoices i JOIN vendors v ON i.vendor_id = v. vendor_id
WHERE invoice_total < ANY       (2)
   (SELECT invoice_total        (1)
    FROM invoices
    WHERE vendor_id = 115)
~~~~
1. Returns the invoice_total column from vendor 115
2. Where the invoice_total value is less than any of the columns returned in the ANY subquery.

This subquery could and should be rewritten using the `MAX` function:
~~~~mysql
WHERE invoice_total <        
   (SELECT MAX(invoice_total)
    FROM invoices
    WHERE vendor_id = 115)
~~~~
## Subqueries in FROM clause 

Sometimes we need to compute an intermediate / temporary table only to use it later in a `SELECT-FROM-WHERE`.
-	Option 1: use a subquery in the FROM clause
-	Option 2: use the WITH clause

**Example** : Returns the largest invoice total for the top vendor in each state. <br>
![image](https://user-images.githubusercontent.com/49015081/137148328-3ca19b10-2d72-416c-841a-e65f5cf7bf23.png) <br>
Note that this can also be solved without a subquery. 


Code a subquery in place of a table specification. The result is sometimes referred to as an inline view.
When you code a subquery in the `FROM` clause, you must assign an alias.
In the subquery, you should use an alias for any columns in the subquery that perform calculations. The main query can refer to the columns by these names.

**Example** : Returns the largest invoice total for the top vendor in each state. <br>
~~~~mysql
SELECT vendor_state, MAX(sum_of_invoices) AS max_sum_of_invoices    (2)
FROM 
(
   SELECT vendor_state, vendor_name,            (1)
      SUM(invoice_total) AS sum_of_invoices     (2)
   FROM vendors v JOIN invoices i
         ON v.vendor_id = i.vendor_id
   GROUP BY vendor_state, vendor_name
) t                                             (3)
GROUP BY vendor_state
ORDER BY vendor_state;
~~~~
1. The subquery returns the sum of the invoice_total for every vendor that is in the invoices table
2. Alias in the subquery is used in the main query.
3. Alias for the subquery

## Subqueries in the HAVING clause
Specify a search condition just like the WHERE clause.

## Subqueries in the SELECT clause
Replace a column specification with a subquery. The result of a query must return a single value for that column. In most cases, you use a correlated subquery in the `SELECT` clause.

You can usually restate each as a `JOIN`, so subqueries are not usually used in `SELECT` clause.
**Example** <br>
~~~~mysql
SELECT vendor_name,
   (SELECT MAX(invoice_date) FROM invoices              (1)
    WHERE vendor_id = vendors.vendor_id) AS latest_inv
FROM vendors
ORDER BY latest_inv DESC;
~~~~
1. Calculates the maximum invoice date for each vendor in the vendors table by referring to the vendor_id from the vendors table in the FROM clause of the main query.
Goes through each vendor_id in the invoices table and calculates the max date for each vendor_id in the vendors table. <br>

Restated as a JOIN
~~~~mysql
SELECT vendor_name, MAX(invoice_date) AS latest_inv
FROM vendors v
   LEFT JOIN invoices i ON v.vendor_id = i.vendor_id
GROUP BY vendor_name
ORDER BY latest_inv DESC;
~~~~
