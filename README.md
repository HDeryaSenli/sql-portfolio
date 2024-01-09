# sql-portfolio
My humble SQL portfolio

# Database Validation - Constraints
In this repo I'll explain about different data validations in postgreSQL as far as I know.
First of all let's simply explain what is database validation and what is the point?

## What is data validation in SQL and why is it important?  
> Data validation is a method for checking the accuracy and quality of data.  Information in databases is constantly being updated, deleted, queried, or moved by multiple people or processes, so ensuring that data is valid at all times is essential. Some of you may be saying to yourself “data validation like this should be performed in the application layer, not the database layer”.  Of course, validation in the application layer is crucial.  But there are instances where updates could be performed directly in the database.  It is also good practice to make sure that the database has some form of data validation even it also exists elsewhere.

> Source: https://sqlspreads.com/blog/simple-data-validation-in-sql/


First constraint is **NOT NULL**


## NOT NULL Constraint
Simply, we are going to say to the database if an entry to the database is empty (NULL), don't let it happen.
There are 2 ways:
1. You can add a constrain at the start of the creating the database.
2. You can add a constrain after the start 

### Option 1: 
First option is pretty basic.
You just use **NOT NULL** constraint.
Let's see below by creating a database. This gonna be pretty basic and common db.

```sql
CREATE TABLE products2
(
	id SERIAL PRIMARY KEY,
	name VARCHAR(40),
	department VARCHAR(40),
	price INTEGER NOT NULL,
	weight INTEGER
);
```
Now if you try to add some values without price, database will give you an error.

For example:
```sql
INSERT INTO products2 (name, department, weight)
VALUES
	('Shirt','Clothes',1);

--You can see the error that postgre gives below.

ERROR:  Failing row contains (2, Shirt, Clothes, null, 1).null value in column "price" of relation "products2" violates not-null constraint 

ERROR:  null value in column "price" of relation "products2" violates not-null constraint
SQL state: 23502
Detail: Failing row contains (2, Shirt, Clothes, null, 1).

```

### Option 2: 
You might want to add the constraint after creating the db.
For this **ALTER** statement comes in.

Let's create the first db without any constraint.




