<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# in readme. code

Here's the MS SQL Script Cheatsheet formatted for a README.md file:

```markdown
# MS SQL Script Cheatsheet: Comprehensive Guide with Examples

## Database Operations

### Creating and Managing Databases
```

-- Create a new database
CREATE DATABASE CompanyDB;

-- Use a specific database
USE CompanyDB;

-- Drop a database
DROP DATABASE CompanyDB;

-- Show all databases
SELECT name FROM sys.databases;

```

## Table Operations

### Creating Tables

```

-- Create a table with various data types
CREATE TABLE Employees (
EmployeeID INT PRIMARY KEY IDENTITY(1,1),
FirstName VARCHAR(50) NOT NULL,
LastName VARCHAR(50) NOT NULL,
Email VARCHAR(100) UNIQUE,
Department VARCHAR(50),
Salary DECIMAL(10,2),
HireDate DATE DEFAULT GETDATE(),
IsActive BIT DEFAULT 1
);

```

### Modifying Tables

```

-- Add a new column
ALTER TABLE Employees ADD Phone VARCHAR(15);

-- Modify column data type
ALTER TABLE Employees ALTER COLUMN Phone VARCHAR(20);

-- Drop a column
ALTER TABLE Employees DROP COLUMN Phone;

-- Drop a table
DROP TABLE Employees;

```

## Data Manipulation

### Inserting Data

```

-- Insert single record
INSERT INTO Employees (FirstName, LastName, Email, Department, Salary)
VALUES ('John', 'Doe', 'john.doe@company.com', 'IT', 65000.00);

-- Insert multiple records
INSERT INTO Employees (FirstName, LastName, Email, Department, Salary)
VALUES
('Jane', 'Smith', 'jane.smith@company.com', 'HR', 55000.00),
('Bob', 'Johnson', 'bob.johnson@company.com', 'Finance', 60000.00),
('Alice', 'Williams', 'alice.williams@company.com', 'IT', 70000.00);

```

### Updating Data

```

-- Update specific records
UPDATE Employees
SET Salary = 68000.00
WHERE EmployeeID = 1;

-- Update multiple columns
UPDATE Employees
SET Department = 'Engineering', Salary = Salary \* 1.1
WHERE Department = 'IT';

-- Update with JOIN
UPDATE e
SET e.Salary = e.Salary \* 1.05
FROM Employees e
INNER JOIN Departments d ON e.Department = d.DepartmentName
WHERE d.Budget > 100000;

```

### Deleting Data

```

-- Delete specific records
DELETE FROM Employees WHERE EmployeeID = 5;

-- Delete with conditions
DELETE FROM Employees WHERE Salary < 40000;

-- Delete all records (but keep table structure)
DELETE FROM Employees;

-- Truncate table (faster than DELETE)
TRUNCATE TABLE Employees;

```

## Querying Data

### Basic SELECT Statements

```

-- Select all columns
SELECT \* FROM Employees;

-- Select specific columns
SELECT FirstName, LastName, Salary FROM Employees;

-- Select with alias
SELECT FirstName + ' ' + LastName AS FullName,
Salary AS AnnualSalary
FROM Employees;

-- Select distinct values
SELECT DISTINCT Department FROM Employees;

```

### Filtering with WHERE

```

-- Basic WHERE conditions
SELECT \* FROM Employees WHERE Department = 'IT';

-- Multiple conditions with AND/OR
SELECT \* FROM Employees
WHERE Department = 'IT' AND Salary > 60000;

SELECT \* FROM Employees
WHERE Department = 'HR' OR Department = 'Finance';

-- Using NOT
SELECT \* FROM Employees WHERE NOT Department = 'IT';

-- Pattern matching with LIKE
SELECT _ FROM Employees WHERE FirstName LIKE 'J%';
SELECT _ FROM Employees WHERE Email LIKE '%@company.com';

-- Range filtering
SELECT \* FROM Employees WHERE Salary BETWEEN 50000 AND 70000;

-- List filtering
SELECT \* FROM Employees WHERE Department IN ('IT', 'HR', 'Finance');

-- NULL checking
SELECT _ FROM Employees WHERE Email IS NULL;
SELECT _ FROM Employees WHERE Email IS NOT NULL;

```

### Sorting and Limiting

```

-- Order by single column
SELECT \* FROM Employees ORDER BY Salary DESC;

-- Order by multiple columns
SELECT \* FROM Employees ORDER BY Department ASC, Salary DESC;

-- Limit results (SQL Server uses TOP)
SELECT TOP 5 \* FROM Employees ORDER BY Salary DESC;

-- Pagination with OFFSET and FETCH
SELECT \* FROM Employees
ORDER BY EmployeeID
OFFSET 10 ROWS
FETCH NEXT 5 ROWS ONLY;

```

## Aggregate Functions and Grouping

### Common Aggregate Functions

```

-- Count records
SELECT COUNT(\*) AS TotalEmployees FROM Employees;
SELECT COUNT(Email) AS EmailCount FROM Employees; -- Excludes NULLs

-- Sum and average
SELECT SUM(Salary) AS TotalSalaries FROM Employees;
SELECT AVG(Salary) AS AverageSalary FROM Employees;

-- Min and max
SELECT MIN(Salary) AS MinSalary, MAX(Salary) AS MaxSalary FROM Employees;

-- Group by department
SELECT Department,
COUNT(\*) AS EmployeeCount,
AVG(Salary) AS AvgSalary,
SUM(Salary) AS TotalSalary
FROM Employees
GROUP BY Department;

-- Having clause (filter groups)
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > 60000;

```

## Joins

### Inner Join

```

-- Inner join
SELECT e.FirstName, e.LastName, d.DepartmentName, d.Location
FROM Employees e
INNER JOIN Departments d ON e.Department = d.DepartmentName;

```

### Outer Joins

```

-- Left join
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
LEFT JOIN Departments d ON e.Department = d.DepartmentName;

-- Right join
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
RIGHT JOIN Departments d ON e.Department = d.DepartmentName;

-- Full outer join
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
FULL OUTER JOIN Departments d ON e.Department = d.DepartmentName;

```

### Self Join

```

-- Self join (find employees and their managers)
SELECT e1.FirstName + ' ' + e1.LastName AS Employee,
e2.FirstName + ' ' + e2.LastName AS Manager
FROM Employees e1
LEFT JOIN Employees e2 ON e1.ManagerID = e2.EmployeeID;

```

## String Functions

```

-- Concatenation
SELECT FirstName + ' ' + LastName AS FullName FROM Employees;
SELECT CONCAT(FirstName, ' ', LastName) AS FullName FROM Employees;

-- String manipulation
SELECT UPPER(FirstName) AS UpperName FROM Employees;
SELECT LOWER(LastName) AS LowerName FROM Employees;
SELECT LEN(FirstName) AS NameLength FROM Employees;
SELECT SUBSTRING(FirstName, 1, 3) AS ShortName FROM Employees;
SELECT LEFT(FirstName, 2) AS FirstTwo FROM Employees;
SELECT RIGHT(LastName, 3) AS LastThree FROM Employees;

-- String replacement
SELECT REPLACE(Email, '@company.com', '@newcompany.com') AS NewEmail
FROM Employees;

-- Trimming whitespace
SELECT LTRIM(RTRIM(FirstName)) AS CleanName FROM Employees;

```

## Date and Time Functions

```

-- Current date and time
SELECT GETDATE() AS CurrentDateTime;
SELECT GETUTCDATE() AS CurrentUTCDateTime;

-- Date parts
SELECT YEAR(HireDate) AS HireYear FROM Employees;
SELECT MONTH(HireDate) AS HireMonth FROM Employees;
SELECT DAY(HireDate) AS HireDay FROM Employees;
SELECT DATEPART(QUARTER, HireDate) AS HireQuarter FROM Employees;

-- Date arithmetic
SELECT DATEADD(YEAR, 1, HireDate) AS OneYearLater FROM Employees;
SELECT DATEDIFF(YEAR, HireDate, GETDATE()) AS YearsEmployed FROM Employees;

-- Date formatting
SELECT FORMAT(HireDate, 'yyyy-MM-dd') AS FormattedDate FROM Employees;
SELECT FORMAT(HireDate, 'MMMM dd, yyyy') AS LongDate FROM Employees;

```

## Conditional Logic

### CASE Statements

```

-- Simple CASE
SELECT FirstName, LastName, Salary,
CASE
WHEN Salary > 70000 THEN 'High'
WHEN Salary > 50000 THEN 'Medium'
ELSE 'Low'
END AS SalaryLevel
FROM Employees;

-- CASE with multiple conditions
SELECT FirstName, LastName, Department,
CASE Department
WHEN 'IT' THEN 'Technology'
WHEN 'HR' THEN 'Human Resources'
WHEN 'Finance' THEN 'Financial'
ELSE 'Other'
END AS DepartmentCategory
FROM Employees;

```

### IIF Function

```

-- IIF (Immediate IF) - SQL Server 2012+
SELECT FirstName, LastName,
IIF(Salary > 60000, 'High Earner', 'Standard') AS PayGrade
FROM Employees;

```

## Subqueries

### Scalar Subqueries

```

-- Subquery returning single value
SELECT FirstName, LastName, Salary
FROM Employees
WHERE Salary > (SELECT AVG(Salary) FROM Employees);

```

### Correlated Subqueries

```

-- Correlated subquery
SELECT FirstName, LastName, Department, Salary
FROM Employees e1
WHERE Salary > (
SELECT AVG(Salary)
FROM Employees e2
WHERE e2.Department = e1.Department
);

```

### EXISTS

```

-- EXISTS subquery
SELECT FirstName, LastName
FROM Employees e
WHERE EXISTS (
SELECT 1
FROM Projects p
WHERE p.EmployeeID = e.EmployeeID
);

```

## Window Functions

```

-- ROW_NUMBER
SELECT FirstName, LastName, Salary,
ROW_NUMBER() OVER (ORDER BY Salary DESC) AS SalaryRank
FROM Employees;

-- RANK and DENSE_RANK
SELECT FirstName, LastName, Salary,
RANK() OVER (ORDER BY Salary DESC) AS Rank,
DENSE_RANK() OVER (ORDER BY Salary DESC) AS DenseRank
FROM Employees;

-- Partition by department
SELECT FirstName, LastName, Department, Salary,
ROW_NUMBER() OVER (PARTITION BY Department ORDER BY Salary DESC) AS DeptRank
FROM Employees;

-- LAG and LEAD
SELECT FirstName, LastName, Salary,
LAG(Salary, 1) OVER (ORDER BY Salary) AS PreviousSalary,
LEAD(Salary, 1) OVER (ORDER BY Salary) AS NextSalary
FROM Employees;

```

## Common Table Expressions (CTEs)

```

-- Basic CTE
WITH HighEarners AS (
SELECT FirstName, LastName, Salary
FROM Employees
WHERE Salary > 65000
)
SELECT \* FROM HighEarners ORDER BY Salary DESC;

-- Recursive CTE (Organizational Hierarchy)
WITH EmployeeHierarchy AS (
-- Anchor: Top-level employees (no manager)
SELECT EmployeeID, FirstName, LastName, ManagerID, 0 AS Level
FROM Employees
WHERE ManagerID IS NULL

    UNION ALL

    -- Recursive: Employees with managers
    SELECT e.EmployeeID, e.FirstName, e.LastName, e.ManagerID, eh.Level + 1
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmployeeID
    )

SELECT \* FROM EmployeeHierarchy ORDER BY Level, LastName;

```

## Indexes

```

-- Create clustered index
CREATE CLUSTERED INDEX IX_Employee_ID ON Employees(EmployeeID);

-- Create non-clustered index
CREATE NONCLUSTERED INDEX IX_Employee_Email ON Employees(Email);

-- Create composite index
CREATE INDEX IX_Employee_Dept_Salary ON Employees(Department, Salary);

-- Drop index
DROP INDEX IX_Employee_Email ON Employees;

```

## Stored Procedures

```

-- Create stored procedure
CREATE PROCEDURE GetEmployeesByDepartment
@Department VARCHAR(50)
AS
BEGIN
SELECT FirstName, LastName, Email, Salary
FROM Employees
WHERE Department = @Department
ORDER BY LastName;
END;

-- Execute stored procedure
EXEC GetEmployeesByDepartment @Department = 'IT';

-- Stored procedure with output parameter
CREATE PROCEDURE GetDepartmentStats
@Department VARCHAR(50),
@EmployeeCount INT OUTPUT,
@AvgSalary DECIMAL(10,2) OUTPUT
AS
BEGIN
SELECT @EmployeeCount = COUNT(\*),
@AvgSalary = AVG(Salary)
FROM Employees
WHERE Department = @Department;
END;

-- Execute with output parameters
DECLARE @Count INT, @Avg DECIMAL(10,2);
EXEC GetDepartmentStats 'IT', @Count OUTPUT, @Avg OUTPUT;
SELECT @Count AS EmployeeCount, @Avg AS AvgSalary;

```

## Transactions

```

-- Basic transaction
BEGIN TRANSACTION;

UPDATE Employees SET Salary = Salary \* 1.1 WHERE Department = 'IT';
INSERT INTO AuditLog (Action, TableName, Timestamp)
VALUES ('Salary Update', 'Employees', GETDATE());

-- Check if everything is OK, then commit
IF @@ERROR = 0
COMMIT TRANSACTION;
ELSE
ROLLBACK TRANSACTION;

-- Transaction with try-catch
BEGIN TRY
BEGIN TRANSACTION;

    DELETE FROM Employees WHERE EmployeeID = 999;
    UPDATE Departments SET Budget = Budget - 50000 WHERE DepartmentName = 'IT';

    COMMIT TRANSACTION;
    PRINT 'Transaction completed successfully';
    END TRY

BEGIN CATCH
IF @@TRANCOUNT > 0
ROLLBACK TRANSACTION;

    PRINT 'Error occurred: ' + ERROR_MESSAGE();
    END CATCH;

```

## Performance Tips

1. **Use appropriate indexes** on frequently queried columns
2. **Avoid SELECT *** - specify only needed columns
3. **Use WHERE clauses** to filter data early
4. **Use EXISTS instead of IN** for subqueries when possible
5. **Avoid functions in WHERE clauses** on indexed columns
6. **Use UNION ALL instead of UNION** when duplicates don't matter
7. **Consider query execution plans** using `SET STATISTICS IO ON`

## Quick Reference

| Operation | Syntax |
|-----------|--------|
| Select All | `SELECT * FROM table` |
| Filter | `WHERE condition` |
| Sort | `ORDER BY column [ASC\|DESC]` |
| Group | `GROUP BY column` |
| Join | `JOIN table2 ON condition` |
| Insert | `INSERT INTO table VALUES (...)` |
| Update | `UPDATE table SET column = value WHERE condition` |
| Delete | `DELETE FROM table WHERE condition` |
| Create Table | `CREATE TABLE name (column datatype, ...)` |
| Drop Table | `DROP TABLE name` |

---

**Note:** This cheatsheet covers SQL Server (MS SQL) specific syntax. Some functions and features may vary in other database systems.
```

This README.md formatted version includes proper markdown syntax with code blocks, headers, tables, and formatting that will display nicely in any GitHub repository or documentation system.
