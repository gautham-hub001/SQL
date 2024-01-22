# SQL Basics

## Introduction to SQL:

### What is SQL?

SQL, or Structured Query Language, is a standardized programming language designed for managing, manipulating, and querying relational databases.

### What is DBMS?

Database management systems (DBMS) implements the SQL standard, such as MySQL, PostgreSQL, Oracle Database, Microsoft SQL Server, and SQLite, among others.

### Applications of SQL in databases:

1. Data Retrieval and Querying
2. Data Manipulation and Modification
3. Database Design and Schema Definition
4. Data Security and Access Control
5. Data Analysis and Reporting
6. Transaction Management and Concurrency Control
7. Database Backup, Recovery, and Maintenance

### SQL Syntax:

1. SQL statements (SELECT, INSERT, UPDATE, DELETE).
2. Components of SQL queries: SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY.

### Data Definition Language (DDL):

1. Creating tables
2. Altering tables
3. Dropping tables

### Data Manipulation Language (DML):

Inserting, updating, and deleting records.

### Data Types:

INTEGER, VARCHAR, DATE, BOOLEAN.

Choosing appropriate data types based on data requirements.

### DISTINCT keyword:

CREATE TABLE employees (
employee_id INT PRIMARY KEY,
first_name VARCHAR(50),
last_name VARCHAR(50),
department VARCHAR(50)
);

1. Retrieve Unique Departments: SELECT DISTINCT department FROM employees;
2. Retrieve Unique Combinations of First and Last Names: SELECT DISTINCT first_name, last_name FROM employees;

## Intermediate SQL

### Joins and Relationships:

JOIN operations:

1. INNER JOIN
2. LEFT JOIN
3. RIGHT JOIN
4. FULL OUTER JOIN

Ex.
CREATE TABLE employees (
employee_id INT PRIMARY KEY,
first_name VARCHAR(50),
last_name VARCHAR(50),
department_id INT
);

INSERT INTO employees VALUES
(1, 'John', 'Doe', 1),
(2, 'Jane', 'Smith', 2),
(3, 'Bob', 'Johnson', 1),
(4, 'Alice', 'Williams', 3);

CREATE TABLE departments (
department_id INT PRIMARY KEY,
department_name VARCHAR(50)
);

INSERT INTO departments VALUES
(1, 'IT'),
(2, 'HR'),
(3, 'Finance'),
(4, 'Marketing');

1. INNER JOIN:

SELECT employees.employee_id, first_name, last_name, department_name
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;

| employee_id | first_name | last_name | department_name |
| ----------- | ---------- | --------- | --------------- |
| 1           | John       | Doe       | IT              |
| 2           | Jane       | Smith     | HR              |
| 3           | Bob        | Johnson   | IT              |
| 4           | Alice      | Williams  | Finance         |

2. LEFT JOIN (or LEFT OUTER JOIN):

SELECT employees.employee_id, first_name, last_name, department_name
FROM employees
LEFT JOIN departments ON employees.department_id = departments.department_id;

| employee_id | first_name | last_name | department_name |
| ----------- | ---------- | --------- | --------------- |
| 1           | John       | Doe       | IT              |
| 2           | Jane       | Smith     | HR              |
| 3           | Bob        | Johnson   | IT              |
| 4           | Alice      | Williams  | Finance         |

3. RIGHT JOIN (or RIGHT OUTER JOIN):

SELECT employees.employee_id, first_name, last_name, department_name
FROM employees
RIGHT JOIN departments ON employees.department_id = departments.department_id;

| employee_id | first_name | last_name | department_name |
| ----------- | ---------- | --------- | --------------- |
| 1           | John       | Doe       | IT              |
| 2           | Jane       | Smith     | HR              |
| 3           | Bob        | Johnson   | IT              |
| NULL        | NULL       | NULL      | Marketing       |
| 4           | Alice      | Williams  | Finance         |

4. FULL OUTER JOIN:

SELECT employees.employee_id, first_name, last_name, department_name
FROM employees
FULL OUTER JOIN departments ON employees.department_id = departments.department_id;

| employee_id | first_name | last_name | department_name |
| ----------- | ---------- | --------- | --------------- |
| 1           | John       | Doe       | IT              |
| 2           | Jane       | Smith     | HR              |
| 3           | Bob        | Johnson   | IT              |
| NULL        | NULL       | NULL      | Marketing       |
| 4           | Alice      | Williams  | Finance         |

Establishing relationships between tables using primary and foreign keys.

### Subqueries / Nested Queries / Inner queries:

Subqueries are used to retrieve data from multiple tables.
Subqueries are enclosed within parentheses and can be used in various parts of a SQL statement such as the SELECT, FROM, WHERE, or HAVING clauses. The result of a subquery is used by the outer query to perform further operations.

1. Subquery in WHERE Clause (Filtering):

SELECT employee_id, first_name, last_name
FROM employees
WHERE department_id = (SELECT department_id FROM departments WHERE department_name = 'IT');

| employee_id | first_name | last_name |
| ----------- | ---------- | --------- |
| 1           | John       | Doe       |
| 3           | Bob        | Johnson   |

2. Subquery in SELECT Clause (Column Value):

Types of subqueries:

Single-Row Subquery
Multiple-Row Subquery
Multiple-Column Subquery
Correlated Subquery
Nested Subquery
Scalar Subquery

Ex.
CREATE TABLE employees (
employee_id INT PRIMARY KEY,
first_name VARCHAR(50),
last_name VARCHAR(50),
salary INT,
department_id INT
);

INSERT INTO employees VALUES
(1, 'John', 'Doe', 50000, 1),
(2, 'Jane', 'Smith', 45000, 2),
(3, 'Bob', 'Johnson', 55000, 1),
(4, 'Alice', 'Williams', 60000, 3);

1. Single-Row Subquery:

SELECT first_name, last_name, salary
FROM employees
WHERE salary = (SELECT MAX(salary) FROM employees);

| first_name | last_name | salary |
| ---------- | --------- | ------ |
| Alice      | Williams  | 60000  |

2. Multiple-Row Subquery:

SELECT first_name, last_name
FROM employees
WHERE department_id = (SELECT department_id FROM employees WHERE employee_id = 1);

| first_name | last_name |
| ---------- | --------- |
| John       | Doe       |
| Bob        | Johnson   |

3. Multiple-Column Subquery:

SELECT first_name, last_name
FROM employees
WHERE (first_name, last_name) IN (SELECT first_name, last_name FROM employees WHERE employee_id = 2);

| first_name | last_name |
| ---------- | --------- |
| Jane       | Smith     |

4. Correlated Subquery:

SELECT employee_id, first_name, last_name, salary
FROM employees emp_outer
WHERE salary > (SELECT AVG(salary) FROM employees WHERE department_id = emp_outer.department_id);

| employee_id | first_name | last_name | salary |
| ----------- | ---------- | --------- | ------ |
| 3           | Bob        | Johnson   | 55000  |

5. Nested Subqueries:
   a nested subquery is a subquery (or inner query) that is embedded within another query (or outer query). The result of the inner subquery is used by the outer query, and nesting can occur to multiple levels. Nested subqueries are used to perform more complex queries by using the results of one subquery as input for another.

SELECT employee_id, first_name, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

Here, the avg comes to 52500, so:

| employee_id | first_name | last_name | salary |
| ----------- | ---------- | --------- | ------ |
| 3           | Bob        | Johnson   | 55000  |
| 4           | Alice      | Williams  | 60000  |

Utilizing correlated subqueries and understanding query optimization.

### Aggregation Functions:

Used in combination with GROUP BY and HAVING clauses.
Note: It is mandatory to use aggregate functions when using GROUP BY but not vice versa.

SUM, AVG, COUNT, MAX, MIN to perform calculations on grouped data.

Ex.
CREATE TABLE orders (
order_id INT PRIMARY KEY,
product_id INT,
customer_id INT,
order_amount DECIMAL(10, 2)
);

INSERT INTO orders VALUES
(1, 101, 1, 50.00),
(2, 102, 2, 30.00),
(3, 101, 1, 20.00),
(4, 103, 3, 40.00),
(5, 102, 2, 25.00),
(6, 101, 1, 35.00);

1. Grouping by Product ID and Calculating Total Order Amount:
   SELECT product_id, SUM(order_amount) AS total_amount
   FROM orders
   GROUP BY product_id;

Expected Output:
| product_id | total_amount |
|------------|--------------|
| 101 | 105.00 |
| 102 | 55.00 |
| 103 | 40.00 |

2. Grouping by Customer ID and Counting Orders:
   SELECT customer_id, COUNT(\*) AS order_count
   FROM orders
   GROUP BY customer_id;

Expected Output:
| customer_id | order_count |
|-------------|-------------|
| 1 | 3 |
| 2 | 2 |
| 3 | 1 |

3. Grouping by Multiple Columns:
   SELECT product_id, customer_id, COUNT(\*) AS order_count
   FROM orders
   GROUP BY product_id, customer_id;

Expected Output:
| product_id | customer_id | order_count |
|------------|-------------|-------------|
| 101 | 1 | 2 |
| 101 | 3 | 1 |
| 102 | 2 | 2 |
| 103 | 3 | 1 |

4. Using HAVING to Filter Grouped Results:
   SELECT product*id, COUNT(*) AS order*count
   FROM orders
   GROUP BY product_id
   HAVING COUNT(*) > 1;

Expected Output:
| product_id | order_count |
|------------|-------------|
| 101 | 2 |
| 102 | 2 |

### Indexes and Optimization:

Creating indexes to enhance query performance.
Analyzing query execution plans and optimizing SQL queries.

Indexes play a crucial role in optimizing SQL queries by improving the speed of data retrieval. An index is a data structure associated with a table that allows for faster retrieval of rows based on the indexed columns.

#### Benefits of Indexes:

1. Faster Data Retrieval: Indexes allow the database engine to locate and retrieve specific rows more quickly.
2. Ordered Access: Sorted indexes can speed up queries that involve sorting or ordering by the indexed column.
3. Unique Constraints: Indexes enforce uniqueness constraints efficiently.

#### Considerations for Indexing:

1. Selectivity: Highly selective columns are good candidates for indexing (columns with a high cardinality).
2. Size of the Table: Large tables benefit more from indexes, but too many indexes can have downsides.
3. Write Operations: Indexes can slow down write operations (INSERT, UPDATE, DELETE), so it's a trade-off.

#### Common Indexing Strategies:

1. Single-Column Index: Indexing a single column.
2. Composite Index: Indexing multiple columns.
3. Clustered Index: Determines the physical order of rows in a table.
4. Covering Index: Includes all the columns needed for a query, reducing the need to access the actual table.

Ex.
-- Creating an index on the 'salary' column
CREATE INDEX idx_salary ON employees(salary);

-- Query using the indexed column
SELECT \* FROM employees WHERE salary > 50000;

#### Optimizing SQL Queries:

Use WHERE Clauses Effectively: Indexes are most effective for columns used in WHERE clauses.
Avoid Functions on Indexed Columns: Applying functions on indexed columns can prevent index usage.
JOIN Conditions: Index the columns used in JOIN conditions.
LIMIT the Result Set: Use LIMIT or TOP to limit the number of rows returned.

1. Use Indexes:

Example: Creating an index on the salary column.
CREATE INDEX idx_salary ON employees(salary);

2. Write Efficient Queries:

Example: Avoid using SELECT _, only select necessary columns.
-- Bad
SELECT _ FROM employees WHERE department_id = 1;

-- Good
SELECT employee_id, first_name, last_name FROM employees WHERE department_id = 1;

3. Optimize JOIN Operations:

Example: Using INNER JOIN instead of LEFT JOIN.
-- Bad
SELECT \* FROM employees e LEFT JOIN departments d ON e.department_id = d.department_id;

-- Good
SELECT \* FROM employees e INNER JOIN departments d ON e.department_id = d.department_id;

4. Limit Data Retrieval:

Example: Using LIMIT to retrieve a specific number of rows.
SELECT \* FROM employees LIMIT 10;

5. Avoid Nested Subqueries When Possible:

Example: Rewriting a nested subquery using JOIN.
-- Bad
SELECT \* FROM employees WHERE department_id IN (SELECT department_id FROM departments WHERE department_name = 'IT');

-- Good
SELECT e.\* FROM employees e JOIN departments d ON e.department_id = d.department_id WHERE d.department_name = 'IT';

6. Batch Operations:

Example: Using a batch operation for multiple INSERTs.
-- Bad
INSERT INTO my_table (column1) VALUES (value1);
INSERT INTO my_table (column1) VALUES (value2);
-- ...

-- Good
INSERT INTO my_table (column1) VALUES (value1), (value2), ...;

7. Properly Configure Database Server:

Example: Adjusting the size of the query cache.
-- Increase query cache size
SET GLOBAL query_cache_size = 67108864;

8. Analyze and Update Statistics:

Example: Running ANALYZE TABLE to update statistics.
ANALYZE TABLE employees;

9. Avoid Using Cursors:

Example: Using a set-based operation instead of a cursor.
-- Cursor example
DECLARE employee_cursor CURSOR FOR SELECT employee_id FROM employees;
OPEN employee_cursor;
FETCH employee_cursor INTO @employee_id;

-- Set-based operation
UPDATE employees SET salary = salary \* 1.1;

10. Use Stored Procedures:

Example: Creating a simple stored procedure.
DELIMITER //
CREATE PROCEDURE GetEmployeeById(IN emp_id INT)
BEGIN
SELECT \* FROM employees WHERE employee_id = emp_id;
END //
DELIMITER ;

### Transactions and Concurrency:

Understanding SQL transactions, ACID properties (Atomicity, Consistency, Isolation, Durability).

Implementing transaction control statements (BEGIN TRANSACTION, COMMIT, ROLLBACK).

## Advanced SQL

### Cursors

Cursors in SQL are database objects that allow you to iterate over a result set, usually within the context of a stored procedure or a batch of SQL statements. Many operations can be performed using set-based operations, which are generally more efficient than cursor-based approaches.
For example, use UPDATE or INSERT...SELECT statements for batch operations instead of cursor-based operations.

Types of Cursors:

1. Forward-Only Cursor: Can only move forward through the result set. Most efficient but limited functionality.
2. Scrollable Cursor: Can move backward and forward through the result set. More flexible but potentially slower.

#### Basic Cursor Syntax:

The basic syntax for declaring and using a cursor involves the following steps:
Declare Cursor: Define the cursor, specifying the SELECT statement that retrieves the result set.
Open Cursor: Open the cursor to start processing the result set.
Fetch: Retrieve a row from the result set into variables.
Close Cursor: Close the cursor when done processing.

### Stored Procedures and Functions:

Creating and executing stored procedures and functions.
Parameterized stored procedures and dynamic SQL.

A stored procedure is a precompiled collection of one or more SQL statements or procedural logic, which is stored in a relational database management system (RDBMS) for later use. Stored procedures offer several advantages, including modularity, reusability, security, and improved performance.

Ex.
CREATE PROCEDURE GetEmployeeById
@emp_id INT
AS
BEGIN
SELECT \* FROM employees WHERE employee_id = @emp_id;
END;

Execution:
EXEC procedure_name parameter1_value, parameter2_value, ...;

Returning Data:
Stored procedures can return result sets using SELECT statements.
Example:
CREATE PROCEDURE GetEmployeesByDepartment
@dept_id INT
AS
BEGIN
SELECT \* FROM employees WHERE department_id = @dept_id;
END;

Output Parameters:
Stored procedures can have output parameters to return values.
Example:
CREATE PROCEDURE CalculateSalary
@emp_id INT,
@new_salary INT OUTPUT
AS
BEGIN
-- Perform calculations
SET @new_salary = ...;
END;

Error Handling:
Stored procedures can include error handling using TRY...CATCH blocks.
Example:
CREATE PROCEDURE InsertEmployee
@emp_id INT,
@first_name VARCHAR(50),
@last_name VARCHAR(50)
AS
BEGIN
BEGIN TRY
INSERT INTO employees VALUES (@emp_id, @first_name, @last_name);
END TRY
BEGIN CATCH
-- Handle the error
PRINT ERROR_MESSAGE();
END CATCH
END;

Dynamic SQL:
Stored procedures can use dynamic SQL to build and execute SQL statements dynamically.
Example:
CREATE PROCEDURE ExecuteDynamicSQL
@sql_query NVARCHAR(MAX)
AS
BEGIN
EXEC sp_executesql @sql_query;
END;

### Dynamic SQL:

Dynamic SQL refers to the generation and execution of SQL statements during runtime rather than at compile time. In other words, the SQL statements are constructed as strings and then executed dynamically using a mechanism provided by the database engine. This approach offers flexibility in building SQL statements based on varying conditions or user inputs. Dynamic SQL involves constructing SQL statements as strings in a programming language (e.g., T-SQL, PL/SQL, or others) during runtime.

### Triggers:

Triggers are special types of stored procedures that automatically execute in response to certain events on a particular table or view. These events can include INSERT, UPDATE, DELETE, or even a combination of these operations. Triggers are commonly used for enforcing business rules, maintaining data integrity, and automating certain tasks in the database.
Implementing triggers to automate actions based on database events (INSERT, UPDATE, DELETE).
Understanding trigger types (AFTER, BEFORE) and best practices.

Types of Triggers:
BEFORE Triggers: Executed before the specified event (e.g., BEFORE INSERT, BEFORE UPDATE).
AFTER Triggers: Executed after the specified event (e.g., AFTER INSERT, AFTER UPDATE).

Ex.
CREATE TRIGGER trigger_name
{BEFORE | AFTER} {INSERT | UPDATE | DELETE}
ON table_name
[FOR EACH ROW]
BEGIN
-- Trigger logic (SQL statements)
END;

Triggers can access the data involved in the triggering event, usually using the NEW and OLD keywords.

1. Disable triggers on a table
   ALTER TABLE table_name DISABLE TRIGGER ALL;

2. Enable triggers on a table
   ALTER TABLE table_name ENABLE TRIGGER ALL;

### Views and Materialized Views:

Creating and managing views to encapsulate complex queries.
Utilizing materialized views for improved query performance and data access.
Window Functions:

Exploring window functions (ROW_NUMBER, RANK, DENSE_RANK, LEAD, LAG).
Applying window functions for advanced data analysis and reporting.

### Advanced Query Techniques:

Mastering advanced SQL query techniques, recursive queries, pivot/unpivot operations, and hierarchical data querying.
Leveraging SQL extensions and features specific to different database management systems (DBMS) like PostgreSQL, MySQL, SQL Server, Oracle, etc.
Practical Application
