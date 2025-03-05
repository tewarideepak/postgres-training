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


## CHECK constraint

CREATE TABLE contacts(
    name VARCHAR(50),
    mob VARCHAR(15) UNIQUE CHECK (LENGTH(mob) >= 10)
);

ALTER TABLE person
ADD COLUMN
    mob VARCHAR(15)
        CHECK (LENGTH(mob) >= 10);


ALTER TABLE contacts
DROP CONSTRAINT mob_no_less_than_10digits;

ALTER TABLE contacts
ADD CONSTRAINT mob_not_null CHECK (mob != null);

# NAMED Constraint

CREATE TABLE contacts(
    name VARCHAR(50),
    mob VARCHAR(15) UNIQUE,
    CONSTRAINT mob_no_less_than_10digits CHECK (LENGTH(mob) >= 10)
);


## Expression CASE

SELECT fname, salary,
CASE
    WHEN salary >= 50000 THEN 'High'
    ELSE 'Low'
END AS sal_cat
FROM employees;


SELECT fname, salary,
CASE
WHEN salary > 0 THEN ROUND(salary*0.1)
END AS bonus
FROM employees;


SELECT
CASE
    WHEN salary > 55000 THEN 'High'
    WHEN salary BETWEEN 48000 AND 55000 THEN 'Mid'
    ELSE 'Low'
END AS sal_cat,
COUNT(emp_id)
FROM employees
GROUP BY sal_cat;


## RELATIONSHIP

# one to one

Each record in Table A is related to only one record in Table B, and vice versa.

Example:

A Users table and a UserProfiles table where each user has only one profile.

CREATE TABLE Users (
    user_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE UserProfiles (
    profile_id INT PRIMARY KEY,
    user_id INT UNIQUE,  -- Ensures one-to-one relationship
    bio TEXT,
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

# one to many

Each record in Table A can be related to multiple records in Table B, but each record in Table B is related to only one record in Table A.
Example:

A Customers table and an Orders table where each customer can place multiple orders.

CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id) ON DELETE CASCADE
);

# many to many

A Students table and a Courses table where each student can enroll in multiple courses, and each course can have multiple students.

CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100) NOT NULL
);

-- Junction Table (Enrollment)
CREATE TABLE Enrollments (
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Students(student_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

## Foreign Key

A foreign key is a column (or a set of columns) in a database table that establishes a relationship between two tables. It refers to the primary key in another table, ensuring data integrity by enforcing referential constraints.

Key Points:

- Ensures Referential Integrity: Prevents actions that would break the relationship between tables (e.g., deleting a referenced record).
- Establishes Relationships: Links tables together in one-to-many or many-to-many relationships.
- Prevents Invalid Data: A foreign key can only contain values that exist in the referenced primary key column.

# Customers:

CREATE TABLE customers(
    cust_id SERIAL PRIMARY KEY,
    cust_name VARCHAR(100) NOT NULL
);

INSERT INTO customers(cust_name)
VALUES
('Raju'),('Shyam'),('Paul'),('Alex');

# Orders:

CREATE TABLE orders(
    ord_id SERIAL PRIMARY KEY,
    ord_date DATE NOT NULL,
    price NUMERIC NOT NULL,
    cust_id INTEGER NOT NULL,
    FOREIGN KEY (cust_id) REFERENCES
    customers(cust_id)
);

INSERT INTO orders (ord_date, cust_id, price)
VALUES
('2024-01-01', 1, 250.00),
('2024-01-15', 1, 300.00),
('2024-02-01', 2, 150.00),
('2024-03-01', 3, 450.00),
('2024-04-04', 2, 550.00);


## JOINS

JOIN operation is used to combine rows from two or more tables based on a related column between them.

# Types of Join

# Cross Join

Every row from one table is combined with every row from another table.

SELECT * FROM customers CROSS JOIN orders;


# Inner Join

Returns only the rows where there is a match between the specified columns in both the left (or first) and right (or second) tables.

SELECT * FROM Customers AS c
INNER JOIN
orders AS o
ON c.cust_id = o.cust_id;

SELECT c.cust_name, COUNT(o.ord_id)
FROM customers as c
INNER JOIN orders as o
ON o.cust_id = c.cust_id
GROUP BY c.cust_name;


# Left Join

Returns all rows from the left (or first) table and the matching rows from the right (or second) table.

SELECT * FROM Customers AS c
LEFT JOIN
orders AS o
ON c.cust_id = o.cust_id;


# Right Join

Returns all rows from the right (or second) table and the matching rows from the left (or first) table.

SELECT * FROM Customers AS c
RIGHT JOIN
orders AS o
ON c.cust_id = o.cust_id;


## MANY - MANY Relationship

CREATE DATABASE institute;

# Students:

CREATE TABLE students(
    s_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

INSERT INTO students(name) VALUES
('Raju'),
('Shyam'),
('Alex');


# Courses:

CREATE TABLE courses(
    c_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    fee NUMERIC NOT NULL
);

INSERT INTO courses(name, fee)
VALUES
('Maths', 500.00),
('Physics', 600.00),
('Chemistry', 700.00);


# Enrollment:

CREATE TABLE enrollment(
    enrollment_id SERIAL PRIMARY KEY,
    s_id INT NOT NULL,
    c_id INT NOT NULL,
    enrollment_date DATE NOT NULL,
    FOREIGN KEY (s_id) REFERENCES students(s_id),
    FOREIGN KEY (c_id) REFERENCES courses(c_id)
);

INSERT INTO enrollment(s_id, c_id, enrollment_date)
VALUES
(1, 1, '2024-01-01'), -- Raju enrolled in Mathematics
(1, 2, '2024-01-15'), -- Raju enrolled in Physics
(2, 1, '2024-02-01'), -- Shyam enrolled in Mathematics
(2, 3, '2024-02-15'), -- Shyam enrolled in Chemistry
(3, 3, '2024-03-25'); -- Alex enrolled in Chemistry


SELECT s.name, c.name, e.enrollment_date, c.fee
FROM enrollment as e
JOIN students as s
ON e.s_id = s.s_id
JOIN courses as c
ON c.c_id = e.c_id;


# Project of E-Store

Create a one-to-many and many-to-many relationship in a shopping store context using four tables:

- customers
- orders
- products
- order_items

Include a price column in the products table and display the relationship between customers and their orders along with the details of the products in each order.


# Task - StoreDB

## customers

CREATE TABLE customers(
    cust_id SERIAL PRIMARY KEY,
    cust_name VARCHAR(100) NOT NULL
);


INSERT INTO customers (cust_name)
VALUES
    ('Raju'), ('Sham'), ('Paul'), ('Alex');


## orders

CREATE TABLE orders(
    ord_id SERIAL PRIMARY KEY,
    ord_date DATE NOT NULL,
    cust_id INTEGER NOT NULL,
    FOREIGN KEY (cust_id) REFERENCES customers (cust_id)
);


INSERT INTO orders (ord_date, cust_id)
VALUES
    ('2024-01-01', 1),  -- Raju first order
    ('2024-02-01', 2),  -- Sham first order
    ('2024-03-01', 3),  -- Paul first order
    ('2024-04-04', 2);  -- Sham second order


## products

CREATE TABLE products (
    p_id SERIAL PRIMARY KEY,
    p_name VARCHAR(100) NOT NULL,
    price NUMERIC NOT NULL
);


INSERT INTO products (p_name, price)
VALUES
    ('Laptop', 55000.00),
    ('Mouse', 500),
    ('Keyboard', 800.00),
    ('Cable', 250.00)
;


## order_items

CREATE TABLE order_items (
    item_id SERIAL PRIMARY KEY,
    ord_id INTEGER NOT NULL,
    p_id INTEGER NOT NULL,
    quantity INTEGER NOT NULL,
    FOREIGN KEY (ord_id) REFERENCES orders(ord_id),
    FOREIGN KEY (p_id) REFERENCES products(p_id)
);


INSERT INTO order_items (ord_id, p_id, quantity)
VALUES
    (1, 1, 1),  -- Raju ordered 1 Laptop
    (1, 4, 2),  -- Raju ordered 2 Cables
    (2, 1, 1),  -- Sham ordered 1 Laptop
    (3, 2, 1),  -- Paul ordered 1 Mouse
    (3, 4, 5),  -- Paul ordered 5 Cables
    (4, 3, 1);  -- Sham ordered 1 Keyboard


SELECT 
	c.cust_name,
	o.ord_date,
	p.p_name,
	p.price,
	oi.quantity,
	(oi.quantity*p.price) AS total_price
FROM order_items oi
	JOIN
		products p ON oi.p_id=p.p_id
	JOIN
		orders o ON o.ord_id=oi.ord_id
	JOIN
		customers c ON o.cust_id=c.cust_id;


# Listing tables

\dt


# Listing specific table

\d <table_name>


# Views

It is a temporary table which we can access anytime with a single click.


CREATE VIEW billing_info AS
SELECT 
	c.cust_name,
	o.ord_date,
	p.p_name,
	p.price,
	oi.quantity,
	(oi.quantity*p.price) AS total_price
FROM order_items oi
	JOIN
		products p ON oi.p_id=p.p_id
	JOIN
		orders o ON o.ord_id=oi.ord_id
	JOIN
		customers c ON o.cust_id=c.cust_id;


SELECT * FROM billing_info;


# List the Views

\dv


# Removing a View

DROP VIEW billing_info;


# HAVING Clause

SELECT p_name, SUM(total_price)
FROM billing_info
GROUP BY p_name
HAVING SUM(total_price) > 1500;


# GROUP BY ROLLUP

SELECT
    COALESCE(p_name, 'Total'),  -- If p_name is null
    SUM(total_price) AS amount
FROM billing_info
    GROUP BY
    ROLLUP(p_name) ORDER BY amount;



# STORED Routine

An SQL statement or a set of SQL statement that can be stored on database server which can be call no. of times.


# Types of STORED Routine

- STORED Procedure
- User defined functions


# STORED Procedure

Set of SQL statements and procedural logic that can perform operations such as inserting, updating, deleting, and querying data.


- Using bank_db

SELECT * FROM employees;


CREATE OR REPLACE PROCEDURE procedure_name (parameter_name parameter_type, ...)
LANGUAGE plpgsql    -- Procedural Language for psql
AS $$       -- For changing delimiter
BEGIN
    -- procedural code here
END;
$$;


CREATE OR REPLACE PROCEDURE update_emp_salary(
    p_employee_id INT,
    p_new_salary NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE employees
    SET salary = p_new_salary
    WHERE emp_id = p_employee_id;
END;
$$;


CALL update_emp_salary(3, 71000);


    CREATE OR REPLACE PROCEDURE add_employee(
        p_fname VARCHAR,
        p_lname VARCHAR,
        p_email VARCHAR,
        p_dept VARCHAR,
        p_salary NUMERIC
    )
    LANGUAGE plpgsqlAS $$
    BEGIN
        INSERT INTO employees(fname, lname, email, dept, salary)
        VALUES(p_fname, p_lname, p_email, p_dept, p_salary);
    END;
    $$;


# User Defined Functions

Custom function created by the user to perform specific operations and return a value.

    CREATE OR REPLACE FUNCTION function_name(parameters)
    RETURNS return_type AS $$
    BEGIN
        -- Function body (SQL statements)
        RETURN some_value;  -- For scalar functions (returns a single value)
    END;
    $$ LANGUAGE plpgsql;


## Find name of the employees in each department having maximum salary.

- Using Subquery:

SELECT e.emp_id, e.fname, e.salary
FROM employees e
WHERE e.dept = 'HR'
AND e.salary = (
    SELECT MAX(emp.salary) FROM employees
    WHERE emp.dept = 'HR'
);


- Using User defined function

CREATE OR REPLACE FUNCTION dept_max_sal_emp1(dept_name VARCHAR)
RETURNS TABLE(emp_id INT, fname VARCHAR, salary NUMERIC)
AS $$
BEGIN
    RETURN QUERY
    SELECT
        e.emp_id, e.fname, e.salary
    FROM
        employees e
    WHERE
        e.dept = dept_name
    AND e.salary = (
        SELECT MAX(emp.salary)
        FROM employees emp
        WHERE emp.dept = dept_name
    );
END
$$ LANGUAGE plpgsql;


SELECT * FROM dept_max_sal_emp1('HR');