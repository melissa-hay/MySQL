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

## Cross Join

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

# `GROUP BY` clause
_**What**_<br>
 The `GROUP BY` statement groups rows that have the same values into summary rows, like "find the number of customers in each country".
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

# Common table expressions (CTE)
A CTE is a `SELECT` statement that creates one or more named temporary result sets i.e. **aliases** that can be used by the query that follows. Use CTEs to simplify complex queries that use subqueries.
To specify common table expressions, use a WITH clause that has one or more comma-separated subclauses. Each subclause provides a subquery that produces a result set, and associates a name with the subquery. 

CTEs begin with a `WITH` clause that has one or more comma-separated subclauses. Each subclause provides a subquery that produces a result set, and associates a name (alias) with the subquery. 

**Syntax with 1 alias**
~~~~mysql
WITH alias AS (SELECT column_name(s) FROM table_name)
SELECT column_names(s) FROM alias [,table_name]
[WHERE <join_condition>]
~~~~

~~~~mysql
WITH
  alias1 AS (SELECT column_name(s) FROM table1),
  alias2 AS (SELECT column_name(s) FROM table2)
SELECT b, d FROM alias1 JOIN alias2
WHERE <join_condition>;
~~~~

The following example defines CTEs named cte1 and cte2 in the WITH clause, and refers to them in the top-level `SELECT` that follows the `WITH` clause:

_Notes_:
- When you use multiple CTEs, separate them with commas.
- You can use them with `SELECT`, `INSERT`, `UPDATE`, and `DELETE` clauses, but they are most often used with `SELECT`.

# Subqueries (nested queries)
A **subquery** is a query within a query and it is used to answer multiple part questions.

A subquery may occur in :
- A `SELECT`, `INSERT`, `UPDATE` or `DELETE` clause
- A `FROM` clause
-	A `WHERE` clause

A subquery is usually added within the `WHERE` Clause of another `SELECT` statement.<br>
You can use the comparison operators, such as >, <, or =. The comparison operator can also be a multiple-row operator, such as IN, ANY, or ALL.
> The inner query executes first before its parent query so that the results of an inner query can be passed to the outer query.

**Syntax**
~~~~mysql
SELECT column_names(s) [AS alias] 
FROM table_name
WHERE conditions (SELECT column_names(s) from table_name);
~~~~


### guidelines to consider when using subqueries
- A subquery must be enclosed in parentheses. 
- A subquery must be placed on the right side of the comparison operator. 
- Subqueries cannot manipulate their results internally, therefore `ORDER BY` clause cannot be added into a subquery. 
  - You can use an `ORDER BY` clause in the main `SELECT` statement (outer query) which will be the last clause.
- Use single-row operators with single-row subqueries. 
- If a subquery (inner query) returns a `NULL` value to the outer query, the outer query will not return any rows when using certain comparison operators in a `WHERE` clause.










