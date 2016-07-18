<!--
Created By: Alex White
Market: SF
Adapted By: Zeb Girouard
Market: DEN
-->

![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

<!-- 2:30 5 minutes -->

<!-- Hook: So nowadays, there are two main ways to model large datasets.  Basically, Objects or Tables.  Raise your hand if you prefer the Object/JSONish way.  Raise your hand if you prefer Tables (like we just saw, kind of like Excel and Google Sheets). Well, today we're going to focus on the most prominent Table-organized tool.-->

# SQL

### Why is this important?
*This workshop is important because:*
- Every SQL database system uses SQL or Structured Query Language
- We will be using ActiveRecord extensively, which dynamically writes our SQL requests for us! But it is important to understand what's going on under the hood.


### What are the objectives?
*After this workshop, developers will be able to:*

- **Create** a database table
- **Insert**, **retrieve**, **update**, and **delete** a row or rows of a database table

### Where should we be now?

*Before this lesson, students should already be able to:*

- **Install** **[PostgreSQL](http://www.postgresql.org/)**
- **Describe** the relationship between tables, rows, and columns
- **Draw** an ERD diagram
- **Explain** the difference between table relationships


## We know about Databases, but what is SQL?  

Let's review: at its simplest, a *relational database* is a mechanism to store and retrieve data in a tabular form.  Spreadsheets are a good analogy.  But how do we interact with our database: inserting data, updating data, retrieving data, and deleting data? We can't just type them in by hand every time. That's where SQL comes in!

### What is SQL?

SQL stands for Structured Query Language, and it is a language universally used and adapted to interact with relational databases.  When you use a SQL client and connect to a relational database that contains tables with data, the scope of what you can do with SQL commands includes:

- Inserting data
- Querying or retrieving data
- Updating or deleting data
- Creating new tables and entire databases
- Control permissions of who can have access to our data

Note that all these actions depend on what the database administrator sets for user permissions: a lot of times, as an analyst, for example, you'll only have access to retrieving company data; but as a developer, you could have access to all these commands and be in charge of setting the database permissions for your web or mobile application.

### Why is SQL important?

Well, a database is just a repository to store the data and you need to use systems that dictate how the data will be stored and as a client to interact with the data.  We call these systems "Database Management Systems", they come in _many_ forms:

- MySQL
- SQLite
- PostgreSQL (what we'll be using!)

...and all of these management systems use SQL (or some adaptation of it) as a language to manage data in the system.

<!-- What was the DBMS we used for a noSQL DB? -->

<!-- 2:35 5 minutes -->

## Connect, Create a Database - Code Along
Let's make a database!  First, make sure you have PostgreSQL running.  Once you do, open your terminal and type:

```bash
$ psql
```

You should see something like:

```bash
your_user_name=#
```

If you see
```bash
psql: FATAL: database does not exist
```

try running
```bash
createdb <user>
```
and then `psql` again.

Great! You've entered the PostgreSQL equivalent of PRY: now, you can execute PSQL commands, or PostgreSQL's version of SQL.

We will use these commands soon, but before we can, we must create a database.  Let's call it wdi:

```psql
your_user_name=# CREATE DATABASE wdi;
CREATE DATABASE
```

The semicolon is important! Be sure to always end your SQL queries and commands with semicolons.

Now let's _use_ that database we just created:

```psql
your_user_name=# \c wdi
You are now connected to database "wdi" as user "your_user_name".
wdi=#
```

<!-- Half-mast -->
<!-- 2:40 5 minutes -->

## Insert and Query data - Demo

Now that we have a database, let's create a table (think of this like, "hey now that we have a workbook/worksheet, let's block off these cells with a border and labels to show it's a unique set of values"):

```sql
CREATE TABLE instructors (
  ID  INT PRIMARY KEY NOT NULL,
  NAME TEXT NOT NULL,
  EXPERIENCE INT NOT NULL,
  WEBSITE CHAR(50)
);
```

When we paste this into psql:

```psql
wdi=# CREATE TABLE instructors (
wdi(#  ID          INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME        TEXT                NOT NULL,
wdi(#  EXPERIENCE  INT                 NOT NULL,
wdi(#  WEBSITE     CHAR(50)
wdi(#  );
CREATE TABLE
```

Notice the different parts of these commands:

```psql
wdi=# CREATE TABLE instructors (
```
This starts our table creation, it tells PostgreSQL to create a table named "instructors"..

```psql
wdi(#  ID        INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME      TEXT                NOT NULL,
```

...then, each line after denotes a new column we're going to create for this table, what the column will be called, the data type, whether it's a primary key, and whether the database - when data is added - can allow data without missing values.  In this case, we're not allowing NAME, AGE, or ID to be blank; but we're ok with website being blank.

<!-- 2:45 10 minutes -->

## Create a student table and insert data - Codealong

Now that we've done it to keep track of our instructors, let's create a table for students that collects information about:

- an id that cannot be left blank
- the students name that cannot be left blank
- their age
- and their address that cannot be left blank.

Take a few minutes to try and create this table.

<details>
Here's what that query should have looked like:

```sql
CREATE TABLE students (
  ID INT PRIMARY KEY NOT NULL,
  NAME TEXT NOT NULL,
  AGE INT NOT NULL,
  ADDRESS CHAR(50)
);
```

In psql that will look like:

```psql
wdi=# CREATE TABLE students (
wdi(#  ID          INT   PRIMARY KEY   NOT NULL,
wdi(#  NAME        TEXT                NOT NULL,
wdi(#  AGE         INT                 NOT NULL,
wdi(#  ADDRESS     CHAR(50)
wdi(#  );
CREATE TABLE
```
</details>

Great job! Now let's finally _insert_ some data into that table - remember what cannot be left blank!

We'll insert five students, Jack, Jill, John, Jackie, and Slagathorn. The syntax is as follows:

```psql
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...);
```

Let's do it for Jack, together:

```sql
INSERT INTO students VALUES (1, 'Jack Sparrow', 28, '50 Main St, New York, NY');
```
In psql that will look like:

```psql
wdi=# INSERT INTO students VALUES (1, 'Jack Sparrow', 28, '50 Main St, New York, NY');
INSERT 0 1
```

<!-- 2:55 5 minutes -->

## Insert Data - Independent Practice

Now, try it for the other students, and pay attention to the order of Jack's parameters and the single quotes - they both matter.

- Jill's full name is Jilly Cakes; she's 30 years old, and lives at 123 Webdev Dr. Boston, MA
- Johns's full name is Johnny Bananas; hes 25 years old, and lives at 555 Five St, Fivetowns, NY
- Jackie's full name is Jackie Lackie; she's 101 years old, and lives at 2 OldForThis Ct, Fivetowns, NY
- Slagathorn's full name is Slaggy McRaggy; he's 28 and prefers not to list his address

<details>
You probably came up with something like:

```sql
INSERT INTO students VALUES (2, 'Jilly Cakes', 30, '123 Webdev Dr. Boston, MA');
INSERT INTO students VALUES (3, 'Johnny Bananas', 25, '555 Five St, Fivetowns, NY');
INSERT INTO students VALUES (4, 'Jackie Lackie', 101, '2 OldForThis Ct, Fivetowns, NY');
INSERT INTO students VALUES (5, 'Slaggy McRaggy', 28);
```

In psql this should look like:

```psql
wdi=# INSERT INTO students VALUES (2, 'Jilly Cakes', 30, '123 Webdev Dr. Boston, MA');
INSERT 0 1
wdi=# INSERT INTO students VALUES (3, 'Johnny Bananas', 25, '555 Five St, Fivetowns, NY');
INSERT 0 1
wdi=# INSERT INTO students VALUES (4, 'Jackie Lackie', 101, '2 OldForThis Ct, Fivetowns, NY');
INSERT 0 1
wdi=# INSERT INTO students VALUES (5, 'Slaggy McRaggy', 28);
INSERT 0 1
```
</details>

<!--3:00 15 minutes -->

## What's in our database? Code Along

So now that we have this data saved, we're going to need to access it at some point, right?  We're going to want to _select_ particular datapoints in our dataset provided certain conditions.  The PostgreSQL SELECT statement is used to fetch the data from a database table which returns data in the form of result table. These result tables are called result-sets. The syntax is probably what you would have guessed:

```psql
SELECT column1, column2, columnN FROM table_name;
```
We can pass in what columns we want to look at - like above - or even get all our table records:

```psql
SELECT * FROM table_name;
```

For us, we can get all the records back:

```psql
wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
(5 rows)
```

Or we can get just the name and ages of our students:

```psql
wdi=# SELECT name, age FROM students;
      name      | age
----------------+-----
 Jack Sparrow   |  28
 Jilly Cakes    |  30
 Johnny Bananas |  25
 Jackie Lackie  | 101
 Slaggy McRaggy |  28
(5 rows)
```

<!-- Half-mast -->

#### Getting more specific

Just like Ruby or JavaScript, all of our comparison and boolean operators can do work for us to select more specific data.

- I want the names of all the students who are spring chickens - done:

```psql
wdi=# SELECT name FROM students WHERE age < 30;
      name
----------------
 Jack Sparrow
 Johnny Bananas
 Slaggy McRaggy
(3 rows)
```

- How about the names of students orderedby age? Done:

```psql
wdi=# SELECT name, age FROM students ORDER BY age;
      name      | age
----------------+-----
 Johnny Bananas |  25
 Jack Sparrow   |  28
 Slaggy McRaggy |  28
 Jilly Cakes    |  30
 Jackie Lackie  | 101
(5 rows)
```

- How about reversed? Ok:

```psql
wdi=# SELECT name, age FROM students ORDER BY age DESC;
      name      | age
----------------+-----
 Jackie Lackie  | 101
 Jilly Cakes    |  30
 Jack Sparrow   |  28
 Slaggy McRaggy |  28
 Johnny Bananas |  25
(5 rows)
```

- How about those who live in Fivetowns? We can find strings within strings too!

```psql
wdi=# SELECT * FROM students WHERE address LIKE '%Fivetowns%';
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
(2 rows)
```

Now try to do the following on your own:
- SELECT only the student names and their age, ORDER them by age
- SELECT only the student names and their id, ORDER them by name
- SELECT only the student names and addresses, only return students whose addresses are in NY

<!--3:15 10 minutes -->

## Updates to our database - Codealong

Ok, so let's say we make some mistakes to our database. That's cool, cause we can totally update it or delete information we don't like. Let's start by adding one more student:

```psql
wdi=# INSERT INTO students VALUES (6, 'Miss Take', 500, 'asdfasdfasdf');
INSERT 0 1
```

But oh no, we messed up - Miss Take doesn't live at asdfasdfasdf, she lives at 100 Main St., New York, NY.  Let's fix it:  

```psql
wdi=# UPDATE students SET address = '100 Main St., New York, NY' WHERE address = 'asdfasdfasdf';
UPDATE 1

wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
  6 | Miss Take      | 500 | 100 Main St., New York, NY
(6 rows)
```

But wait, actually, she just transferred to another GA class - no big deal!

```psql
wdi=# DELETE FROM students WHERE name = 'Miss Take';
DELETE 1

wdi=# SELECT * FROM students;
 id |      name      | age |                      address
----+----------------+-----+----------------------------------------------------
  1 | Jack Sparrow   |  28 | 50 Main St, New York, NY
  2 | Jilly Cakes    |  30 | 123 Webdev Dr. Boston, MA
  3 | Johnny Bananas |  25 | 555 Five St, Fivetowns, NY
  4 | Jackie Lackie  | 101 | 2 OldForThis Ct, Fivetowns, NY
  5 | Slaggy McRaggy |  28 |
(5 rows)

```

<!-- 3:25 10 minutes -->

## Independent Practice

There's _no way_ you're going to remember the exact syntax of everything we just did, but let's practice a habit we have been doing since week 1: finding and reading documentation. Checkout [this PostgreSQL tutorial](http://www.tutorialspoint.com/postgresql/postgresql_syntax.htm), and using the same database and datatable of students, get through as many of these SQL challenges as possible in the next 10 minutes:

- Insert five more students:
  - Nancy Gong is 40 and lives at 200 Horton Ave., Lynbrook, NY
  - Laura Rossi is 70 and listed her address as "Unlisted"
  - David Daniele is 28 and lives at 300 Dannington Ln., Washington, DC.
  - Greg Fitzgerald is 25 and did not list an address
  - Randi Fitz is 28 and lives in Oceanside, NY

- Randi wants her address to be corrected to 25 Broadway, New York, NY
- Get a list of student names and addresses who are older than 30 and order them by their address alphabetically
- Get a list of students whose first name begins with the letter "J"
- Get a list of student names who live in MA

<!-- 3:35 10 minutes -->

## Closing Thoughts

When we finally hook our apps up to databases - especially with Rails - we will have a whole slew of shortcuts we can use to get the data we need. So, wait, why the heck are we practicing SQL?  Well, let's look at what happens when you call for a particular user from a users table - with some nifty methods - in a Ruby on Rails environment when you're connected to a database:

```ruby  
User.last
  User Load (1.5ms)  SELECT  "users".* FROM "users"   ORDER BY "users"."id" DESC LIMIT 1
=> #<User id: 1, first_name: "jay", last_name: "nappy"...rest of object >
```

There's our SQL!!!

```SQL
SELECT  "users".* FROM "users"   ORDER BY "users"."id" DESC LIMIT 1
```

The Ruby/Rails scripts gets converted to raw SQL before querying the database.  So now you know the underlying concepts and query language for how the data gets returned to you.

Answer these questions:

- How does SQL relate to relational databases?
- What kinds of boolean and conditional operators can we use in SQL?

## Additional Resources
- [SQL Injection](https://www.youtube.com/watch?v=_jKylhJtPmI)

## Licensing
All content is licensed under a CC­BY­NC­SA 4.0 license.
All software code is licensed under GNU GPLv3. For commercial use or alternative licensing, please contact legal@ga.co.
