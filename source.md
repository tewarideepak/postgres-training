## What is a Database?

An organized collection of data.
A method to manipulate and access the data.


## Database vs DBMS

Think of a database as a bookshelf full of books (data).
A DBMS is like a librarian who helps you organize, find, update, or remove books easily.

So, while a database is just a collection of stored information, a DBMS makes it efficient, secure, and accessible.


## What is RDBMS?

A type of database system that stores data in structured tables (using rows and columns) and uses SQL for managing and querying data.


## SQL

Structured Query Language
Which is used to talk to our databases.

Example: SELECT * FROM person_db;


## Switching Over to the postgres Account

Switch over to the postgres account on your server by typing:

    sudo -i -u postgres

You can now access the PostgreSQL prompt immediately by typing:

    psql

From there you are free to interact with the database management system as necessary.

Exit out of the PostgreSQL prompt by typing:

    \q


List down the database:

    \l


Change password:

    \password postgres


## Creating a new role

Currently, you just have the postgres role configured within the database. You can create new roles from the command line with the createrole command. The --interactive flag will prompt you for the name of the new role and also ask whether it should have superuser permissions.

If you are logged in as the postgres account, you can create a new user by typing:

    createuser --interactive

If, instead, you prefer to use sudo for each command without switching from your normal account, type:

    sudo -u postgres createuser --interactive

The script will prompt you with some choices and, based on your responses, execute the correct Postgres commands to create a user to your specifications.

You can get more control by passing some additional flags. Check out the options by looking at the man page:

    man createuser



# Creating a new database

CREATE DATABASE <database_name>;

Another assumption that the Postgres authentication system makes by default is that for any role used to log in, that role will have a database with the same name which it can access.

This means that if the user you created in the last section is called sammy, that role will attempt to connect to a database which is also called “sammy” by default. You can create the appropriate database with the createdb command.

If you are logged in as the postgres account, you would type something like:

    createdb sammy

If, instead, you prefer to use sudo for each command without switching from your normal account, you would type:

    sudo -u postgres createdb sammy

This flexibility provides multiple paths for creating databases as needed.


## Database vs Schema vs Table

Imagine a library system:
📚 Database → The entire library (all books, sections, and records).
📂 Schema → A specific section (e.g., Fiction or Science) that organizes related books.
📄 Table → A shelf in that section containing books on a particular topic.

So, a Database holds everything, a Schema organizes data logically, and a Table stores actual records in rows and columns.


## List down existing database

SELECT datname FROM pg_database;

\l


## Change a Database

\c <database_name>;


## Deleting a Database

DROP DATABASE <db_name>;


## CRUD

## Creating Tables

# Table

A table is a collection of related data held in a table format within a database.


CREATE TABLE person (
    id INT,
    name VARCHAR(100),
    city VARCHAR(100)
);


# List the table schema

\d person;


## Adding data into a table

INSERT INTO person(id, name, city)
VALUES (101, 'Raju', 'Delhi');

INSERT INTO students VALUES (101, 'RAHUL');

# Adding multiple data

INSERT INTO person(id, name, city)
VALUES
(101, 'Raju', 'Delhi'),
(102, 'Shyam', 'Mumbai');


## Reading data from a table

SELECT * FROM <table_name>;

SELECT <column_name> FROM students;
SELECT <column_name1, column_name2> FROM students;


## Updating the data

UPDATE person
    SET city='London'
    WHERE id=2;


## Deleting data from table

DELETE FROM students
    WHERE name='Raju';


## DataTypes

An attribute that specifies the type of data in a column of our database - table.

# Most widely used are

- Numeric - INT DOUBLE FLOAT DECIMAL
- String - VARCHAR
- Date - DATE
- Boolean - BOOLEAN


DECIMAL(5, 2)

5 - Total digit
2 - Digits after decimal

Example: 155.38, 28.15


## Constraint

A Constraint in PostgreSQL is a rule applied to a column.


# Primary Key

- The PRIMARY KEY constraint uniquely identifies each record in a table.

- Primary keys must contain UNIQUE values, and cannot contain NULL values.

- A table can have only ONE primary key.


# NOT NULL

CREATE TABLE customers
(
    id INT NOT NULL,
    name VARCHAR(100) NOT NULL
);


# DEFAULT Value

CREATE TABLE customers
(
    acc_no INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    acc_type VARCHAR(50) NOT NULL DEFAULT 'Savings'
);


# AUTO_INCREMENT

CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    firstname VARCHAR(50),
    lastname VARCHAR(50)
);


## Task: Creating New Table

CREATE TABLE employees (
    emp_id SERIAL PRIMARY KEY,
    fname VARCHAR(50) NOT NULL,
    lname VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    dept VARCHAR(50),
    salary DECIMAL(10, 2) DEFAULT 30000.00,
    hire_date DATE NOT NULL DEFAULT CURRENT_DATE
);


SELECT setval('employees_emp_id_seq', 1);

SELECT currval('employees_emp_id_seq');


## Data Refining

## Clauses

# WHERE

SELECT * FROM employees
WHERE emp_id = 5;

SELECT * FROM employees
WHERE dept = 'IT' AND salary > 50000;

SELECT * FROM employees
WHERE dept = 'IT' OR salary > 50000;


# Relational Operators

<
>
<=
>=
=
!=


# Logical Operators

AND
OR


# IN Operator

SELECT * FROM employees
WHERE dept IN ('IT', 'Finance', 'HR');

# NOT IN Operator

SELECT * FROM employees
WHERE dept NOT IN ('IT', 'Finance', 'HR');

# BETWEEN Operator (Both are inclusive)

SELECT * FROM employees
WHERE salary BETWEEN 50000 AND 60000;


# DISTINCT

SELECT DISINCT dept FROM employees;


# ORDER BY (Ascending)

SELECT * FROM employees ORDER BY fname;

SELECT * FROM employees ORDER BY fname DESC;


# LIMIT

SELECT * FROM employees LIMIT 3;


# LIKE (case-sensitive)

SELECT * FROM employees
WHERE fname LIKE 'A%';

SELECT * FROM employees
WHERE fname LIKE '%a';


- Starts with 'A': LIKE 'A%'
- Ends with 'A': LIKE '%A'
- Contains 'A': LIKE '%A%'
- Second character is 'A': LIKE '_A%'
- Case-insensitive contains 'john': ILIKE '%john%'


SELECT * FROM employees
WHERE dept LIKE '__';


## Aggregate functions

# COUNT

SELECT COUNT(emp_id) FROM employees;


# SUM

SELECT SUM(salary) FROM employees;


# AVG

SELECT AVG(salary) FROM employees;


# MIN

SELECT MIN(salary) FROM employees;


# MAX

SELECT MAX(salary) FROM employees;


## GROUP BY

- Number of employees in each department:

SELECT dept FROM employees
GROUP BY dept;

SELECT dept, COUNT(fname) FROM employees
GROUP BY dept;


## String Functions

# CONCAT

CONCAT(first_col, sec_col)
CONCAT(first_word, sec_word, ...)

SELECT CONCAT('Hello', 'World');

SELECT CONCAT(fname, lname) AS Fullname
FROM employees;


# CONCAT_WS

CONCAT_WS('-', fname, lname, ...)


# SUBSTRING / SUBSTR (index starts from 1)

SELECT SUBSTRING('Hey Buddy', 1, 4);


# REPLACE

REPLACE(str, from_str, to_str)
REPLACE('Hey Buddy', 'Hey', 'Hello')

SELECT REPLACE(dept, 'IT', 'Tech') FROM employees;


# REVERSE

SELECT REVERSE('Hello World')


# LENGTH

SELECT LENGTH('Hello World')


# UPPER and LOWER

SELECT UPPER('Hello')
SELECT LOWER('Hello')


# LEFT and RIGHT

SELECT LEFT('Abcdefghij', 3)

SELECT RIGHT('Abcdefghij', 4)


# TRIM

SELECT TRIM('  Alright!  ')


# POSITION

SELECT POSITION('OM' in 'Thomas')


## EXERCISES

# Task 1

1:Raj:Sharma:IT

SELECT CONCAT_WS(':', emp_id, fname, lname, dept)
FROM employees LIMIT 1;


# Task 2

1:Raju Sharma:IT:50000

SELECT CONCAT_WS(':', emp_id, REPLACE(CONCAT_WS(' ', fname, lname), fname, 'Raju'), dept, salary) FROM employees LIMIT 1;


# Task 3

4:Suman:FINANCE

SELECT CONCAT_WS(':', emp_id, fname, UPPER(dept)) FROM employees WHERE emp_id = 4;


# Task 4

I1 Raju
H2 Priya

SELECT CONCAT(LEFT(dept, 1), emp_id), fname FROM employees;


## Exercises (DISTINCT, ORDER BY, LIKE, LIMIT)

# 1. Find different type of departments in database

SELECT DISTINCT(dept) FROM employees;


# 2. Display records with High-low salary

SELECT * FROM employees ORDER BY salary DESC;


# 3. How to see only top 3 records from a table

SELECT * FROM employees LIMIT 3;


# 4. Show records where first name start with letter 'A'

SELECT * FROM employees WHERE fname LIKE 'A%';


# 5. Show records where length of the lname is 4 characters

SELECT * FROM employees WHERE LENGTH(lname) = 4;


## Exercises (COUNT, GROUP BY, MIN, MAX, SUM, AVG)

# 1. Find total number of employees in database

SELECT COUNT(emp_id) AS Total FROM employees;


# 2. Find number of employees in each department

SELECT dept, COUNT(emp_id) FROM employees GROUP BY dept;


# 3. Find lowest salary paying

SELECT MIN(salary) FROM employees;


# 4. Find highest salary paying (Subquery)

SELECT MAX(salary) FROM employees;

SELECT * FROM employees ORDER BY salary DESC LIMIT 1;

SELECT * FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);

# 5. Find total salary paying in Loan department

SELECT SUM(salary) FROM employees WHERE dept = 'Finance';

# 6. Average salary paying in each department

SELECT dept, AVG(salary) FROM employees
GROUP BY dept;


## ALTER Query

# How to add or remove a column?

ALTER TABLE contacts
ADD COLUMN city VARCHAR(50);

ALTER TABLE contacts
DROP COLUMN city;


# How to rename a column or table name?

ALTER TABLE contacts
RENAME COLUMN name to full_name;

ALTER TABLE contacts
RENAME TO mycontacts;

RENAME TABLE contacts TO mycontacts;


# How to modify a column?

Ex: Changing datatype or adding Default values etc

ALTER TABLE person
ALTER COLUMN fname
SET DATA TYPE VARCHAR(200);

# How to set DEFAULT value?

ALTER TABLE person
ALTER COLUMN fname
SET DEFAULT 'unknown';

ALTER TABLE person
ALTER COLUMN fname
DrOP DEFAULT;

# How to set NOT NULL?

ALTER TABLE person
ALTER COLUMN fname
SET NOT NULL;