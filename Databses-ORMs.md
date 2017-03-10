# Databases and ORMS

#### 1. Review
____
## 1. Review
#### ACID – Good principles for transactions in a database.
* Atomicity: All or Nothing
* Consistency: Valid status before and after (rules)
* Isolation: Transactions are independent of each other
* Durability: Data sticks around

#### CAP Theorem
* Consistency: all clients see the same data
* Availability: each client can always read or write
* Partition Tolerance: systems works even if network splits

**CAP theorem says: "Pick two!"**

> Allegory with group at a concert.

Posgres chooses consistency and availability.

## 2. Sequelize
Sequelize is an Object-Relational Mapper (ORM).

It maps objects to DB relations.

It allows us to easily access SQL database from Node.js.

Features:
* Schema modeling
* Data casting (convert SQL types to JS types)
* Query building
* Hooks
* Class and instance methods of models

        POSTGRES            Sequelize
          Tables              Models
            +        ==          +
          Rows                Instances

#### Sequelize Basics
* Make a model (interactive blueprint object).
* Extends the model with
* Connect/sync the completed model to an actual table in the actual SQL db.
* Use the model to create/find instances (rows)

> See slides for creating a model

`User.sync().then(...)`;
** notice this returns a Promise!**

Sequelize uses promises to handle our DBs. Sequelize lives inside of node and knows how to communicate to various SQL dbs.

Even after you switch your DB, you could keep using Sequelize.

Sequelize uses `pg` to help us communicate with the DB. Sequelize abstracts `pg` for us.

      Sequelize ==> pg ===> postgres ==> SQL ===> File System

      [instance] <=============== data =================|

Sequelize is our ORM that allows us to interface with pg, then to postgres, then to SQL, then to the file system.

Sequelize returns and casts the data back into instances, which we can operate on in Javascript.

#### Data Flow Walkthrough
Here's what happens when you're working with Sequelize and call a method like Users.create({}) !

1. Users.create({name: 'Kate'})
2. constructs the SQL string
3. Passes SQL To `pg`
4. `pg` connects to PostGres with TCP/IP and sends SQL Query
5. PostGres is listening, receives and parses query
6. Changes data on disk
7. Sends a response back to `pg` via the Posgres protocol
8. `pg` receives and parses the data into an array of objects
9. `pg` hands the data back to Sequelize
10. Sequelize calls our model constructors on the response an constructs new objects with prototypal methods
11. Resolves the promise that User.create made and the Promise is resolved!





_____
