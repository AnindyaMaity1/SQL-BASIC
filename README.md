# SQL Handbook - Complete Reference & Interview Guide

## Table of Contents
1. [SQL Basics](#sql-basics)
2. [Data Operations (DML, DDL, DCL, TCL)](#data-operations)
3. [Joins, Subqueries & Views](#joins-subqueries-views)
4. [Indexes, Triggers & Stored Procedures](#indexes-triggers-stored-procedures)
5. [Aggregate Functions & Window Functions](#aggregate-functions-window-functions)
6. [Normalization & Denormalization](#normalization-denormalization)
7. [Advanced Concepts & Security](#advanced-concepts-security)
8. [Top 100+ SQL Interview Questions](#interview-questions)

---

## SQL Basics

### What is SQL?
SQL (Structured Query Language) is a standardized programming language designed for managing and manipulating relational databases. It enables users to create, read, update, and delete data stored in tables through declarative statements.

### Database Fundamentals
A database is an organized collection of structured data stored electronically. Relational databases organize data into tables with rows (records) and columns (fields), establishing relationships between different entities through keys.

### Tables and Schemas
Tables are the fundamental storage units in relational databases, consisting of rows and columns. A schema defines the logical structure, including table definitions, relationships, constraints, and data types within a database system.

### Data Types

#### Numeric Data Types
- **INT/INTEGER**: Stores whole numbers from -2,147,483,648 to 2,147,483,647
- **BIGINT**: Stores large integers from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807
- **FLOAT**: Stores floating-point numbers with decimal precision
- **DECIMAL(p,s)**: Stores fixed-point numbers with specified precision and scale

#### Character Data Types
- **CHAR(n)**: Fixed-length character string, padded with spaces if shorter than n
- **VARCHAR(n)**: Variable-length character string, maximum length n
- **TEXT**: Stores large text data without length restrictions

#### Date and Time Data Types
- **DATE**: Stores date values (YYYY-MM-DD format)
- **TIME**: Stores time values (HH:MM:SS format)
- **DATETIME/TIMESTAMP**: Stores both date and time values
- **YEAR**: Stores year values in four-digit format

#### Other Data Types
- **BOOLEAN**: Stores TRUE or FALSE values
- **BINARY/VARBINARY**: Stores binary data
- **ENUM**: Stores one value from a predefined list of values

### Keys in SQL

#### Primary Key
A primary key uniquely identifies each record in a table. It cannot contain NULL values and must be unique across all rows. Only one primary key is allowed per table.

#### Foreign Key
A foreign key establishes relationships between tables by referencing the primary key of another table. It ensures referential integrity and maintains data consistency across related tables.

#### Unique Key
A unique key ensures all values in a column are distinct. Unlike primary keys, unique keys can contain NULL values and multiple unique keys can exist per table.

#### Composite Key
A composite key consists of multiple columns combined to uniquely identify records. It's used when no single column can serve as a unique identifier for table rows.

#### Candidate Key
A candidate key is any column or combination of columns that can uniquely identify records. Among candidate keys, one is chosen as the primary key for the table.

---

## Data Operations

### DDL (Data Definition Language)
DDL commands define and modify database structure including tables, schemas, indexes, and constraints. These commands auto-commit and cannot be rolled back in most database systems.

#### CREATE
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    salary DECIMAL(10,2),
    hire_date DATE
);
```

#### ALTER
```sql
ALTER TABLE employees 
ADD COLUMN department VARCHAR(50);

ALTER TABLE employees 
MODIFY COLUMN salary DECIMAL(12,2);
```

#### DROP
```sql
DROP TABLE employees; -- Removes table and all data
DROP DATABASE company; -- Removes entire database
```

#### TRUNCATE
```sql
TRUNCATE TABLE employees; -- Removes all data, keeps structure
```

### DML (Data Manipulation Language)
DML commands manipulate data within existing database structures. These operations can be rolled back using transaction control statements and affect the actual data stored in tables.

#### INSERT
```sql
INSERT INTO employees (id, name, salary, hire_date)
VALUES (1, 'John Doe', 50000.00, '2024-01-15');

INSERT INTO employees (id, name, salary)
VALUES (2, 'Jane Smith', 60000.00);
```

#### SELECT
```sql
SELECT name, salary FROM employees 
WHERE salary > 50000 
ORDER BY hire_date DESC;

SELECT * FROM employees 
WHERE department = 'IT' AND salary BETWEEN 40000 AND 80000;
```

#### UPDATE
```sql
UPDATE employees 
SET salary = salary * 1.1 
WHERE department = 'IT';

UPDATE employees 
SET department = 'HR' 
WHERE id = 1;
```

#### DELETE
```sql
DELETE FROM employees 
WHERE hire_date < '2020-01-01';

DELETE FROM employees 
WHERE department IS NULL;
```

### DCL (Data Control Language)
DCL commands manage user access rights and permissions within the database system. These commands are typically executed by database administrators to control data security and access.

#### GRANT
```sql
GRANT SELECT, INSERT ON employees TO 'hr_user'@'localhost';
GRANT ALL PRIVILEGES ON company.* TO 'admin_user'@'%';
```

#### REVOKE
```sql
REVOKE INSERT ON employees FROM 'hr_user'@'localhost';
REVOKE ALL PRIVILEGES ON company.* FROM 'admin_user'@'%';
```

### TCL (Transaction Control Language)
TCL commands manage database transactions, ensuring data consistency and enabling recovery from failures. These commands work with DML operations to maintain database integrity through transaction boundaries.

#### COMMIT
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- Makes changes permanent
```

#### ROLLBACK
```sql
BEGIN TRANSACTION;
DELETE FROM employees WHERE department = 'IT';
ROLLBACK; -- Undoes all changes in transaction
```

#### SAVEPOINT
```sql
BEGIN TRANSACTION;
INSERT INTO employees VALUES (1, 'John', 50000, '2024-01-01');
SAVEPOINT sp1;
UPDATE employees SET salary = 55000 WHERE id = 1;
ROLLBACK TO sp1; -- Rolls back to savepoint
COMMIT;
```

---

## Joins, Subqueries & Views

### SQL Joins
Joins combine data from multiple tables based on related columns. They enable querying across normalized database structures and retrieving comprehensive information from related entities.

#### INNER JOIN
```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.id;
```
Returns only matching records from both tables.

#### LEFT JOIN (LEFT OUTER JOIN)
```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.id;
```
Returns all records from left table and matching records from right table.

#### RIGHT JOIN (RIGHT OUTER JOIN)
```sql
SELECT e.name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.id;
```
Returns all records from right table and matching records from left table.

#### FULL OUTER JOIN
```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.id;
```
Returns all records from both tables, with NULLs for non-matching records.

#### CROSS JOIN
```sql
SELECT e.name, p.project_name
FROM employees e
CROSS JOIN projects p;
```
Returns Cartesian product of both tables (all possible combinations).

#### SELF JOIN
```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.id;
```
Joins a table with itself using aliases.

### Subqueries
Subqueries are nested queries that execute within another query. They can be used in SELECT, WHERE, FROM, and HAVING clauses to create complex filtering and data retrieval logic.

#### Scalar Subquery
```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

#### Correlated Subquery
```sql
SELECT e1.name, e1.salary
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id
);
```

#### EXISTS Subquery
```sql
SELECT name FROM employees e
WHERE EXISTS (
    SELECT 1 FROM projects p 
    WHERE p.employee_id = e.id
);
```

#### IN Subquery
```sql
SELECT name FROM employees
WHERE department_id IN (
    SELECT id FROM departments 
    WHERE location = 'New York'
);
```

### Views
Views are virtual tables created from SQL queries. They provide security, simplify complex queries, and present customized data perspectives without storing actual data physically in the database.

#### Creating Views
```sql
CREATE VIEW high_earners AS
SELECT name, salary, department_id
FROM employees
WHERE salary > 70000;

CREATE VIEW dept_summary AS
SELECT d.department_name, COUNT(e.id) as employee_count, AVG(e.salary) as avg_salary
FROM departments d
LEFT JOIN employees e ON d.id = e.department_id
GROUP BY d.id, d.department_name;
```

#### Using Views
```sql
SELECT * FROM high_earners WHERE department_id = 1;
UPDATE high_earners SET salary = 75000 WHERE name = 'John Doe';
```

---

## Indexes, Triggers & Stored Procedures

### Indexes
Indexes are database objects that improve query performance by creating shortcuts to data. They speed up SELECT operations but may slow down INSERT, UPDATE, and DELETE operations due to index maintenance overhead.

#### Creating Indexes
```sql
-- Single column index
CREATE INDEX idx_employee_name ON employees(name);

-- Composite index
CREATE INDEX idx_dept_salary ON employees(department_id, salary);

-- Unique index
CREATE UNIQUE INDEX idx_employee_email ON employees(email);
```

#### Types of Indexes
- **Clustered Index**: Physically orders table data, one per table
- **Non-Clustered Index**: Logical ordering with pointers to data rows
- **Unique Index**: Ensures uniqueness and improves performance
- **Composite Index**: Covers multiple columns for complex queries

### Triggers
Triggers are stored procedures that automatically execute in response to specific database events. They enforce business rules, maintain data integrity, and perform automated tasks without explicit invocation.

#### Types of Triggers
- **BEFORE**: Executes before the triggering event
- **AFTER**: Executes after the triggering event
- **INSTEAD OF**: Replaces the triggering event (views only)

#### Creating Triggers
```sql
CREATE TRIGGER audit_employee_changes
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO audit_log (table_name, operation, old_value, new_value, change_date)
    VALUES ('employees', 'UPDATE', OLD.salary, NEW.salary, NOW());
END;

CREATE TRIGGER validate_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    IF NEW.salary < 0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Salary cannot be negative';
    END IF;
END;
```

### Stored Procedures
Stored procedures are precompiled SQL code blocks stored in the database. They improve performance, enhance security, promote code reusability, and reduce network traffic between applications and database servers.

#### Creating Stored Procedures
```sql
DELIMITER //
CREATE PROCEDURE GetEmployeesByDepartment(IN dept_id INT)
BEGIN
    SELECT id, name, salary, hire_date
    FROM employees
    WHERE department_id = dept_id
    ORDER BY hire_date;
END //
DELIMITER ;

-- Procedure with output parameter
DELIMITER //
CREATE PROCEDURE GetDepartmentStats(
    IN dept_id INT,
    OUT emp_count INT,
    OUT avg_salary DECIMAL(10,2)
)
BEGIN
    SELECT COUNT(*), AVG(salary)
    INTO emp_count, avg_salary
    FROM employees
    WHERE department_id = dept_id;
END //
DELIMITER ;
```

#### Executing Stored Procedures
```sql
CALL GetEmployeesByDepartment(1);

CALL GetDepartmentStats(1, @count, @avg);
SELECT @count, @avg;
```

---

## Aggregate Functions & Window Functions

### Aggregate Functions
Aggregate functions perform calculations on multiple rows and return single values. They're commonly used with GROUP BY clause to summarize data and generate reports from large datasets.

#### Common Aggregate Functions
```sql
-- Basic aggregates
SELECT 
    COUNT(*) as total_employees,
    COUNT(DISTINCT department_id) as departments,
    SUM(salary) as total_payroll,
    AVG(salary) as average_salary,
    MIN(salary) as lowest_salary,
    MAX(salary) as highest_salary
FROM employees;

-- With GROUP BY
SELECT 
    department_id,
    COUNT(*) as employee_count,
    AVG(salary) as avg_salary,
    SUM(salary) as dept_payroll
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;
```

### GROUP BY and HAVING
GROUP BY groups rows with same values into summary rows, while HAVING filters grouped results. HAVING clause is used instead of WHERE when filtering aggregate results.

```sql
SELECT department_id, COUNT(*) as emp_count, AVG(salary) as avg_sal
FROM employees
WHERE hire_date > '2020-01-01'
GROUP BY department_id
HAVING COUNT(*) > 3 AND AVG(salary) > 50000
ORDER BY avg_sal DESC;
```

### Window Functions
Window functions perform calculations across related rows without collapsing results into single values. They provide analytical capabilities while maintaining individual row details in the result set.

#### ROW_NUMBER, RANK, DENSE_RANK
```sql
SELECT 
    name, 
    salary,
    department_id,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) as row_num,
    RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) as rank_pos,
    DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) as dense_rank
FROM employees;
```

#### Running Totals and Moving Averages
```sql
SELECT 
    name,
    salary,
    hire_date,
    SUM(salary) OVER (ORDER BY hire_date ROWS UNBOUNDED PRECEDING) as running_total,
    AVG(salary) OVER (ORDER BY hire_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) as moving_avg
FROM employees
ORDER BY hire_date;
```

#### LEAD and LAG
```sql
SELECT 
    name,
    salary,
    LAG(salary, 1) OVER (ORDER BY salary) as prev_salary,
    LEAD(salary, 1) OVER (ORDER BY salary) as next_salary,
    salary - LAG(salary, 1) OVER (ORDER BY salary) as salary_diff
FROM employees;
```

#### FIRST_VALUE and LAST_VALUE
```sql
SELECT 
    name,
    salary,
    department_id,
    FIRST_VALUE(salary) OVER (PARTITION BY department_id ORDER BY salary DESC) as highest_in_dept,
    LAST_VALUE(salary) OVER (
        PARTITION BY department_id 
        ORDER BY salary DESC 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) as lowest_in_dept
FROM employees;
```

---

## Normalization & Denormalization

### Normalization
Normalization is a database design technique that organizes tables to minimize redundancy and dependency. It divides larger tables into smaller, related tables and establishes relationships through foreign keys.

#### First Normal Form (1NF)
- Eliminate repeating groups
- Each column contains atomic values
- Each column contains values of same data type
- Each row is unique

#### Second Normal Form (2NF)
- Must be in 1NF
- All non-key columns fully depend on primary key
- Eliminate partial dependencies

#### Third Normal Form (3NF)
- Must be in 2NF
- No transitive dependencies
- Non-key columns depend only on primary key

#### Boyce-Codd Normal Form (BCNF)
- Must be in 3NF
- Every determinant must be a candidate key
- Eliminates anomalies in 3NF

#### Example of Normalization
```sql
-- Before normalization (violates 1NF, 2NF, 3NF)
CREATE TABLE order_info (
    order_id INT,
    customer_name VARCHAR(100),
    customer_address VARCHAR(200),
    product_name VARCHAR(100),
    product_price DECIMAL(10,2),
    quantity INT
);

-- After normalization
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    customer_address VARCHAR(200)
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    product_price DECIMAL(10,2)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_item_id INT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity INT,
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

### Denormalization
Denormalization is the process of adding redundancy to normalized databases to improve query performance. It trades data integrity for faster read operations and reduced join complexity.

#### When to Denormalize
- Read-heavy workloads with complex joins
- Reporting and analytics requirements
- Performance bottlenecks in normalized design
- Data warehouse environments

#### Example of Denormalization
```sql
-- Normalized tables
SELECT c.customer_name, o.order_date, p.product_name, oi.quantity, p.product_price
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p ON oi.product_id = p.product_id;

-- Denormalized table for reporting
CREATE TABLE order_summary (
    order_id INT,
    customer_name VARCHAR(100),
    customer_address VARCHAR(200),
    order_date DATE,
    product_name VARCHAR(100),
    product_price DECIMAL(10,2),
    quantity INT,
    total_amount DECIMAL(10,2)
);
```

#### Advantages of Normalization
- Eliminates data redundancy
- Ensures data integrity
- Reduces storage space
- Simplifies data maintenance

#### Advantages of Denormalization
- Improves query performance
- Reduces complex joins
- Better for read-heavy workloads
- Simplifies reporting queries

---

## Advanced Concepts & Security

### ACID Properties
ACID properties ensure reliable transaction processing in database systems. These fundamental principles guarantee data consistency, integrity, and reliability even during system failures or concurrent operations.

#### Atomicity
Ensures that transactions are treated as single units that either completely succeed or fail entirely. If any part of a transaction fails, the entire transaction is rolled back to maintain consistency.

#### Consistency
Guarantees that transactions bring the database from one valid state to another, maintaining all defined rules, constraints, and relationships throughout the process.

#### Isolation
Manages concurrent transaction execution by ensuring that simultaneous transactions don't interfere with each other, preventing data corruption and maintaining result predictability.

#### Durability
Ensures that committed transaction results persist permanently, even after system crashes or power failures, through logging and storage mechanisms.

### Transaction Isolation Levels
```sql
-- Read Uncommitted (lowest isolation)
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;

-- Read Committed (default in most systems)
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;

-- Repeatable Read
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;

-- Serializable (highest isolation)
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

### SQL Injection Prevention
SQL injection occurs when malicious SQL code is inserted into application queries. Prevention techniques include parameterized queries, input validation, and stored procedures.

#### Vulnerable Code Example
```sql
-- NEVER DO THIS - Vulnerable to SQL injection
SELECT * FROM users WHERE username = '" + userInput + "' AND password = '" + passwordInput + "'";
```

#### Secure Code Examples
```sql
-- Use parameterized queries
PREPARE stmt FROM 'SELECT * FROM users WHERE username = ? AND password = ?';
SET @username = 'john_doe';
SET @password = 'secure_password';
EXECUTE stmt USING @username, @password;

-- Stored procedure approach
DELIMITER //
CREATE PROCEDURE AuthenticateUser(IN p_username VARCHAR(50), IN p_password VARCHAR(100))
BEGIN
    SELECT user_id, username, email
    FROM users
    WHERE username = p_username AND password = SHA2(p_password, 256);
END //
DELIMITER ;
```

### Database Partitioning
Partitioning divides large tables into smaller, manageable segments while maintaining logical unity. It improves query performance, enables parallel processing, and simplifies maintenance operations.

#### Horizontal Partitioning (Sharding)
```sql
-- Range partitioning by date
CREATE TABLE sales (
    id INT,
    sale_date DATE,
    amount DECIMAL(10,2)
)
PARTITION BY RANGE (YEAR(sale_date)) (
    PARTITION p2020 VALUES LESS THAN (2021),
    PARTITION p2021 VALUES LESS THAN (2022),
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024)
);

-- Hash partitioning
CREATE TABLE customers (
    customer_id INT,
    name VARCHAR(100),
    email VARCHAR(100)
)
PARTITION BY HASH(customer_id) PARTITIONS 4;
```

#### Vertical Partitioning
```sql
-- Split frequently and rarely accessed columns
CREATE TABLE employee_basic (
    employee_id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    department_id INT
);

CREATE TABLE employee_details (
    employee_id INT PRIMARY KEY,
    address TEXT,
    phone VARCHAR(20),
    emergency_contact TEXT,
    FOREIGN KEY (employee_id) REFERENCES employee_basic(employee_id)
);
```

### Query Optimization
Query optimization involves improving SQL query performance through proper indexing, query restructuring, and understanding execution plans.

```sql
-- Use EXPLAIN to analyze query performance
EXPLAIN SELECT e.name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.id
WHERE e.salary > 50000;

-- Optimization techniques
-- 1. Use indexes on frequently searched columns
CREATE INDEX idx_salary ON employees(salary);

-- 2. Avoid SELECT *
SELECT e.name, e.salary FROM employees e WHERE e.department_id = 1;

-- 3. Use EXISTS instead of IN for subqueries
SELECT name FROM employees e
WHERE EXISTS (SELECT 1 FROM projects p WHERE p.employee_id = e.id);

-- 4. Use LIMIT for large result sets
SELECT name, salary FROM employees ORDER BY salary DESC LIMIT 10;
```

---

## Interview Questions

### Basic SQL Interview Questions (1-30)

#### 1. What is SQL and what are its main advantages?
SQL (Structured Query Language) is a standardized programming language for managing relational databases. It provides declarative syntax for data manipulation, supports complex queries, ensures data integrity, and offers platform independence across different database systems.

#### 2. What are the different types of SQL commands?
SQL commands are categorized into DDL (CREATE, ALTER, DROP), DML (SELECT, INSERT, UPDATE, DELETE), DCL (GRANT, REVOKE), and TCL (COMMIT, ROLLBACK, SAVEPOINT). Each category serves specific database management functions.

#### 3. Explain the difference between DELETE, DROP, and TRUNCATE.
DELETE removes specific rows based on conditions and can be rolled back. DROP removes entire database objects permanently. TRUNCATE removes all table data quickly, resets identity columns, but cannot be rolled back in most databases.

#### 4. What is a primary key and can a table have multiple primary keys?
A primary key uniquely identifies each row in a table, cannot contain NULL values, and ensures entity integrity. A table can have only one primary key, though it may consist of multiple columns (composite primary key).

#### 5. What is the difference between CHAR and VARCHAR data types?
CHAR stores fixed-length strings, padding with spaces if shorter than specified length. VARCHAR stores variable-length strings up to maximum length, using only necessary space. CHAR is faster for fixed-size data, VARCHAR saves storage space.

#### 6. Explain the concept of foreign key and referential integrity.
Foreign key establishes relationships between tables by referencing another table's primary key. Referential integrity ensures that foreign key values either match existing primary key values or are NULL, maintaining consistent relationships across tables.

#### 7. What are SQL constraints and name different types?
Constraints enforce rules on table data to ensure integrity. Types include PRIMARY KEY (uniqueness), FOREIGN KEY (relationships), UNIQUE (distinct values), NOT NULL (required values), CHECK (custom conditions), and DEFAULT (automatic values).

#### 8. What is the difference between WHERE and HAVING clauses?
WHERE filters individual rows before grouping operations and cannot use aggregate functions. HAVING filters grouped results after GROUP BY operations and can use aggregate functions to filter group-level conditions.

#### 9. Explain the difference between UNION and UNION ALL.
UNION combines result sets from multiple queries, removing duplicate rows and requiring identical column structures. UNION ALL combines results without removing duplicates, providing faster performance when duplicates are acceptable or impossible.

#### 10. What are aggregate functions? Give examples.
Aggregate functions perform calculations on multiple rows, returning single values. Examples include COUNT() for row counting, SUM() for totals, AVG() for averages, MIN()/MAX() for extreme values, and GROUP_CONCAT() for string concatenation.

#### 11. What is a subquery and what are its types?
A subquery is a query nested within another query, executed first to provide results for the outer query. Types include scalar (single value), correlated (references outer query), and table subqueries (multiple rows/columns).

#### 12. Explain the difference between correlated and non-correlated subqueries.
Non-correlated subqueries execute independently of the outer query and can be run separately. Correlated subqueries reference columns from the outer query, executing once for each outer query row, typically slower but more flexible.

#### 13. What is a view in SQL and what are its advantages?
A view is a virtual table created from SQL queries, presenting data without physical storage. Advantages include data security, query simplification, abstraction of complexity, and providing different perspectives on underlying table data.

#### 14. Can you update data through a view?
Yes, but with restrictions. Simple views based on single tables without aggregate functions, DISTINCT, or GROUP BY are generally updatable. Complex views with joins, calculations, or derived columns may not allow updates.

#### 15. What is an index and how does it improve performance?
An index is a database object that creates shortcuts to table data, similar to book indexes. It improves SELECT query performance by reducing data search time but may slow INSERT/UPDATE/DELETE operations due to index maintenance overhead.

#### 16. What are the different types of indexes?
Index types include clustered (physically orders data), non-clustered (logical pointers), unique (enforces uniqueness), composite (multiple columns), partial (filtered), and covering indexes (includes all query columns for complete coverage).

#### 17. What is database normalization and why is it important?
Normalization organizes database tables to minimize redundancy and dependency through systematic decomposition. It prevents data anomalies, ensures consistency, reduces storage requirements, and maintains data integrity by eliminating duplicate information.

#### 18. Explain the different normal forms.
1NF eliminates repeating groups, 2NF removes partial dependencies, 3NF eliminates transitive dependencies, and BCNF ensures every determinant is a candidate key. Each level builds upon the previous, increasing data integrity.

#### 19. What is denormalization and when would you use it?
Denormalization adds controlled redundancy to normalized databases for performance optimization. Use it for read-heavy applications, reporting systems, data warehouses, or when complex joins significantly impact query performance and response times.

#### 20. What are ACID properties in database transactions?
ACID ensures reliable transaction processing: Atomicity (all-or-nothing execution), Consistency (maintains data validity), Isolation (concurrent transaction independence), and Durability (permanent change persistence after commitment). Critical for data integrity.

#### 21. What is a database transaction?
A transaction is a logical unit of work comprising one or more SQL operations that must execute completely or not at all. Transactions ensure data consistency, enable error recovery, and maintain database integrity during complex operations.

#### 22. Explain COMMIT and ROLLBACK commands.
COMMIT permanently saves all changes made within a transaction to the database. ROLLBACK undoes all changes made within a transaction, returning the database to its state before the transaction began, ensuring data consistency.

#### 23. What is a deadlock in databases?
A deadlock occurs when two or more transactions wait indefinitely for each other to release locks. Database systems detect deadlocks and automatically terminate one transaction (deadlock victim) to resolve the situation and allow others to proceed.

#### 24. What are stored procedures and their advantages?
Stored procedures are precompiled SQL code blocks stored in the database server. Advantages include improved performance, enhanced security, code reusability, reduced network traffic, and centralized business logic implementation within the database system.

#### 25. What is the difference between stored procedures and functions?
Stored procedures perform actions and may/may not return values, can have multiple OUT parameters, and allow DML operations. Functions must return single values, are called within expressions, and typically don't perform DML operations.

#### 26. What are triggers and when are they used?
Triggers are special stored procedures that automatically execute in response to specific database events (INSERT, UPDATE, DELETE). They enforce business rules, maintain audit trails, validate data, and perform automatic calculations or updates.

#### 27. What are the different types of triggers?
Trigger types include BEFORE (executes before the event), AFTER (executes after the event), and INSTEAD OF (replaces the event for views). They can respond to INSERT, UPDATE, or DELETE operations on tables.

#### 28. What is SQL injection and how can it be prevented?
SQL injection is a security vulnerability where malicious SQL code is inserted into application queries. Prevention includes using parameterized queries, input validation, stored procedures, escaping special characters, and implementing proper access controls.

#### 29. What are window functions and how do they differ from aggregate functions?
Window functions perform calculations across related rows without collapsing results, maintaining individual row details. Unlike aggregate functions with GROUP BY, window functions provide analytical capabilities while preserving original row structure in results.

#### 30. Explain the OVER() clause in window functions.
The OVER() clause defines the window of rows for calculations, supporting PARTITION BY (grouping), ORDER BY (sorting within partitions), and frame specifications (ROWS/RANGE). It enables running totals, rankings, and analytical computations across row sets.

### Intermediate SQL Interview Questions (31-70)

#### 31. How do you find the second highest salary from an employee table?
Use subquery approach: `SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees)`. Alternative approaches include ROW_NUMBER(), DENSE_RANK(), or LIMIT with OFFSET for different database systems.

#### 32. Write a query to find duplicate records in a table.
`SELECT column_name, COUNT(*) FROM table_name GROUP BY column_name HAVING COUNT(*) > 1`. This identifies values appearing more than once. Use additional columns in GROUP BY for multi-column duplicate detection.

#### 33. How do you remove duplicate rows from a table?
Use ROW_NUMBER() with DELETE: `DELETE FROM table WHERE id NOT IN (SELECT MIN(id) FROM table GROUP BY duplicate_columns)`. Alternative: CREATE new table with DISTINCT values, DROP original, RENAME new table.

#### 34. What is the difference between RANK() and DENSE_RANK()?
RANK() leaves gaps after tied values (1,1,3,4), while DENSE_RANK() doesn't leave gaps (1,1,2,3). ROW_NUMBER() assigns unique numbers even for ties. Choose based on desired ranking behavior for tied values.

#### 35. How do you calculate running totals in SQL?
Use window functions: `SELECT date, amount, SUM(amount) OVER (ORDER BY date ROWS UNBOUNDED PRECEDING) as running_total FROM transactions`. This accumulates values from the first row to current row.

#### 36. What is a CTE (Common Table Expression) and its advantages?
CTE is a temporary named result set defined within query scope using WITH clause. Advantages include improved readability, recursive queries, replacing complex subqueries, and creating modular, reusable query components within single statements.

#### 37. How do you write recursive queries using CTEs?
```sql
WITH RECURSIVE employee_hierarchy AS (
  SELECT id, name, manager_id, 1 as level
  FROM employees WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.manager_id, eh.level + 1
  FROM employees e
  JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy;
```

#### 38. What is the difference between EXISTS and IN operators?
EXISTS checks for subquery result existence, returning TRUE/FALSE regardless of NULL values. IN compares values with subquery results, but NULLs can cause unexpected behavior. EXISTS is often more efficient for correlated subqueries.

#### 39. How do you handle NULL values in SQL?
Use IS NULL/IS NOT NULL for comparisons, COALESCE() for default values, NULLIF() for conditional nulls, and ISNULL()/NVL() for replacements. Remember that NULL comparisons with operators (=, !=) always return unknown/false.

#### 40. What is the difference between COALESCE and ISNULL functions?
COALESCE is ANSI standard, accepts multiple arguments, returns first non-null value. ISNULL is database-specific (SQL Server), accepts only two arguments. COALESCE is more portable across different database management systems.

#### 41. How do you perform pivoting in SQL?
Use PIVOT operator (SQL Server) or conditional aggregation: `SELECT customer, SUM(CASE WHEN product='A' THEN amount END) as product_a, SUM(CASE WHEN product='B' THEN amount END) as product_b FROM sales GROUP BY customer`.

#### 42. What is the difference between CROSS JOIN and FULL OUTER JOIN?
CROSS JOIN produces Cartesian product of all row combinations without join conditions. FULL OUTER JOIN returns all rows from both tables with NULLs for non-matching records, requiring specific join conditions.

#### 43. How do you optimize slow SQL queries?
Analyze execution plans, create appropriate indexes, avoid SELECT *, use WHERE clauses effectively, optimize JOIN conditions, consider query rewriting, update table statistics, and partition large tables for better performance.

#### 44. What are query execution plans and how do you read them?
Execution plans show how database engines execute queries, displaying operation costs, join types, index usage, and data flow. Read from right-to-left, top-to-bottom, identifying expensive operations and optimization opportunities.

#### 45. What is database partitioning and its types?
Partitioning divides large tables into smaller segments for performance and maintenance benefits. Types include horizontal (row-based), vertical (column-based), range, hash, and list partitioning based on partition key values.

#### 46. How do you implement pagination in SQL?
Use OFFSET and FETCH (standard): `SELECT * FROM table ORDER BY id OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY`. Alternative: LIMIT and OFFSET (MySQL, PostgreSQL) or ROW_NUMBER() with filtering.

#### 47. What is the difference between clustered and non-clustered indexes?
Clustered indexes physically order table data, one per table, determine storage organization. Non-clustered indexes create separate structures pointing to data rows, multiple per table, faster for lookups but require additional storage.

#### 48. How do you find the Nth highest value without using LIMIT or TOP?
Use correlated subquery: `SELECT DISTINCT salary FROM employees e1 WHERE N-1 = (SELECT COUNT(DISTINCT salary) FROM employees e2 WHERE e2.salary > e1.salary)`. Alternative: ROW_NUMBER() with filtering.

#### 49. What are database locks and their types?
Locks prevent concurrent access conflicts during transactions. Types include shared (read), exclusive (write), intent locks, and deadlock prevention mechanisms. Lock granularity ranges from row-level to table-level depending on requirements.

#### 50. How do you handle concurrent transactions in databases?
Use appropriate isolation levels, implement proper locking strategies, design deadlock-free transaction orders, keep transactions short, use optimistic concurrency control, and handle deadlock exceptions in application code appropriately.

#### 51. What is the MERGE statement and when do you use it?
MERGE combines INSERT, UPDATE, and DELETE operations in a single statement based on matching conditions. Use for synchronizing data between tables, implementing upsert operations, or data warehouse ETL processes.

#### 52. How do you calculate moving averages in SQL?
Use window functions with frame specifications: `SELECT date, value, AVG(value) OVER (ORDER BY date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) as moving_avg FROM data`. Adjust frame size for different periods.

#### 53. What are materialized views and their benefits?
Materialized views are precomputed, stored query results that improve performance for complex queries. Benefits include faster response times, reduced computation overhead, and improved concurrency for read-heavy analytical workloads.

#### 54. How do you implement audit trails in databases?
Create audit tables, use triggers for automatic logging, implement stored procedures for controlled changes, utilize temporal tables (where available), or use database-specific auditing features to track data modifications.

#### 55. What is data modeling and its importance?
Data modeling defines database structure, relationships, and constraints before implementation. It ensures data integrity, optimizes performance, facilitates communication between stakeholders, and provides blueprints for database development and maintenance.

#### 56. How do you design a many-to-many relationship?
Create a junction/bridge table containing foreign keys from both related tables. Example: Students and Courses require a StudentCourses table with student_id and course_id columns, enabling flexible many-to-many associations.

#### 57. What are database schemas and their purpose?
Schemas are logical containers organizing database objects (tables, views, procedures) with access control and namespace separation. They provide security boundaries, organizational structure, and simplified permission management within database systems.

#### 58. How do you handle time zone data in databases?
Use appropriate data types (TIMESTAMP WITH TIME ZONE), store in UTC with application-level conversion, implement time zone lookup tables, and consider business rules for daylight saving time transitions.

#### 59. What are the differences between OLTP and OLAP systems?
OLTP (Online Transaction Processing) handles real-time transactional workloads with normalized structures. OLAP (Online Analytical Processing) supports analytical queries with denormalized, dimensional structures optimized for reporting and business intelligence.

#### 60. How do you implement database versioning and migration?
Use migration scripts with version control, implement rollback procedures, maintain schema version tracking, automate deployment processes, test migrations in staging environments, and document all database changes systematically.

#### 61. What are temporary tables and their use cases?
Temporary tables store intermediate results during complex operations, visible only to creating session. Use for breaking complex queries, storing calculated results, implementing batch processing, or working with large datasets requiring multiple steps.

#### 62. How do you perform bulk data operations efficiently?
Use bulk insert statements, disable indexes during loading, implement batching strategies, utilize database-specific bulk loading tools, consider parallel processing, and optimize transaction boundaries for large data operations.

#### 63. What is database connection pooling and its benefits?
Connection pooling reuses database connections across multiple requests, reducing connection overhead. Benefits include improved performance, resource conservation, better scalability, and reduced database server load through efficient connection management.

#### 64. How do you implement data validation in databases?
Use CHECK constraints for business rules, NOT NULL for required fields, FOREIGN KEY for referential integrity, UNIQUE for distinct values, and triggers for complex validation logic requiring multiple table access.

#### 65. What are database events and how are they handled?
Database events are notifications triggered by specific conditions (errors, startup, shutdown). Handle through event handlers, stored procedures, or application monitoring systems for automated responses, logging, and system administration tasks.

#### 66. How do you implement hierarchical data in relational databases?
Use adjacency list model (parent_id column), nested set model (left/right values), path enumeration, or closure table approaches. Each has trade-offs for query performance, update complexity, and specific hierarchical operation requirements.

#### 67. What are database synonyms and their purpose?
Synonyms provide alternative names for database objects, enabling location transparency, simplifying complex object names, maintaining backward compatibility during refactoring, and abstracting underlying schema changes from applications.

#### 68. How do you handle large object (LOB) data types?
Store BLOBs/CLOBs separately from regular tables when possible, use streaming for large data transfer, implement proper indexing strategies, consider external storage for very large objects, and optimize application code for LOB handling.

#### 69. What is database replication and its types?
Database replication creates copies of databases across multiple servers. Types include master-slave (one-way), master-master (bidirectional), snapshot, transactional, and merge replication, each serving different availability and scalability requirements.

#### 70. How do you monitor and troubleshoot database performance?
Use performance monitoring tools, analyze execution plans, monitor wait statistics, track resource utilization, implement logging for slow queries, review index usage, and establish baseline performance metrics for comparison.

### Advanced SQL Interview Questions (71-110)

#### 71. How do you implement database sharding strategies?
Implement horizontal partitioning across multiple databases using range-based, hash-based, or directory-based sharding. Consider data distribution, cross-shard queries, rebalancing strategies, and application-level routing for optimal performance and scalability.

#### 72. What are the challenges with distributed databases?
Challenges include maintaining ACID properties across nodes, handling network partitions, ensuring consistency vs availability trade-offs, managing distributed transactions, coordinating schema changes, and implementing effective monitoring across multiple systems.

#### 73. How do you implement eventual consistency in databases?
Use asynchronous replication, implement conflict resolution strategies, design idempotent operations, utilize vector clocks for causality tracking, and establish acceptable consistency windows based on business requirements for distributed systems.

#### 74. What is the CAP theorem and its implications?
CAP theorem states distributed systems can guarantee only two of: Consistency, Availability, and Partition tolerance. Implications include choosing appropriate consistency models, designing for network failures, and balancing trade-offs based on application requirements.

#### 75. How do you handle database migrations in production environments?
Implement blue-green deployments, use backward-compatible changes, create rollback procedures, perform migrations during low-traffic periods, use feature flags, maintain data consistency checks, and coordinate with application deployment strategies.

#### 76. What are database design patterns and anti-patterns?
Common patterns include repository, unit of work, and active record. Anti-patterns include god tables, lack of constraints, inappropriate denormalization, missing indexes, and circular dependencies. Follow established design principles for maintainable systems.

#### 77. How do you implement database caching strategies?
Use query result caching, implement application-level caching (Redis, Memcached), utilize database buffer pools, implement cache invalidation strategies, consider cache-aside vs write-through patterns, and monitor cache hit ratios for optimization.

#### 78. What are microservices database patterns?
Database per service maintains data isolation, saga pattern handles distributed transactions, event sourcing captures state changes, CQRS separates read/write models, and shared databases enable legacy integration while maintaining service boundaries.

#### 79. How do you implement data encryption in databases?
Use transparent data encryption (TDE) for at-rest encryption, implement column-level encryption for sensitive data, use SSL/TLS for data in transit, manage encryption keys securely, and consider application-level encryption for enhanced security.

#### 80. What is database forensics and incident response?
Database forensics involves investigating security incidents, analyzing transaction logs, identifying unauthorized access patterns, preserving evidence integrity, and implementing recovery procedures while maintaining audit trails for compliance and legal requirements.

#### 81. How do you implement database disaster recovery?
Create comprehensive backup strategies, implement geographic replication, establish recovery time objectives (RTO) and recovery point objectives (RPO), test recovery procedures regularly, and maintain disaster recovery documentation and procedures.

#### 82. What are database security best practices?
Implement principle of least privilege, use role-based access control, enable audit logging, encrypt sensitive data, apply security patches regularly, use strong authentication methods, and conduct regular security assessments.

#### 83. How do you handle database version control?
Use database migration tools, maintain schema version tracking, implement code review for database changes, utilize branching strategies for development, and coordinate database versions with application releases.

#### 84. What are the considerations for database cloud migration?
Evaluate cloud provider compatibility, assess network latency impacts, implement proper security controls, consider costs and licensing changes, plan data transfer strategies, and ensure compliance requirements are met.

#### 85. How do you implement database monitoring and alerting?
Set up performance metrics collection, establish alert thresholds for key indicators, implement automated incident response, use dashboards for visualization, monitor both system and business metrics, and integrate with incident management systems.

#### 86. What are database compliance and regulatory requirements?
Understand GDPR, HIPAA, SOX, PCI-DSS requirements, implement data retention policies, ensure audit trail completeness, establish data privacy controls, maintain proper documentation, and conduct regular compliance assessments.

#### 87. How do you optimize database storage and capacity planning?
Monitor storage growth patterns, implement data archiving strategies, optimize data types and compression, plan for peak capacity needs, consider partitioning for large tables, and establish storage lifecycle management policies.

#### 88. What are the considerations for database technology selection?
Evaluate ACID requirements, assess scalability needs, consider consistency models, analyze query patterns, evaluate operational complexity, assess vendor support, consider total cost of ownership, and align with team expertise.

#### 89. How do you implement database testing strategies?
Create unit tests for stored procedures, implement integration tests for data flows, perform load testing for performance validation, use test data management tools, implement database mocking for development, and maintain test environments.

#### 90. What are emerging trends in database technologies?
NewSQL databases combine ACID properties with NoSQL scalability, serverless databases reduce operational overhead, graph databases handle complex relationships, time-series databases optimize temporal data, and AI/ML integration enables intelligent optimization.

#### 91. How do you handle database refactoring and schema evolution?
Plan backward-compatible changes, implement gradual migrations, use database refactoring tools, maintain comprehensive testing, coordinate with application teams, document schema changes, and establish rollback procedures for safety.

#### 92. What are the principles of database-driven development?
Design databases first approach, ensure referential integrity, implement proper normalization, use stored procedures for business logic, maintain consistency between database and application models, and optimize for data access patterns.

#### 93. How do you implement database batch processing?
Design efficient batch job scheduling, implement proper error handling and restart capabilities, optimize for bulk operations, manage resource utilization, implement progress tracking, and ensure data consistency throughout batch operations.

#### 94. What are database analytics and business intelligence considerations?
Design dimensional models for reporting, implement ETL processes, create data marts for specific domains, ensure data quality and consistency, optimize for analytical queries, and provide self-service analytics capabilities.

#### 95. How do you handle database legacy system integration?
Assess existing system constraints, design integration patterns, implement data synchronization strategies, plan gradual migration paths, maintain backward compatibility, and ensure data consistency across systems.

#### 96. What are the considerations for database containerization?
Evaluate stateful application challenges, implement persistent volume strategies, consider networking requirements, plan for scaling and orchestration, ensure data persistence across container lifecycles, and manage configuration and secrets securely.

#### 97. How do you implement database DevOps practices?
Integrate database changes into CI/CD pipelines, implement infrastructure as code, automate testing and deployment, maintain environment consistency, use configuration management tools, and establish monitoring and feedback loops.

#### 98. What are the challenges with polyglot persistence?
Managing multiple database technologies increases complexity, requiring diverse expertise, consistent monitoring tools, and standardized operational procedures. Consider data consistency across systems, transaction boundaries, and integration complexity.

#### 99. How do you design databases for high availability?
Implement redundancy at multiple levels, use clustering and replication, design for graceful degradation, eliminate single points of failure, implement automatic failover mechanisms, and maintain geographic distribution for disaster recovery.

#### 100. What are the future directions in database technology?
Autonomous databases with AI-driven optimization, quantum-resistant security measures, enhanced integration with machine learning workflows, improved cloud-native capabilities, edge computing database solutions, and blockchain integration for immutable audit trails.

#### 101. How do you implement database-as-a-service (DBaaS) strategies?
Evaluate cloud provider offerings, implement proper security controls, optimize costs through right-sizing, establish service level agreements, plan for vendor lock-in considerations, and maintain operational visibility and control.

#### 102. What are the considerations for real-time database systems?
Design for low latency requirements, implement proper caching strategies, optimize network configurations, ensure consistent performance under load, plan for scaling requirements, and maintain data freshness requirements.

#### 103. How do you handle database performance tuning at scale?
Implement systematic performance monitoring, use automated tuning tools where available, optimize based on actual usage patterns, maintain performance baselines, implement capacity planning processes, and coordinate tuning efforts across teams.

#### 104. What are the principles of database security architecture?
Implement defense in depth, use zero-trust principles, maintain proper access controls, encrypt data at all layers, implement comprehensive audit logging, plan for incident response, and regularly assess security postures.

#### 105. How do you implement database change management?
Establish formal change approval processes, maintain comprehensive documentation, implement automated testing, coordinate with stakeholders, maintain rollback capabilities, track change impacts, and ensure proper communication throughout the organization.

#### 106. What are the considerations for database multi-tenancy?
Design for tenant isolation, implement proper resource allocation, maintain security boundaries, optimize for shared resources, plan for tenant-specific customizations, ensure scalability across tenants, and maintain operational simplicity.

#### 107. How do you handle database performance optimization for analytical workloads?
Implement columnar storage where appropriate, design for analytical query patterns, use materialized views and summary tables, optimize for parallel processing, implement proper indexing strategies, and consider specialized analytical databases.

#### 108. What are the challenges with database modernization initiatives?
Balancing legacy system constraints with modern requirements, managing risk during transitions, ensuring business continuity, coordinating across multiple teams, maintaining data integrity, and optimizing costs throughout the modernization process.

#### 109. How do you implement database observability and monitoring?
Establish comprehensive metrics collection, implement distributed tracing for complex operations, maintain proper alerting thresholds, create meaningful dashboards, integrate with incident management systems, and ensure monitoring covers all system layers.

#### 110. What are the best practices for database documentation and knowledge management?
Maintain up-to-date schema documentation, document business rules and constraints, create runbooks for operational procedures, establish knowledge sharing practices, maintain disaster recovery documentation, and ensure documentation is accessible and searchable.

---

## Conclusion

This comprehensive SQL handbook covers fundamental concepts through advanced topics, providing practical examples and detailed explanations for each area. Whether you're preparing for interviews, learning SQL basics, or advancing your database skills, these concepts form the foundation of professional SQL development.

Key areas to focus on for interview preparation:
- Master basic SQL operations and syntax
- Understand join types and when to use each
- Practice writing complex queries with subqueries and CTEs
- Learn performance optimization techniques
- Understand database design principles
- Study transaction management and ACID properties
- Practice problem-solving with real-world scenarios

Remember that SQL proficiency comes through practice. Work with real datasets, experiment with different query approaches, and understand the reasoning behind database design decisions. This knowledge will serve you well in technical interviews and professional database development.

For continued learning, practice with online SQL platforms, work on personal projects, and stay updated with database technology trends and best practices. The field of database management continues to evolve, making continuous learning essential for career growth.
