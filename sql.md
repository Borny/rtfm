# SQL

- SQL command / statement
- [MySQL](#mysql)
- PostgreSQL
- Data types
- Create
- Delete/Drop
- [Update/Alter](#updatealter)
- Check constraint
- Unique identifier
- CRUD operations
- Views
- JOIN
- Foreign Key
- Relations
- Aggregate functions
- Filter
- Window Functions
- Mathematical Functions
- String Functions
- Date Functions
- Pattern Matching
- Conditional Expressions
- Transactions
- Indexes
- [MySQL & NodeJS](#mysql--nodejs)

---

## SQL command / statement

A _statement_ is made of **tokens**.  
It should end with a semi-colon.

- Key words
- Identifiers
- Clauses

---

## MySQL

- Install Windows
- MySQL Service
- MySQL Shell
- [Install Ubuntu](#install-ubuntu)

### Install Windows

Go to <https://dev.mysql.com/downloads/installer/> and click on the **Download** button.  
Install the following packages:  

- MySQL Server
- MySQL Workbench
- MySQL Shell

### MySQL Service

To start/pause/stop the MySQL service go to the **Service app** and run the desired command.

### MySQL Shell

Open the **MySQL Shell** and connect to the database:  

```javascript
shell.connect({host: 'localhost', user: 'root', port: 3305}) // the port option is only necessary if a port other than the default (3306) was specified  
```

Then specify the language used by typing: `\sql`.  
Go back to using the default mode by typing: `\js`.  

### Install Ubuntu

[MySQL Ubuntu](https://doc.ubuntu-fr.org/mysql)  

Run the following commands to install the MySQL server on Ubuntu:

```bash
sudo apt update
sudo apt install mysql-server
mysql --version
```

Run the following command to **start** the server:  
`sudo /etc/init.d/mysql start` or `sudo service mysql start`

Restart the server:  
`sudo service mysql restart`

Stop the server:  
`sudo service mysql stop`

Secure installation:  
`sudo mysql_secure_installation`

https://medium.com/@alef.duarte/cant-connect-to-local-mysql-server-through-socket-var-run-mysqld-mysqld-sock-155d580f3a06


https://www.nixcraft.com/t/mysql-failed-error-set-password-has-no-significance-for-user-root-localhost-as-the-authentication-method-used-doesnt-store-authentication-data-in-the-mysql-server-please-consider-using-alter-user/4233

Terminate the mysql_secure_installation:  
`sudo killall -9 mysql_secure_installation`

Create user:  
`create user userName@localhost identified by '<some password here>';`

List users:  
`SELECT user FROM mysql. user`

Delete user:  
`DROP USER '<userNameToDelete>'@'host';`

#### Uninstall Ubuntu

Stop the server: `sudo service mysql stop`  
Uninstall MySQL: `sudo apt-get purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*`  
Remove MySQL data: `sudo rm -rf /etc/mysql /var/lib/mysql`  
(Optional) Remove unnecessary packages: `sudo apt autoremove`  
(Optional) Remove apt cache: `sudo apt autoclean`




#### Issue with login

[Stackoverflow link](https://stackoverflow.com/questions/72103302/mysql-installation-on-ubuntu-20-04-error-when-using-mysql-secure-installation)

---

## PostgreSQL

- Install

### Install

Go to <https://www.enterprisedb.com/downloads/postgres-postgresql-downloads> and click on the **Download** button.  
Install the default packages.  

---

## Data types

- text
- numeric
- date
- other
- files

### text

- CHAR(x) : stores text up to x characters. Shorter text will be space padded: `char(5): 'hi' => '   hi'`
- VARCHAR(x) _(most often used)_ : stores text up to x characters. Shorter strings will not be changed
- TEXT, LONGTEXT : text of any size can be stored. No need to specify the max size first.
- ENUM : only values of a predifined set are accepted  

### numeric

- INT, SMALLINT : integer numbers => good performance
- DECIMAL, NUMERIC : decimal numbers with a fixed precision => lower performance
- FLOAT, REAL : decimal numbers with floating points => good performance

### date

- DATE : value like 1968-06-21 (no hours, minutes...)
- DATETIME, TIMESTAMP : value like 1968-06-21 12:34:56  

Existing keywords: `NOW(), CURRENT_TIMESTAMP, CURRENT_DATE`  

### other

- BOOLEAN
- JSON

### files

Files are **NOT** stored in a database as they are not primitive types. Instead they are stored on a server, hard-drive where they can be accessed using a path that will be stored in the database.

---

## Create

- Database
- Table
- Default values
- NOT NULL
- Generated Columns

### Database

`CREATE DATABASE <ddName>`

Use `IF NOT EXISTS` to avoid errors:  
`CREATE DATABASE IF NOT EXISTS <dbName>`

### Table

```sql
CREATE TABLE <table-name> (
  <column_name> <TYPE>, 
  <column_name> <TYPE>, 
  <column_name> <TYPE>,
  ...
)
```

e.g:  

```sql
CREATE TABLE users (
  first_name VARCHAR(200), 
  title VARCHAR(200),
  age INT, 
  status ENUM('tourist','pro','enthusiaste'),
  ...
)
```

### Default values

Add **default values** to the tables when creating them:  

```sql
CREATE TABLE <table-name> (
  <column_name> <TYPE> DEFAULT <value>
)
```

e.g:  

```sql
CREATE TABLE users (
  creation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  updated_at DATE DEFAULT (CURRENT_DATE) -- CURRENT_DATE must be between parentheses
)
```

### NOT NULL

Ensures that the column does not accept **NULL** values:

```sql
CREATE TABLE <table-name> (
  <column_name> <TYPE> NOT NULL
)
```

### Generated Columns

Generates a column that cannot be updated manually.

```sql
CREATE TABLE users (
  first_name VARCHAR(200),
  last_name VARCHAR(200),
  full_name VARCHAR(401) GENERATED ALWAYS AS (CONCAT(first_name, ' ', last_name)),
)
```

---

## Delete/Drop

- Database
- Table

### Database

To delete a Database and all its data:  
`DROP DATABASE <database-name>`

### Table

To delete a Table and all its data:  
`DROP TABLE <table-name>`

## Update/Alter

Update the table/column:  

```sql
ALTER TABLE <table-name>
ALTER COLUMN <name> <TYPE>; -- PostgreSQL syntax 
MODIFY COLUMN <column_name> <TYPE>; -- MySQL syntax
```

**Examples:**

**Updating the type** of a **column**  

```sql
ALTER TABLE users
MODIFY COLUMN first_name TEXT; -- MySQL syntax
```

**Droping** a column named _first\_name_  

```sql
ALTER TABLE users
DROP COLUMN first_name; -- MySQL syntax
```

---

## Check constraint

Add a check to a column to ensure the data meets the required format:  

MySQL:

```sql
-- making sure that the cost field is not greater than 1500
CREATE TABLE trips (
  transportation_mode enum('train','flippers','plane'),
  cost INT CHECK (cost < 1500) 
)
```

PostgreSQL:

```sql
-- making sure that the cost field is not greater than 1500
ALTER TABLE trips
ADD CONSTRAINT cost_constraint CHECK (cost < 1500)
```

---

## Unique identifier

- PRIMARY KEY

To create a unique field value (required for ids), use the UNIQUE, PRIMARY KEY keywords.

### PRIMARY KEY

A **PRIMARY KEY** is a value that is **unique** and that **never changes** such as an _id_.  
A **Table** should only have **one** PRIMARY KEY:

```sql
CREATE TABLE trips (
  id INT PRIMARY KEY AUTO_INCREMENT, -- MySQL syntax
  id SERIAL PRIMARY KEY, -- PostgreSQL syntax
  name VARCHAR(200) NOT NULL
)
```

A **surrogate key** is a column that is created specically for having a **PRIMARY KEY** like the **id** column.  
Using a **composite** PRIMARY KEY consist of using one of the **Real** keys such as an _email address_ which should be unique.  
It is also possible to use a **combination** of **multiple columns** as the PRIMARY KEY.

e.g:

```sql
CREATE TABLE trips (
  first_name VARCHAR(200) NOT NULL,
  last_name VARCHAR(200) NOT NULL,
  PRIMARY KEY(first_name, last_name)
)
```

---

## CRUD operations

- Create/Insert
- Read/Select
- Update
- Delete

### Create/Insert

Use the **INSERT INTO** keyword to create new values in a _Table_:  

```sql
INSERT INTO <table-name> (<column_one>, <column_two>,...) VALUES (
  <value_one>,
  <value_two>,
  ...
);
```

Exemple:  

```sql
INSERT INTO heroes (first_name, title,status, age) 
VALUES (
  'Aragorn',
  'Ranger',
  'Descendant of Isildur',
  89
),
(
  'Gandalf',
  'Wizard',
  'Istari',
  2675
);
```

### Read/Select

Use the **SELECT** keyword to fetch data:  

Use **'*'** to select everything in the table:  

```sql
SELECT * FROM <table_name>
```

Specify the column name you want to display:  

```sql
SELECT <column_name>, <column_name> FROM <table_name>
```

Add an **alias** to a _column_:  

```sql
SELECT <column_name> AS <alias> FROM <table_name>
```

e.g:

```sql
SELECT last_name AS family_name FROM people
```

**Filtering** keywords and symbols:  

- = / !=
- IS / IS NOT
- < / <= / > / >=
- AND / OR / BETWEEN
- ORDER BY / DESC / ASC
- LIMIT
- OFFSET
- DISTINCT

e.g:

```sql
SELECT *  FROM people
WHERE age > 40
AND is_alive IS TRUE
AND last_seen IS NOT NULL
AND timestamp BETWEEN 23454334 AND 45647584
```

```sql
SELECT * FROM sales
ORDER BY volume DESC -- will sort the volume starting from thre biggest
LIMIT 10 -- will only return the first 10 results
OFFSET 2 -- will start after at the third result
```

```sql
SELECT DISTINCT name FROM people -- will remove all name duplicates and return a list of names
```

### Update

Use the **UPDATE**,**SET** and **WHERE** keywords to modify the data:  

```sql
UPDATE <table-name>
SET
  <column_name> = <value>,
  <column_name> = <value>
WHERE <column_name> = <value>
```

e.g:

```sql
UPDATE heroes
SET
   age = 3456,
WHERE id = 1
```

### Delete

Use the **DELETE** keyword to remove some rows:  

```sql
DELETE from <table_name>
WHERE <column_name> = <value> 
```

e.g:

```sql
DELETE from heroes
WHERE id = 1 
```

## Views

Create a view based on a query:

```sql
CREATE VIEW <view_name> AS SELECT * FROM <table_name> 
WHERE <condition> 

SELECT <table_name> FROM <view_name> 
```

e.g:

```sql
CREATE VIEW vip_user AS SELECT * FROM people 
WHERE age BETWEEN 18 AND 34

SELECT name FROM vip_user
```

---

## JOIN

- LEFT JOIN
- INNER JOIN
- RIGHT JOIN
- UNION

Use **JOINS** to merge data from different tables

### LEFT JOIN

**LEFT JOIN** will merge all the data from the desired tables that matches a condition and will return all the data of the _left_ (first) table:  

```sql
SELECT * FROM <table_name> AS <alias>
LEFT JOIN <table_name> AS <alias> ON <condition>
```

e.g:

```sql
SELECT u.name, u.age, a.address FROM users AS u
LEFT JOIN addresses AS a ON u.address_id = a.id; -- will return all the users even those who don't have a corresponding address 
```

### INNER JOIN

**INNER JOIN** will merge all the data from the desired tables that matches a condition. It will not return those who don't have a relation:  

```sql
SELECT * FROM <table_name> AS <alias>
INNER JOIN <table_name> AS <alias> ON <condition>
```

e.g:

```sql
SELECT u.name, u.age, a.address FROM users AS u
INNER JOIN addresses AS a ON u.address_id = a.id; -- will return all the users that have a corresponding address 
```

### RIGHT JOIN

**RIGHT JOIN** is the opposite of **LEFT JOIN** and is not often used.

### UNION

**UNION** will merge multiple results:  

```sql
SELECT * FROM users WHERE AGE < 3
UNION
SELECT * FROM users WHERE AGE > 56 -- will result of the two queries combined. 
-- It would be the same as: SELECT * FROM users WHERE AGE < 3 AND AGE > 56
```

---

## Foreign Key

Add a **Foreign Key** constraint to ensure data entegrity:  
Keywords:  

- REFERENCES => defines the related Table + Column
- ON DELETE =>
- ON UPDATE

```sql
CREATE TABLE <table-name> (
  ...
  <column_name> <type> REFERENCES <table-name> (<column_name>) ON DELETE 
  ...
) 
```

e.g:

```sql
CREATE TABLE users (
  address_id INT REFERENCES addresses (id) ON DELETE RESTRICT
) 
```

### Drop foreign key

```sql
ALTER TABLE <tableName> DROP FOREIGN KEY `<foreignKeyName>`;
```

---

## Relations

Tables can have different types of Relations:

- One-to-One
- One-to-Many
- Many-to-Many
- Self-Referencing

### One-to-One

This relation means that one item of a table can only have one reference from another table:  

#### Table users

| name    | age | sport_id   |
|---------|-----|------------|
| Peter   | 90  | 2          |
| Matt    | 23  | 4          |

### One-to-Many

This relation means that one table can have...???????????

### Many-to-Many

Needs an **intermediate** table.  

### Self-Referencing

A Table can reference itself:  

```sql
CREATE TABLE members (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(200) NOT NULL,
  supervisor_id INT REFERENCES members (id) ON DELETE SET NULL
) 
```

## Aggregate functions

**Aggregate functions** are built in functions in SQL that will take multiple values and combine them into one:  

```sql
- COUNT(<column_name>) => will output the number of rows
- SUM(<column_name>) => will output the sum of a given column
- MAX(<column_name>) => will output the highest number of a given column (if working with a string, it will output the word that has the last letter)
- MIN(<column_name>) => will do the opposite of MAX()
- AVG(<column_name>) => will output the average of a given column
- ROUND(<numberToRound>, <decimals>) => will round up a given number: ROUND(45.4567, 2) => output: 45.46
- GROUP BY <column_name>=> will group the rows by the given column name, it will not display duplicates
```

## Filter

- WHERE
- HAVING

### WHERE

**Where** is used to filter the result based on a condition.  
If using _GROUP BY_ it must be placed before it.  
It can't be used with Aggregate Functions.

```sql
SELECT * FROM members
WHERE age > 35
GROUP BY gender
```

### HAVING

**HAVING** is best used with **Aggregate Functions**, it is placed after **GROUP BY**:

```sql
SELECT name, SUM(age) FROM members
GROUP BY age
HAVING age > 35
```

## Window Functions

**Window Functions** add an extra column to the output:  

```sql
SELECT day, scores, SUM(scores) OVER() 
FROM games
```

- RANK() ASC/DESC => will sort the rows  

---

## Mathematical Functions

```sql
- CEIL(<column_name>) => will output the rounded up desired column 
- FLOOR(<column_name>) => will output the rounded down desired column 
- ROUND(<column_name>) => will output the rounded up or down desired column 
- TRUNCATE(<column_name>, <number_of_decimal_to_show>) => will output the desired column with some decimals removed  -- MySQL
- TRUNC(<column_name>, <number_of_decimal_to_show>) => same as TRUNCATE but for PostgreSQL -- PostgreSQL
```

---

## String Functions

```sql
- CONCAT(<column_name>, someString, <column_name>) => will output the concatenated parameters
- LOWER(<column_name>) => will output the result in lowercase characters
- UPPER(<column_name>) => will output the result in uppercase characters
- LENGTH(<string>) => will output the length of a string
- TRIM(BOTH|LEADING|TRAILING <string_to_remove> FROM <string>) => will output the string with the desired characters removed. If no character to remove is specified then the function will trim the whitespaces. By default it will trim both the leading and trailing spaces if no keyword is specified
- LTRIM(<string>) => will trim the leading white spaces, same as with the LEADING keyword 
- RTRIM(<string>) => will trim the trailing white spaces, same as with the TRAILING keyword
```

---

## Date Functions

```sql
- EXTRACT(YEAR|MONTH|DAY|HOUR|MINUTE|SECOND FROM <timestamp>) -- will output the timestamp in the desired format
- NOW() -- will output the current date
```

### PostgreSQL only

```sql
- EXTRACT(DOW FROM <timestamp>) -- will return the Day Of the Week starting with Sunday as 0 and Saturday as 6
- EXTRACT(ISDOW FROM <timestamp>) -- will return the Day Of the Week starting with Monday as 1 and Sunday as 7 
- <timestamp>::TIMESTAMP::DATE|TIME -- will return the timestamp in the desired format
- <timestamp> +|- <timestamp> -- will return the calculation of the timestamp
- <timestamp> +|- <number> -- will return the timestamp with the added days
- (<timestamp> +|- INTERVAL '<number> YEAR|MONTH|DAY|HOUR|MINUTE|SECOND')::TIMESTAMP:DATE|TIME -- will return the date with the desired values
```

### MySQL only

```sql
- WEEKDAY(<timestamp>) -- will return the Day Of the Week starting with Monday as 0 and Sunday as 6
- CONVERT(<timestamp>, DATE|TIME) -- will return the timestamp in the desired format
- DATEDIFF(<later_date>, <earlier_date>) -- will output the difference between two dates
- TIMESTAMPDIFF(YEAR|MONTH|DAY|HOUR|MINUTE|SECOND, <earlier_date>, <later_date>) -- will output the difference between two timestamp
- DATE_ADD(<date>,INTERVAL <number> YEAR|MONTH|DAY|HOUR|MINUTE|SECOND) -- will output the date with the added value
```

## Pattern Matching

- LIKE
- EXISTS
- IN / NOT IN

### LIKE

Use the **LIKE** keyword to filter strings:  

```sql
SELECT * FROM users 
WHERE name LIKE 'Ma' -- will return the names starting with 'Ma'
WHERE name LIKE '%a%' -- will return the names with an 'a'
WHERE name LIKE '__a%' -- will return the names with an 'a' in the third position only
WHERE name LIKE 'J____' -- will return the names starting with 'J' and that have a length of 5 characters
```

### EXISTS

Use the **EXISTS** keyword to check for existing values:  

```sql
SELECT EXISTS( 
  SELECT * FROM users 
  WHERE email = 'test@test.com' -- will return 0|1(MySQL) or TRUE|FALSE(PostgreSQL) depending if the required value exists or not
);    
```

### IN / NOT IN

Use the **IN** as a list to filter data:  

```sql
SELECT * FROM users 
WHERE id IN (3,45,567);    
```

With a subquery:  

```sql
SELECT * FROM users 
WHERE id NOT IN (
  SELECT user_id FROM professionals
);    
```

---

## Conditional Expressions

Use the **CASE,WHEN,THEN,END** keywords to output different values depending on conditions:  

```sql
SELECT status 
  CASE 
    WHEN status = 'Pro' THEN 'Rockstar'
    WHEN status = 'Beginner' THEN 'Noob'
  END
 FROM users
```

```sql
SELECT last_checkin 
  CASE 
    WHEN last_checkin = 0 THEN 'Sunday'
    WHEN last_checkin = 1 THEN 'Monday'
    WHEN last_checkin = 2 THEN 'Tuesday'
    WHEN last_checkin = 3 THEN 'Wednesday'
    WHEN last_checkin = 4 THEN 'Thursday'
    WHEN last_checkin = 5 THEN 'Friday'
    WHEN last_checkin = 6 THEN 'Saturday'
  END
 FROM bookings;
```

---

## Transactions

A **Transaction** ensures that all queries are successful in order to save the data in the database.

```sql
START TRANSACTIONS; -- MySQL first start a new Transaction
BEGIN; -- PostgreSQL first start a new Transaction

INSERT INTO users (name, age) -- declare a statement
VALUES ('Spike', 29);

COMMIT -- Save the result if everything worked fine
-- OR
ROLLBACK -- Cancel the transaction if an error occured
```

**!IMPORTANT :** starting a new transaction without terminating the previous one will **Commit** that previous one.

Use **SAVEPOINT** to add a checkpoint to rollback to if something goes wrong:  

```sql
START TRANSACTIONS; -- first start a new Transaction

INSERT INTO users (name, age) -- declare a statement
VALUES ('Spike', 29);

SAVEPOINT <savepoint_name>;

COMMIT -- Save the result if everything worked fine
-- OR
ROLLBACK <savepoint_name> -- Go back to the savepoint
```

---

## Indexes

**Indexes** help boost the queries by **creating** a **new table** from a desired column which is **often used** for the query but **doesn't change** value too **often**. It needs to be used with a statement with a **WHERE** clause.

- Performance analysis
- CREATE
- DROP

### Performance analysis

- EXPLAIN
- ANALYSE

**EXPLAIN** is used to get information on the planned query:  

```sql
EXPLAIN -- will output some info on the query that will be ran
SELECT * FROM users
WHERE age > 35;
```

**ANALYSE** is used to run the query and output info:  

```sql
EXPLAIN ANALYSE -- will output some info on the query and run it 
SELECT * FROM users
WHERE age > 35;
```

### CREATE

When creating a Table :  

```sql
CREATE TABLE <table_name> (
    <column_name> <type>,
    INDEX <index_name> (<column_name>)
);
```

When a Table already exists :  

```sql
CREATE INDEX <index_name> ON <table_name> (<column_name>)
```

Adding **UNIQUE** will ensure the INDEX is UNIQUE:

```sql
CREATE UNIQUE INDEX <index_name> ON <table_name> (<column_name>)
```

**!IMPORTANT :** a column with the _UNIQUE_ keyword will become an **INDEX** by default.

### DROP

```sql
DROP INDEX <index_name>
```

## MySQL & NodeJS

install the following packages in a NodeJS app:  
`npm i mysql knex`
