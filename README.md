# Renton Technical College CSI-234

<br />

![RTC Logo](Images/logo.jpg)

This tutorial is part of CSI-234 at Renton Technical College.

## Part 1 - Setup

1. Clone this repository to your local machine using either the terminal or GitHub Desktop.
2. Be sure to save all requested SQL Scripts in your local repo before pushing changes.
3. Before you can complete this assignment you MUST have SQL Server running with the AdventureWorksLT database.
4. We will start this assignment as a group in class. Refer to the lecture videos for reference.
5. If you do not have these steps completed come to office hours or make an appointment with myself or a TA to get this running.
6. https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver16&tabs=ssms
7. Open SSMS and connect to your SQL Server instance.
8. In SSMS, create a new query window by clicking on "New Query" button.
9. Verify your connection to AdventureWorksLT by running:
   ```sql
   USE AdventureWorksLT;
   ```

## Part 2 - Basic SELECT Statements

1. Let's start with a simple SELECT statement to retrieve all columns from a table:
   ```sql
   SELECT * 
   FROM SalesLT.Customer;
   ```
   Run this query and observe the results.

2. Now, let's select specific columns:
   ```sql
   SELECT CustomerID, FirstName, LastName, CompanyName
   FROM SalesLT.Customer;
   ```
   Run this query and note the difference in the output.

3. We can use column aliases to rename columns in the output:
   ```sql
   SELECT 
       CustomerID,
       FirstName + ' ' + LastName AS FullName,
       CompanyName AS Company
   FROM SalesLT.Customer;
   ```
   Execute this query and observe how the column names change in the result set.

4. SELECT statements can also include calculations:
   ```sql
   SELECT 
       ProductID,
       Name,
       ListPrice,
       StandardCost,
       ListPrice - StandardCost AS Profit
   FROM SalesLT.Product;
   ```
   Run this query to see how we can perform calculations in SELECT statements.

## Part 2 - Practice Exercises

Complete the following exercises:

1. Write a query to retrieve the product ID, name, and list price for all products in the SalesLT.Product table.

2. Create a query to list the sales order ID, order date, and total due for all orders in the SalesLT.SalesOrderHeader table.

3. Develop a query to display the customer ID, full name (first name + last name), and email address for all customers, using appropriate column aliases.

### Submitting Your Work

After completing the practice exercises:

1. Save your SQL queries in a file named Part2.sql` in your local repository.
2. Open your terminal in the repository directory.
3. Type `git add .` to stage your new file.
4. Type `git commit -m "Part 2 Complete"`.
5. Type `git push` to submit your work.

## Part 3 - SELECT Statements

Building on the basic SELECT statements we covered in Part 1, let's explore some more advanced features of SELECT queries:

### Using the DISTINCT Keyword

The DISTINCT keyword is used to return only unique values in the result set. This is useful when you want to eliminate duplicate rows from your query results.

Example:
```sql
SELECT DISTINCT City, StateProvince
FROM SalesLT.Address;
```

This query will return a list of unique city and state combinations from the Address table. Run this query and observe how it eliminates duplicate entries.

### Filtering Results with WHERE Clause

The WHERE clause allows you to filter the rows returned by your query based on specified conditions.

Example:
```sql
SELECT ProductID, Name, Color, Size
FROM SalesLT.Product
WHERE Color = 'Red' AND Size = 'M';
```

This query will return only products that are both red and medium-sized. Try modifying the conditions to filter for different colors or sizes.

You can use various operators in the WHERE clause:
- Comparison operators: =, <>, <, >, <=, >=
- Logical operators: AND, OR, NOT
- BETWEEN, IN, LIKE, IS NULL

Example with LIKE:
```sql
SELECT ProductID, Name
FROM SalesLT.Product
WHERE Name LIKE 'Mountain%';
```

This will return all products whose names start with "Mountain".

### Sorting Results with ORDER BY

The ORDER BY clause is used to sort the result set based on one or more columns.

Example:
```sql
SELECT ProductID, Name, ListPrice
FROM SalesLT.Product
ORDER BY ListPrice DESC;
```

This query will return products sorted by price from highest to lowest. You can use ASC for ascending order (which is the default if not specified).

You can also sort by multiple columns:

```sql
SELECT ProductID, Name, Color, ListPrice
FROM SalesLT.Product
ORDER BY Color ASC, ListPrice DESC;
```

This will sort the products first by Color (alphabetically), and then by ListPrice (highest to lowest) within each color group.

## Part 3 - Practice Exercises

Complete the following exercises:

1. Write a query to find all unique product colors in the SalesLT.Product table.

2. Create a query to list all products with a ListPrice between $1000 and $2000.

3. Develop a query to display the top 10 most expensive products in the SalesLT.Product table.

4. Write a query to find all customers whose last name starts with 'S' and order them alphabetically by their first name.

### Submitting Your Work

After completing the practice exercises:

1. Save your SQL queries in a file named `Part3.sql`.
2. Open your git bash terminal in the project directory.
3. Type `git add .` to stage your new file.
4. Type `git commit -m "Completed Part 3"`.
5. Type `git push` to submit your work.


## Part 4 - JOIN Operations and Aggregate Functions

In this section, we'll explore JOIN operations to combine data from multiple tables and Aggregate functions to perform calculations on sets of rows.

### JOIN Operations

JOIN operations allow you to combine rows from two or more tables based on a related column between them. The general syntax for a JOIN is:

```sql
SELECT columns
FROM table1
JOIN_TYPE table2
ON table1.column = table2.column;
```

- The `ON` clause specifies the condition for joining the tables.
- We use aliases with joins to make queries more readable and to clearly distinguish column names when multiple tables have columns with the same name.

1. INNER JOIN: Returns only the rows where there is a match in both tables.

   Example:
   ```sql
   SELECT c.CustomerID, c.FirstName, c.LastName, o.SalesOrderID, o.OrderDate
   FROM SalesLT.Customer AS c
   INNER JOIN SalesLT.SalesOrderHeader AS o ON c.CustomerID = o.CustomerID;
   ```
   This query combines customer information with their orders. The `AS` keyword for aliases is optional.

2. LEFT JOIN: Returns all rows from the left table and matched rows from the right table.

   Example:
   ```sql
   SELECT c.CustomerID, c.FirstName, c.LastName, o.SalesOrderID, o.OrderDate
   FROM SalesLT.Customer c
   LEFT JOIN SalesLT.SalesOrderHeader o ON c.CustomerID = o.CustomerID;
   ```
   This query returns all customers, even those without orders.

3. RIGHT JOIN: Returns all rows from the right table and matched rows from the left table.

   Example:
   ```sql
   SELECT p.ProductID, p.Name, c.Name AS CategoryName
   FROM SalesLT.Product p
   RIGHT JOIN SalesLT.ProductCategory c ON p.ProductCategoryID = c.ProductCategoryID;
   ```
   This query returns all product categories, even those without products.

4. Multiple JOINs: You can join more than two tables in a single query.

   Example:
   ```sql
   SELECT c.CustomerID, c.FirstName, c.LastName, o.SalesOrderID, od.ProductID, p.Name
   FROM SalesLT.Customer c
   INNER JOIN SalesLT.SalesOrderHeader o ON c.CustomerID = o.CustomerID
   INNER JOIN SalesLT.SalesOrderDetail od ON o.SalesOrderID = od.SalesOrderID
   INNER JOIN SalesLT.Product p ON od.ProductID = p.ProductID;
   ```
   This query combines customer, order, order detail, and product information.

### Aggregate Functions and GROUP BY

Aggregate functions perform calculations on a set of rows and return a single result.

1. COUNT: Counts the number of rows that match the specified criteria.

   Example:
   ```sql
   SELECT COUNT(*) AS TotalCustomers
   FROM SalesLT.Customer;
   ```

2. SUM: Calculates the sum of a set of values.

   Example:
   ```sql
   SELECT SUM(TotalDue) AS TotalSales
   FROM SalesLT.SalesOrderHeader;
   ```

3. AVG: Calculates the average of a set of values.

   Example:
   ```sql
   SELECT AVG(ListPrice) AS AveragePrice
   FROM SalesLT.Product;
   ```

4. MAX and MIN: Return the maximum and minimum values in a set.

   Example:
   ```sql
   SELECT 
       MAX(ListPrice) AS HighestPrice,
       MIN(ListPrice) AS LowestPrice
   FROM SalesLT.Product;
   ```

5. GROUP BY and HAVING: GROUP BY is used with aggregate functions to group the result set by one or more columns. HAVING is used to filter the results of GROUP BY based on a condition.

   Example:
   ```sql
   SELECT ProductCategoryID, AVG(ListPrice) AS AveragePrice
   FROM SalesLT.Product
   GROUP BY ProductCategoryID
   HAVING AVG(ListPrice) > 500;
   ```

   This query groups products by category and shows the average price, but only for categories with an average price over $500.

   Note: We use HAVING instead of WHERE when we need to filter on the result of an aggregate function. WHERE is applied to individual rows before they are grouped, while HAVING is applied after grouping.

   Syntax:
   ```sql
   SELECT column1, aggregate_function(column2)
   FROM table
   GROUP BY column1
   HAVING condition_using_aggregate_function;
   ```

## Part 4 - Practice Exercises

Complete the following exercises:

1. Write a query to display the customer's full name (FirstName + LastName) and the total number of orders they have placed. Include all customers, even if they haven't placed any orders.

2. Create a query to show the product category name and the average list price of products in that category. Only include categories with an average list price greater than $500.

3. Develop a query to find the top 5 customers who have spent the most money. Display the customer's full name and their total spending.

4. Write a query to list all product categories along with the number of products in each category. Sort the results by the number of products in descending order.


### Submitting Your Work

After completing the practice exercises:

1. Save your SQL queries in a file named `Part4.sql`.
2. Open your git bash terminal in the project directory.
3. Type `git add .` to stage your new file.
4. Type `git commit -m "Completed Part 4"`.
5. Type `git push` to submit your work.


## Part 5 - Data Modification and Subqueries

In this section, we'll cover how to modify data using INSERT, UPDATE, and DELETE statements, as well as how to use subqueries in your SQL statements.

### Data Modification

1. INSERT: Adds new rows to a table.

   Syntax:
   ```sql
   INSERT INTO table_name (column1, column2, ...)
   VALUES (value1, value2, ...);
   ```

   Example:
   ```sql
   INSERT INTO SalesLT.Customer (FirstName, LastName, CompanyName, EmailAddress)
   VALUES ('John', 'Doe', 'ABC Company', 'john.doe@example.com');
   ```

2. UPDATE: Modifies existing data in a table.

   Syntax:
   ```sql
   UPDATE table_name
   SET column1 = value1, column2 = value2, ...
   WHERE condition;
   ```

   Example:
   ```sql
   UPDATE SalesLT.Customer
   SET CompanyName = 'XYZ Corporation'
   WHERE CustomerID = 1;
   ```

3. DELETE: Removes rows from a table.

   Syntax:
   ```sql
   DELETE FROM table_name
   WHERE condition;
   ```

   Example:
   ```sql
   DELETE FROM SalesLT.Customer
   WHERE CustomerID = 1;
   ```

### Subqueries

A subquery is a query nested inside another query. It can be used in various parts of SQL statements.

1. Subquery in WHERE clause:

   Example:
   ```sql
   SELECT ProductID, Name, ListPrice
   FROM SalesLT.Product
   WHERE ListPrice > (SELECT AVG(ListPrice) FROM SalesLT.Product);
   ```
   This query returns products with a price higher than the average price.

2. Subquery in FROM clause:

   Example:
   ```sql
   SELECT CategoryName, AveragePrice
   FROM (
       SELECT c.Name AS CategoryName, AVG(p.ListPrice) AS AveragePrice
       FROM SalesLT.ProductCategory c
       JOIN SalesLT.Product p ON c.ProductCategoryID = p.ProductCategoryID
       GROUP BY c.ProductCategoryID, c.Name
   ) AS CategoryAverages
   WHERE AveragePrice > 1000;
   ```
   This query uses a subquery in the FROM clause to calculate average prices per category, then filters the results.

3. Correlated Subquery:

   Example:
   ```sql
   SELECT ProductID, Name, ListPrice
   FROM SalesLT.Product p1
   WHERE ListPrice > (
       SELECT AVG(ListPrice)
       FROM SalesLT.Product p2
       WHERE p2.ProductCategoryID = p1.ProductCategoryID
   );
   ```
   This query finds products that are more expensive than the average price in their respective categories.

## Part 5 - Practice Exercises

Complete the following exercises:

1. Write an INSERT statement to add a new customer to the SalesLT.Customer table. Include values for FirstName, LastName, CompanyName, and EmailAddress.

2. Create an UPDATE statement to change the CompanyName for all customers from 'Bike World' to 'Bicycle Universe'.

3. Write a SELECT statement to show all products that have never been ordered. (Hint: Use a subquery with NOT IN to find ProductIDs not in SalesLT.SalesOrderDetail)

4. Develop a query using a subquery in the WHERE clause to find all products that are more expensive than the average price of products in their category.


### Submitting Your Work

After completing the practice exercises:

1. Save your SQL queries in a file named `Part5.sql`.
2. Open your git bash terminal in the project directory.
3. Type `git add .` to stage your new file.
4. Type `git commit -m "Completed Part 5"`.
5. Type `git push` to submit your work.

If you have any questions about this assignment, please reach out to your instructor or the TA for this course.

Feel free to message your instructor or the TA on Canvas if you need any clarification.


If you have any questions about this assignment, please reach out to your instructor or the TA for this course.

Feel free to message your instructor or the TA on Canvas if you need any clarification.
