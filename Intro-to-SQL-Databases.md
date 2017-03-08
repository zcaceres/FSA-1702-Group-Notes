# Intro to SQL Databases

Keeping your data persistent over time is a harder problem than it seems!

##### 1. What is a Database?
##### 2. SQL

___

## 1. What is a Database?
Not just things that hold information (think Phone Book).

#### Persistent, organized data accessible by a computer.

A database persists information and is accessible via code.

Here, 'accessible' means organized, queryable and manageable.

Before 1970's we were in in the Pre-Relationship DB era. Data was stored in a custom data file and read from that file. Simple, no middle layer needed.

The problem is that it's hard to change the system. Data transfer is difficult because it's not standardized.

Database management systems come along and organize our data files into a management system, creating a *shared database*.

Finally, a way for non-technical people to manage data! We don't even have to know how the data is stored to manage it.

E.F. Codd. Cranky guy at IBM that invented relational model of data. Larry Ellison stole this idea later as SQL in 1979. Blew up big time.

> Object relationship impedance mismatch: the mismatch between how we represent data in an object-oriented way and how we store data in a relational way.

#### Databases are Awesome
They are all over the place and standardized. Allows for complex actions on data. Database admins are feared by developers, loved by business people, and relied on by the government.

### Relational Database Management Systems
Data is stored in relations (tables).

Relies on a query language like SQL. The language handles retrieval of the data, the admin just uses the query language to find it.

Multi-user and thread, so that it can be accessed by many at the same time.

### ACID
* Atomicity
* Consistency
* Isolation
* Durability

Databases are supposed to be **ACID-compliant**.

>Transactions: two operations that have to be concurrent (such as removing money from your account and adding it to another account).

* **Atomic** – Transactions have to be atomic, meaning inseparable. One does not happen without the other.

* **Consistency** – The DB has to be the same before and after the transaction.

* **Isolation** – If two transactions happen at the same time, they should not affect each other. Transactions aren't layered.

* **Durability** – Information stays there. Makes it to disc. It's set in stone!

### Relational Jargon
* DBs are collections of tables.
* Tables have columns (attributes) and rows (instances or tuples).
* No duplicate rows.
* Always unique ID.

### Schema and Content
* **Schema**: The shape of the data. It's a table's blueprint for data shape/format.
* **Content**: The actual data, a row!

A schema is used to validate incoming content, to see if it fits the proper shape.

____
## 2. SQL
Used to CRUD data.

* INSERT
* SELECT
* UPDATE
* SELECT
* CREATE: make a new row (not table)

### Join Tables
Tables that use *foreign keys* that relate one table to another.

      SELECT *
      FROM Student
      WHERE age > 12
      // ^ Our predicate to narrow the query

      SELECT name, age
      FROM Student
      WHERE age > 12

JOIN statements combines rows from two or more tables, based on a common field between them.

      SELECT *
      FROM
        Student INNER JOIN Enrollment ON Student.id = Enrollment.StudentID
        INNER JOIN School ON Enrollment.SchoolID = School.id

      // Creates a temporary table of the joined result

      // We can then query this table normally, with SELECT and WHERE

#### The Many Types of Joins
**INNER JOIN**

      SELECT pets.name, owners.name
      FROM owners
      INNER JOIN pets
      pets.OwnerID = owners.ID

– Only rows that match the condition

**OUTER JOIN**

      SELECT pets.name, owners.name
      FROM owners
      FULL OUTER JOIN pets
      ON pets.OwnerID = owners.ID

– All rows that match

**LEFT JOIN**

      SELECT pets.name, owners.name
      FROM owners
      //     ^This is the 'LEFT' of the left join, so non-matching data from this table will be shown
      LEFT JOIN pets
      ON pets.OwnerID = owners.ID

– Include all matches and non-matches from left.

**RIGHT JOIN**

      SELECT pets.name, owners.name
      FROM owners
      //     ^This is the 'RIGHT' of the right join, so non-matching data from this table will be shown
      RIGHT JOIN pets
      ON pets.OwnerID = owners.ID

– Include all matches and non-matches from right.

> See slides for Venn diagram

### Questions:
**Why don't we just use a giant table?**
1. Tons of null values
2. Hard to parse and organized

**PostGres, MySQL, etc. etc.**
PostGres has some advantages. Each have different advantages and disadvantages. SQLite is portable. PostGres has its own protocol, more complex but more powerful.

___
