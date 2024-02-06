# MySQL Starters Cheat Sheet

## How to login to MySQL from terminal

To log in to MySQL from terminal, enter, `mysql -u 'your_username' -p`

If you are connecting to the root, enter, `mysql -u root -p`

You may need to use sudo in front depending on your permissions, `sudo mysql -u root -p`

-u stands for user, -p stands for password. You will be prompted for your password after you enter this command, follow that and you should successfully log in.

Keep in mind that if you are using a terminal, everything else that will cover below requires you to be successfully logged into MySQL. You can also use a MySQL tool, like MySQL Workbench.

## How to Show Users

While logged in, enter, `SELECT user, host FROM mysql.user;` to show a list of all users.

Throughout any MySQL programming make sure not to forget the semi-colons, these indicate the end of the command, if any are missing or in the wrong places your command will not work.

## How to Create Users

To create a new user, enter, `CREATE USER 'example_username'@'localhost' IDENTIFIED BY 'example_password';`

Replace "example_username" and "example_password" with your desired username and password, and this will create a user. To ensure the user has been created successfully, you can select all users again and see if your new user has been added to the list.

## How to Grant Privileges

There are two main ways to grant and revoke privileges. One is to do all at the same time, and another is to do it for specific privileges. You may have to do this in terminal depending on the privileges of the admin user associated with your MySQL Tool. If you want to change those, this is how. Workbench also offers options in settings to change privileges.

To grant all privileges, enter, `GRANT ALL PRIVILEGES ON *.* TO 'example_username'@'localhost';` "\*.\*" signifies all databases, so privileges are granted to every database.

For any new privileges to take effect you must enter, `FLUSH PRIVILEGES;` Do this for each line you enter related to privileges.

To revoke all privileges, enter, `REVOKE ALL PRIVILEGES ON *.* FROM 'example_username'@'localhost';` Note how revoke uses "FROM" instead of "TO".

To grant a specific privilege, let's say the SELECT privilege, enter, `GRANT SELECT ON *.* TO 'example_username'@'localhost';`

To revoke a specific privilege, let's use SELECT again, enter, `REVOKE SELECT ON *.* FROM 'example_username'@'localhost';`

If you want to set a privilege on a specific database, enter, `GRANT SELECT ON example_database.* TO 'example_username'@'localhost';`

Do not forget to `FLUSH PRIVILEGES;`

## How to Show, Delete, Create Databases, and Select Databases

To show all databases, enter, `SHOW DATABASES;`

To create a database, enter, `CREATE DATABASE example_database;`

To select a database, enter, `USE example_database;`

To delete a database, enter, `DROP DATABASE example_database;`

## How to Create a Table With Columns and Their Data Types

To create a table, you need to give it a name and list the columns with the type of info they will store, here is an example:

```
CREATE TABLE example_table (
    example_id INT PRIMARY KEY AUTO_INCREMENT,
    example_string VARCHAR(255),
    example_boolean BOOLEAN,
    example_mandatory VARCHAR(255) NOT NULL,
    example_date DATE 
);
```

Let's go over the datatypes defined above:

- `INT` - Integer.
- `PRIMARY KEY` - Primary keys are used for reference in other tables. You can only have one primary key per table. To reference it, you add it to another table as a `FOREIGN KEY`.
- `AUTO_INCREMENT` - The data for this field will be automatically filled as a number that increases with each entry by one.
- `VARCHAR(255)` - "VARCHAR" stands for Variable Character. This means that you can make an entry with a limit of 255 characters. The limit is adjustable.
- `BOOLEAN` - True/False. In MySQL, True is represented by a 1, and False is represented by a 0.
- `NOT NULL` - Cannot be empty, field needs a value.
- `DATE` - A date value, entered as YYYY-MM-DD. You can also use `DATETIME` if you want a timestamp as well.

This is a list of datatypes used in the table above, keep in mind there are more with many helpful uses.

## How to Show, Delete, and Drop Tables

To show all tables, enter, `SHOW TABLES;`

To delete data from a table, enter, `DELETE FROM example_table WHERE example_row;` Specify what you want to delete where `example_row` is. If you do not use a `WHERE` statement, it removes all rows, use this with caution.

To drop a table (delete the entire table), enter, `DROP TABLE example_table;`

## How to Insert Rows and Records (Single and Multiple)

Let's have the table made above act as the table the data is being entered into, to insert a single row, enter, `INSERT INTO example_table (example_string, example_boolean, example_mandatory, example_date) VALUES ('string', 1, 'mandatory data', '2000-01-01');`

To insert multiple at once, simply add multiple values:

```
INSERT INTO example_table (example_string, example_boolean, example_mandatory, example_date) VALUES
('string', 1, 'mandatory data', '2000-01-01'),
('another string', 0, 'some more mandatory data', '2010-10-10'),
('one more string', 1, 'the end of the mandatory data', '2020-06-15');
```

## How to Select With the Where Clause

Using `WHERE` allows filtering of the selection. This example selects all of the true entries in the last example, `SELECT * FROM example_table WHERE example_boolean = 1;` (* = All)

If you want to select true entries, or any other info but from a specific column, replace * with the name of the columns you want to search through.

## How to Select With the Where Clause Using a Range

You can filter for a certain condition, `SELECT example_id FROM example_table WHERE example_id > 3;`
This would show every entry in "example_id" over 3.

You can also filter with a range, `SELECT * FROM example_table WHERE example_date BETWEEN '2005-01-01' AND '2022-01-01';` This will select all entries between the specified dates.

## How to Delete an Individual Row

To delete a specific row, enter, `DELETE FROM example_table WHERE example_id = 2;`

## How to Update a Row

To update a specific row, enter, `UPDATE example_table SET example_date = '1920-03-28' WHERE example_id = 1;`

## How to Add a New Column

To add a new column, enter, `ALTER TABLE example_table ADD COLUMN example_column_add INT;`

## How to Order By and Use Distinct

To order your columns, enter, `SELECT example_id FROM example_table ORDER BY example_id ASC;` This takes example_ids and orders them in ascending order, use "DESC" for descending order. You can also select multiple columns and sort them at the same time. If you enter some string data, it will order it alphabetically.

Distinct is used to remove duplicate rows, `SELECT DISTINCT example_id, example_string FROM example_table ORDER BY example_string DESC;` This will order the strings in descending alphabetical order and display the ID numbers with them.

## How to Concatenate Columns

Concatenating is used to display a combination of columns. For this example let's assume we are combining someone's names into their full name, `SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM name_table;`

This does change the columns, it also does not create a full_name column, it is purely for displaying data. The space in the middle is added to add a space between the names when they are displayed.

## How to Select Distinct Rows

You can also use Distinct for general selection outside of ordering, `SELECT DISTINCT example_string FROM example_table;`

## How to Use Like to Search

`LIKE` is used for matching, so you can search through databases with it, `SELECT * FROM example_table WHERE example_string LIKE 'string%';` This selects every entry that starts with "string".

The placement of "%" will determine selection:

`SELECT * FROM example_table WHERE example_string LIKE '%string';` This selects every entry that ends with "string".

`SELECT * FROM example_table WHERE example_string LIKE '%string%';` This selects every entry that contains "string" anywhere within it. Think of "%" as where the rest of the string would be, if you want to find the word on the end, "%" has to be at the start as it represents the rest of the string.

There is another way to use like,
`SELECT * FROM example_table WHERE example_string LIKE '%s_%_%_%_%';` This selects every entry that has a word that starts with an "S" and has at least 4 characters after it.

## How Select Using In

In is used to filter selection with specific values, `SELECT * FROM example_table WHERE example_date IN ('2000-01-01', '2020-06-15');`

You can also do multiple at a time, `SELECT * FROM example_table WHERE example_date IN ('2000-01-01', '2020-06-15') AND example_id IN (1, 3);`

## How to Create and Remove Index

Indexes are used to find rows with specific values efficiently, to create one, enter, `CREATE INDEX example_index ON example_table (example_id, example_string);`

To drop an index, enter, `DROP INDEX example_index ON example_table;`

To select your index, enter, `SHOW INDEX FROM example_table;`

## Inner Join and Joining Multiple Tables 

Question Given: "Create Two Tables demonstrating a one to many relationship that shows a PK & FK How to use Inner Join How to JOIN Multiple Tables"

Tables and combining them:

```
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(55),
    last_name VARCHAR(55)
);

CREATE TABLE transactions (
    transaction_id INT PRIMARY KEY AUTO_INCREMENT,
    items_purchased VARCHAR(1000) NOT NULL,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO customers (first_name, last_name) VALUES
('John', 'Doe'),
('Jane', 'Smith'),
('Jen', 'Green');

INSERT INTO transactions (items_purchased, customer_id) VALUES
('apples', 1),
('paper', 2),
('soap', 3);

SELECT customers.customer_id, customers.first_name, customers.last_name, transactions.transaction_id, transactions.items_purchased FROM customers INNER JOIN transactions ON customers.customer_id = transactions.customer_id;
```
