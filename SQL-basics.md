### Create table:
```
CREATE TABLE movies
(
columnname datatype,
columnname datatype,
FOREIGN KEY columnname REFERENCES anothertablename -------this line is optional, and can be used for foreign keys
)
```
* Optional to have `NOT NULL` after "datatype" and before the comma. Also `DEFAULT NULL`, and `PRIMARY KEY` as well
* Optional to have `UNIQUE` and it could be after (where ` NOT NULL` would be if it was there), to prevent duplicate values on same column
`ticket_prices int CHECK (ticket_prices > 0)`
* Optional to have CHECK also used to in this instance, make sure negative numbers are avoided in that column



`CONSTRAINT chosennamehere typeofcontstraint (columnname)`

`CONSTRAINT chosennamehere UNIQUE (columnname, columnname2)` #second line

* Above statements can be used before the closing parenthesis to store contraints in variable for easy access
* the second line makes it so a new row can't be added if it has two values in the specified columns that a pre-existing row also has
* `NOT NULL` can't be used as a table constraint variable thing

###  Remove Table:

`DROP TABLE movies;`

###  Create Database:

`CREATE DATABASE Muvico Theaters;`

###  Adding Data to Table:
```
INSERT INTO movies (columnname,columnname2) 
Values (val1,val2,etc..)           
```
###  Altering Table
```
ALTER TABLE movies
ADD COLUMN columnname datatype
```
#### OR
```
ALTER TABLE movies
DROP COLUMN columnname
```
#### OR 
```
ALTER TABLE movies
MODIFY columname
```
### Updating values on a specific location
```
UPDATE movies
SET duration = 120
WHERE title = 'King Kong'
```
### Delete rows

`DELETE FROM movies WHERE id= 5`

#### OR

`DELETE FROM movies (all table data deleted)`

### SELECT
```
SELECT id, title
FROM movies
WHERE genre = 'Horror'
ORDER BY duration;
```
#### OR
```
SELECT item, price, size
FROM concessions
WHERE item = 'Popcorn' OR item = 'Soda'
ORDER BY price DESC;
```

### Aggregate Functions
```
SELECT count(asterisk)
FROM tablename
```
*Can also sub in sum,avg, max, or min where "count" is used*

Example that gets both min values:
```
SELECT max(cost), min(cost)
FROM movies 
```
### Grouping
```
SELECT genre, sum(cost)
FROM MOVIES
GROUP BY genre
```

* Above example shows the total cost of all the movies, in each genre*
* Optional ||HAVING count(&ast;) > 1|| right below the GROUP BY, to only show genres that have more than 1 movie*
* Optional ||WHERE cost >= 100,000|| right above the GROUP BY, to not include movies in the sum that made less than 100k*
```
SELECT first_name, count(id)
FROM actors
GROUP BY first_name
ORDER BY count(id) DESC 
LIMIT 10;
```
* Above shows list of first names, and their number of occurences in the list(showing on two separate columns)*
* And also in order from greatest to least, and also cutting it off after 10 rows*

### GROUP BY???

Grouping by a column is like a for each on the column being grouped, and it always has an aggregate function (like sum, count etc.) used with it and has to be used with the SELECT.
Since we said it's like for each, it means each there are no repeated values in the grouped column, meaning some rows and values would be left out- this is where the aggregate function comes into play and is sort of like a reduce (like on an array) for the some of the column's hidden values to get an output

So for example, if we wanted to see all the total sales made for each customer (even if they made multiple purchases)
Table columns could be: ID -- Customername -- DatePurchased -- Amountofsale

Our query would be:
```
SELECT Customername, sum(Amountofsale)
FROM WalmartTable
GROUP BY Customername
```
