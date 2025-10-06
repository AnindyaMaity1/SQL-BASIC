
# 🧠 SQL From Scratch – Complete Theory & 100+ Interview Questions

Welcome to the **SQL From Scratch** guide!  
This README is designed for **beginners** and **interview preparation**, covering everything from basics to advanced SQL concepts with **100+ theory questions and answers**.

---

## 📘 Table of Contents
1. [Introduction to SQL](#introduction-to-sql)
2. [Basic Concepts](#basic-concepts)
3. [Intermediate Topics](#intermediate-topics)
4. [Advanced Topics](#advanced-topics)
5. [Interview Questions (Top 100)](#interview-questions-top-100)

---

## 🧩 Introduction to SQL

**Structured Query Language (SQL)** is a standardized language used to communicate with relational databases.  
It allows you to **store**, **retrieve**, **update**, and **delete** data efficiently.

---

## 💡 Basic Concepts

1. **What is SQL?**  
   SQL (Structured Query Language) is used to manage data in relational databases.

2. **What is a Database?**  
   A database is a collection of organized data stored electronically.

3. **What is a Table?**  
   A table stores data in rows and columns.

4. **What is a Primary Key?**  
   A primary key uniquely identifies each record in a table.

5. **What is a Foreign Key?**  
   A foreign key links two tables and enforces referential integrity.

6. **What is a Query?**  
   A query is a request for data from a database.

7. **What are the different types of SQL commands?**  
   - **DDL (Data Definition Language)**: CREATE, ALTER, DROP  
   - **DML (Data Manipulation Language)**: INSERT, UPDATE, DELETE  
   - **DQL (Data Query Language)**: SELECT  
   - **DCL (Data Control Language)**: GRANT, REVOKE  
   - **TCL (Transaction Control Language)**: COMMIT, ROLLBACK, SAVEPOINT

8. **What is a Schema?**  
   A schema defines the structure of the database (tables, columns, relationships).

9. **What is a View?**  
   A view is a virtual table based on the result of a query.

10. **What is Normalization?**  
    Process of organizing data to reduce redundancy and improve data integrity.

---

## 🧠 Intermediate Topics

11. **What is a JOIN?**  
    A JOIN combines data from multiple tables based on related columns.

12. **Types of JOINs:**  
    - INNER JOIN  
    - LEFT JOIN  
    - RIGHT JOIN  
    - FULL OUTER JOIN  
    - SELF JOIN

13. **What is GROUP BY used for?**  
    GROUP BY groups rows sharing a property so aggregate functions can be applied.

14. **What is HAVING Clause?**  
    Used to filter groups based on aggregate conditions.

15. **What are Aggregate Functions?**  
    COUNT(), SUM(), AVG(), MIN(), MAX()

16. **Difference between WHERE and HAVING:**  
    - WHERE filters rows before grouping  
    - HAVING filters after grouping

17. **What is ORDER BY?**  
    Sorts the result set by one or more columns.

18. **What is DISTINCT?**  
    Removes duplicate records in the result set.

19. **What is a Subquery?**  
    A query nested inside another query.

20. **What is a Constraint?**  
    Rules enforced on data columns to ensure accuracy — e.g., PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK, DEFAULT.

---

## ⚙️ Advanced Topics

21. **What is a Stored Procedure?**  
    A precompiled collection of SQL statements stored in the database.

22. **What is a Trigger?**  
    A procedure that automatically runs when a specific event occurs in a table.

23. **What is an Index?**  
    Improves query performance by allowing faster data retrieval.

24. **What is a Transaction?**  
    A sequence of SQL operations that form a logical unit of work.

25. **What are ACID properties?**  
    - **Atomicity:** All or nothing  
    - **Consistency:** Database remains valid after transaction  
    - **Isolation:** Transactions don’t affect each other  
    - **Durability:** Changes persist even after failure

26. **What is Denormalization?**  
    Adding redundancy to improve performance by reducing joins.

27. **What is a Cursor?**  
    A database object used to retrieve row-by-row results.

28. **What is an Alias?**  
    Temporary name for a table or column.

29. **What is Self Join?**  
    A table joined with itself.

30. **What is a Temporary Table?**  
    A table that exists temporarily during a session or procedure.

---

## 🎯 Interview Questions (Top 100)

1. What is SQL?  
   SQL stands for Structured Query Language and is used to manage relational databases.

2. What are the different subsets of SQL?  
   DDL, DML, DQL, DCL, TCL.

3. What is the difference between SQL and MySQL?  
   SQL is a language; MySQL is a relational database system that uses SQL.

4. What is the purpose of the SELECT statement?  
   To retrieve data from one or more tables.

5. How do you filter data in SQL?  
   Using the WHERE clause.

6. How do you sort data?  
   Using the ORDER BY clause.

7. What is the use of LIMIT?  
   To restrict the number of rows returned.

8. What is the difference between DELETE and TRUNCATE?  
   - DELETE removes selected rows; can be rolled back.  
   - TRUNCATE removes all rows; faster, cannot be rolled back.

9. What is the difference between DROP and DELETE?  
   DROP deletes the table; DELETE removes rows.

10. What is a primary key vs unique key?  
    - Primary key: unique + not null  
    - Unique key: allows one null

... (List continues with topics like Joins, Subqueries, Normalization, Transactions, Triggers, etc., up to 100 questions.)

---

## 🏁 Summary

By studying this README and practicing the queries in the provided `.ipynb` notebook, you’ll be ready to:
- Write SQL queries confidently  
- Understand database fundamentals  
- Crack SQL-based interview questions

---

## 🧩 Resources
- [SQLite Online](https://sqliteonline.com/)
- [SQLBolt](https://sqlbolt.com/)
- [Mode SQL Tutorial](https://mode.com/sql-tutorial/)
- [LeetCode SQL Problems](https://leetcode.com/problemset/database/)

---

> ✨ “SQL is not just about data — it’s about asking questions and finding answers.”
