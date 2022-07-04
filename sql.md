# SQL

- SQL command / statement
- MySQL
- PostgreSQL
- Data types
- Create
- Insert
- Delete/Drop
- Update/Alter

---

## SQL command / statement

A _statement_ is made of **tokens**.  
It should end with a semi-colon.

- Key words
- Identifiers
- Clauses

---

## MySQL

- Install
- MySQL Service
- MySQL Shell
- MySQL on Ubuntu

### Install

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
shell.connect({host: 'localhost', user: 'root', port: 3305})` // the port option is only necessary if a port other than the default (3306) was specified  
```

Then specify the language used by typing: `\sql`.  
Go back to using the default mode by typing: `\js`.  

### MySQL on Ubuntu

Run the following commands to install the MySQL server on Ubuntu:

```bash
sudo apt update
sudo apt install mysql-server
mysql --version
```

Run the following command to **start** the server:  
`sudo /etc/init.d/mysql start`  

Secure installation:
`sudo mysql_secure_installation`  

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

Existing keywords: `NOW(), CURRENT_TIMESTAMP`  

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

### Database

`CREATE DATABASE <ddName>`

Use `IF NOT EXISTS` to avoid errors:  
`CREATE DATABASE IF NOT EXISTS <ddName>`

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
)
```

---

## Insert

To insert values into the table use **INSERT INTO**:

```sql
INSERT INTO <table-name> (<column-one>, <column-two>,...) VALUES (
  <value_one>,
  <value_two>,
  ...
);
```

Exemple:  

```sql
INSERT INTO users (first_name, title,status, age) VALUES (
  'Aragorn',
  'Ranger',
  'Descendant of Isildur',
  89
);
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

### Update/Alter

Update the table/column:  

```sql
ALTER TABLE <table-name>
MODIFY COLUMN <column-name> <TYPE>;
```

Example:

```sql
ALTER TABLE users
MODIFY COLUMN first_name TEXT;
```
