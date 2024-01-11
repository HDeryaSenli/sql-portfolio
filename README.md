# sql-notes
My humble SQL notes on certain subjects.

# Database Validation - Row-Level
In this repo I'll explain about different data validations in postgreSQL as far as I know.
First of all let's simply explain what is database validation and what is the point?

## What is data validation in SQL and why is it important?  
> Data validation is a method for checking the accuracy and quality of data.  Information in databases is constantly being updated, deleted, queried, or moved by multiple people or processes, so ensuring that data is valid at all times is essential. Some of you may be saying to yourself “data validation like this should be performed in the application layer, not the database layer”.  Of course, validation in the application layer is crucial.  But there are instances where updates could be performed directly in the database.  It is also good practice to make sure that the database has some form of data validation even it also exists elsewhere.

> Source: https://sqlspreads.com/blog/simple-data-validation-in-sql/


First constraint is **NOT NULL**


## NOT NULL Constraint
Simply, we are going to say to the database if an entry to the database is empty (NULL), don't let it happen.
There are 2 ways:
1. You can add a constrain at the start of the creation of the database.
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

```sql
CREATE TABLE products 
(
	id SERIAL PRIMARY KEY,
	name VARCHAR(40),
	department VARCHAR(40),
	price INTEGER,
	weight INTEGER
);
```
After creating the db without any constraints we can use **ALTER** statement to add the constraint we want.

```pgsql
ALTER TABLE products
ALTER COLUMN price
SET NOT NULL;
```
>Something important here: Before adding the constraint with **ALTER** we should not have any data that is NULL in our database. Otherwise you'll get an error from pgSQL.

For using **ALTER** values one must deal with all NULL's before proceed.
There are different ways to do this based on your database. Google it and you'll see bunch of methods.


## UNIQUE Constraint
Simply, we are going to say to the database if an entry to the database is not unique, don't let it happen
Like **NOT NULL** the two way approach still works.
1. You add UNIQUE during the creation of the db.
2. Or after creating the db.

I'm honestly going to cut it short.

For 1:
```pgsql
CREATE TABLE products
(
id SERIAL PRIMARY KEY,
name VARCHAR(50) UNIQUE,
department VARCHAR(50),
price INTEGER,
weight INTEGER,
);
```
The **UNIQUE** statement do the trick.

For 2:
```pgsql
ALTER TABLE products
ADD UNIQUE (name);
```

> Just like NOT NULL, deal with non-unique values in your db first.

## Deleting Constraints
For deleting constraints you need the name of the constraints.
For that go here:

![image](https://github.com/HDeryaSenli/sql-portfolio/assets/112333951/57cd02ef-48ac-4495-a7e7-41ac950f584d)

And the query goes like:

```pgsql
ALTER TABLE products2
DROP CONSTRAINT products2_name_key;

```

## Validation Check
The **CHECK** constraint is used to limit the value range that can be placed in a column.
Like **NOT NULL** the two way approach still works.
1. You add **CHECK** during the creation of the db.
2. Or after creating the db.

1. Add **CHECK** during the creation
```pgsql
CREATE TABLE products 
(
	id SERIAL PRIMARY KEY,
	name VARCHAR(50),
	department VARCHAR(50) NOT NULL,
	price INTEGER CHECK (price > 0),
	weight INTEGER
);
```

2. Or after creating the db.

```pgsql
ALTER TABLE products
ADD CHECK (price > 0);
```

> Just like NOT NULL and UNIQUE, deal with the values that are doesn't fit the check constraint in your db first.

Look at the main heading of this repo. It says row-level.
A basic summary of this can be found at image below:

![image](https://github.com/HDeryaSenli/sql-portfolio/assets/112333951/a64f13ba-df14-4588-8570-7a0c8726a6f0)
> Source is an Udemy Course for pgSQL called *SQL and PostgreSQL: The Complete Developer's Guide by Stephen Grider*



