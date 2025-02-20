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



