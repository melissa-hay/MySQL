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
One or more columns in a table that refer to a primary key in another table (one-to-many relationship)


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

