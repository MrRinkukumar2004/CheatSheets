

## 📋 Table of Contents

<details>
<summary>Click to expand navigation</summary>

- [🏗️ Database Operations](#️-database-operations)
- [🏷️ Table Operations](#️-table-operations)
- [📊 Data Manipulation](#-data-manipulation)
- [🔍 Querying Data](#-querying-data)
- [📈 Aggregate Functions](#-aggregate-functions-and-grouping)
- [🔗 Joins](#-joins)
- [📝 String Functions](#-string-functions)
- [📅 Date & Time](#-date-and-time-functions)
- [🎯 Conditional Logic](#-conditional-logic)
- [🔄 Subqueries](#-subqueries)
- [🪟 Window Functions](#-window-functions)
- [🌲 CTEs](#-common-table-expressions-ctes)
- [⚡ Indexes](#-indexes)
- [🏭 Stored Procedures](#-stored-procedures)
- [🔄 Transactions](#-transactions)
- [🚀 Performance Tips](#-performance-tips)
- [📖 Quick Reference](#-quick-reference)

</details>

---

## 🏗️ Database Operations

> **Tip**: Always backup your database before making structural changes!

### 🏢 Creating and Managing Databases
```

-- 🆕 Create a new database
CREATE DATABASE CompanyDB;

-- 🎯 Use a specific database
USE CompanyDB;

-- 🗑️ Drop a database (⚠️ Use with caution!)
DROP DATABASE CompanyDB;

-- 📋 Show all databases
SELECT name FROM sys.databases;

```

<div align="center">
<table>
<tr><td><strong>💡 Best Practice</strong></td></tr>
<tr><td>Always specify initial size and growth settings when creating production databases</td></tr>
</table>
</div>

---

## 🏷️ Table Operations

### 🏗️ Creating Tables

```

-- 📋 Create a comprehensive employee table
CREATE TABLE Employees (
EmployeeID INT PRIMARY KEY IDENTITY(1,1), -- 🔑 Auto-incrementing primary key
FirstName VARCHAR(50) NOT NULL, -- ✅ Required field
LastName VARCHAR(50) NOT NULL, -- ✅ Required field
Email VARCHAR(100) UNIQUE, -- 🔒 Unique constraint
Department VARCHAR(50), -- 📁 Department info
Salary DECIMAL(10,2), -- 💰 Salary with 2 decimal places
HireDate DATE DEFAULT GETDATE(), -- 📅 Auto-set hire date
IsActive BIT DEFAULT 1 -- ✅ Active status flag
);

```

### 🔧 Modifying Tables

```

-- ➕ Add a new column
ALTER TABLE Employees ADD Phone VARCHAR(15);

-- 🔄 Modify column data type
ALTER TABLE Employees ALTER COLUMN Phone VARCHAR(20);

-- ➖ Drop a column
ALTER TABLE Employees DROP COLUMN Phone;

-- 🗑️ Drop entire table
DROP TABLE Employees;

```

> **⚠️ Warning**: Dropping columns or tables permanently removes data. Always backup first!

---

## 📊 Data Manipulation

### ➕ Inserting Data

<details>
<summary><strong>Single Record Insert</strong></summary>

```

-- 📝 Insert single employee record
INSERT INTO Employees (FirstName, LastName, Email, Department, Salary)
VALUES ('John', 'Doe', 'john.doe@company.com', 'IT', 65000.00);

```
</details>

<details>
<summary><strong>Bulk Insert</strong></summary>

```

-- 📝 Insert multiple employees at once
INSERT INTO Employees (FirstName, LastName, Email, Department, Salary)
VALUES
('Jane', 'Smith', 'jane.smith@company.com', 'HR', 55000.00),
('Bob', 'Johnson', 'bob.johnson@company.com', 'Finance', 60000.00),
('Alice', 'Williams', 'alice.williams@company.com', 'IT', 70000.00);

```
</details>

### 🔄 Updating Data

```

-- 💰 Give John a raise
UPDATE Employees
SET Salary = 68000.00
WHERE EmployeeID = 1;

-- 🏢 Department rename with 10% raise
UPDATE Employees
SET Department = 'Engineering', Salary = Salary \* 1.1
WHERE Department = 'IT';

-- 🔗 Complex update with JOIN
UPDATE e
SET e.Salary = e.Salary \* 1.05
FROM Employees e
INNER JOIN Departments d ON e.Department = d.DepartmentName
WHERE d.Budget > 100000;

```

### 🗑️ Deleting Data

```

-- ❌ Remove specific employee
DELETE FROM Employees WHERE EmployeeID = 5;

-- 💸 Remove low-salary employees
DELETE FROM Employees WHERE Salary < 40000;

-- 🧹 Clear all data (keeps structure)
DELETE FROM Employees;

-- ⚡ Fast clear (resets identity)
TRUNCATE TABLE Employees;

```

---

## 🔍 Querying Data

### 📋 Basic SELECT Statements

```

-- 📊 Get everything
SELECT \* FROM Employees;

-- 🎯 Select specific columns
SELECT FirstName, LastName, Salary FROM Employees;

-- 🏷️ Use meaningful aliases
SELECT FirstName + ' ' + LastName AS FullName,
Salary AS AnnualSalary
FROM Employees;

-- 🔍 Get unique values only
SELECT DISTINCT Department FROM Employees;

```

### 🎯 Filtering with WHERE

<details>
<summary><strong>Basic Filtering</strong></summary>

```

-- 🏢 Filter by department
SELECT \* FROM Employees WHERE Department = 'IT';

-- 🔗 Multiple conditions
SELECT \* FROM Employees
WHERE Department = 'IT' AND Salary > 60000;

-- 📊 OR conditions
SELECT \* FROM Employees
WHERE Department = 'HR' OR Department = 'Finance';

```
</details>

<details>
<summary><strong>Pattern Matching</strong></summary>

```

-- 🔤 Names starting with 'J'
SELECT \* FROM Employees WHERE FirstName LIKE 'J%';

-- 📧 Company email addresses
SELECT \* FROM Employees WHERE Email LIKE '%@company.com';

-- 📊 Salary range
SELECT \* FROM Employees WHERE Salary BETWEEN 50000 AND 70000;

-- 📋 Department list
SELECT \* FROM Employees WHERE Department IN ('IT', 'HR', 'Finance');

```
</details>

<details>
<summary><strong>NULL Handling</strong></summary>

```

-- ❌ Missing emails
SELECT \* FROM Employees WHERE Email IS NULL;

-- ✅ Valid emails
SELECT \* FROM Employees WHERE Email IS NOT NULL;

```
</details>

### 📈 Sorting and Limiting

```

-- 💰 Highest paid first
SELECT \* FROM Employees ORDER BY Salary DESC;

-- 🏢 Department then salary
SELECT \* FROM Employees ORDER BY Department ASC, Salary DESC;

-- 🏆 Top 5 earners
SELECT TOP 5 \* FROM Employees ORDER BY Salary DESC;

-- 📄 Pagination (Skip 10, Take 5)
SELECT \* FROM Employees
ORDER BY EmployeeID
OFFSET 10 ROWS
FETCH NEXT 5 ROWS ONLY;

```

---

## 📈 Aggregate Functions and Grouping

### 🧮 Common Aggregate Functions

<div align="center">
<table>
<tr>
<th>Function</th>
<th>Purpose</th>
<th>Example</th>
</tr>
<tr>
<td>COUNT(*)</td>
<td>Count all rows</td>
<td>Total employees</td>
</tr>
<tr>
<td>SUM()</td>
<td>Add values</td>
<td>Total payroll</td>
</tr>
<tr>
<td>AVG()</td>
<td>Average value</td>
<td>Average salary</td>
</tr>
<tr>
<td>MIN()/MAX()</td>
<td>Minimum/Maximum</td>
<td>Salary range</td>
</tr>
</table>
</div>

```

-- 👥 Count total employees
SELECT COUNT(\*) AS TotalEmployees FROM Employees;

-- 💰 Financial summary
SELECT SUM(Salary) AS TotalPayroll,
AVG(Salary) AS AverageSalary,
MIN(Salary) AS MinSalary,
MAX(Salary) AS MaxSalary
FROM Employees;

-- 🏢 Department analytics
SELECT Department,
COUNT(\*) AS HeadCount,
AVG(Salary) AS AvgSalary,
SUM(Salary) AS DeptBudget
FROM Employees
GROUP BY Department;

-- 🎯 High-performing departments only
SELECT Department, AVG(Salary) AS AvgSalary
FROM Employees
GROUP BY Department
HAVING AVG(Salary) > 60000;

```

---

## 🔗 Joins

### 🎯 Inner Join
```

-- 🤝 Match employees with their departments
SELECT e.FirstName, e.LastName, d.DepartmentName, d.Location
FROM Employees e
INNER JOIN Departments d ON e.Department = d.DepartmentName;

```

### 🔄 Outer Joins

<div align="center">
<table>
<tr>
<th>Join Type</th>
<th>Description</th>
<th>Use Case</th>
</tr>
<tr>
<td>LEFT JOIN</td>
<td>All from left table</td>
<td>All employees + their departments (if any)</td>
</tr>
<tr>
<td>RIGHT JOIN</td>
<td>All from right table</td>
<td>All departments + their employees (if any)</td>
</tr>
<tr>
<td>FULL OUTER</td>
<td>All from both tables</td>
<td>Everything regardless of matches</td>
</tr>
</table>
</div>

```

-- ⬅️ Left join - All employees, departments optional
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
LEFT JOIN Departments d ON e.Department = d.DepartmentName;

-- ➡️ Right join - All departments, employees optional
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
RIGHT JOIN Departments d ON e.Department = d.DepartmentName;

-- 🔄 Full outer join - Everything
SELECT e.FirstName, e.LastName, d.DepartmentName
FROM Employees e
FULL OUTER JOIN Departments d ON e.Department = d.DepartmentName;

```

### 🪞 Self Join
```

-- 👥 Find employees and their managers
SELECT e1.FirstName + ' ' + e1.LastName AS Employee,
e2.FirstName + ' ' + e2.LastName AS Manager
FROM Employees e1
LEFT JOIN Employees e2 ON e1.ManagerID = e2.EmployeeID;

```

---

## 📝 String Functions

<div align="center">

| 🎯 Function | 📝 Purpose | 💡 Example |
|-------------|------------|------------|
| `CONCAT()` | Join strings | Combine first + last name |
| `UPPER()/LOWER()` | Change case | Standardize data |
| `LEN()` | String length | Validate input |
| `SUBSTRING()` | Extract portion | Get initials |
| `REPLACE()` | Find & replace | Update domains |
| `TRIM()` | Remove spaces | Clean data |

</div>

```

-- 🔗 String concatenation methods
SELECT FirstName + ' ' + LastName AS FullName FROM Employees;
SELECT CONCAT(FirstName, ' ', LastName) AS FullName FROM Employees;

-- 🔤 Case manipulation
SELECT UPPER(FirstName) AS UpperName,
LOWER(LastName) AS LowerName,
LEN(FirstName) AS NameLength
FROM Employees;

-- ✂️ String extraction
SELECT SUBSTRING(FirstName, 1, 3) AS Initials,
LEFT(FirstName, 2) AS FirstTwo,
RIGHT(LastName, 3) AS LastThree
FROM Employees;

-- 🔄 String replacement
SELECT REPLACE(Email, '@company.com', '@newcompany.com') AS NewEmail
FROM Employees;

-- 🧹 Clean whitespace
SELECT LTRIM(RTRIM(FirstName)) AS CleanName FROM Employees;

```

---

## 📅 Date and Time Functions

### ⏰ Current Date/Time
```

-- 🕐 Current timestamps
SELECT GETDATE() AS LocalTime,
GETUTCDATE() AS UTCTime;

```

### 📊 Date Parts
```

-- 📅 Extract date components
SELECT FirstName, LastName, HireDate,
YEAR(HireDate) AS HireYear,
MONTH(HireDate) AS HireMonth,
DAY(HireDate) AS HireDay,
DATEPART(QUARTER, HireDate) AS HireQuarter
FROM Employees;

```

### 🧮 Date Calculations
```

-- ➕ Date arithmetic
SELECT FirstName, LastName, HireDate,
DATEADD(YEAR, 1, HireDate) AS OneYearAnniversary,
DATEDIFF(YEAR, HireDate, GETDATE()) AS YearsEmployed,
DATEDIFF(DAY, HireDate, GETDATE()) AS DaysEmployed
FROM Employees;

-- 🎨 Date formatting
SELECT HireDate,
FORMAT(HireDate, 'yyyy-MM-dd') AS ISOFormat,
FORMAT(HireDate, 'MMMM dd, yyyy') AS LongFormat,
FORMAT(HireDate, 'MMM yyyy') AS MonthYear
FROM Employees;

```

---

## 🎯 Conditional Logic

### 🔀 CASE Statements

```

-- 💰 Salary categorization
SELECT FirstName, LastName, Salary,
CASE
WHEN Salary > 70000 THEN '💰 High'
WHEN Salary > 50000 THEN '💵 Medium'
ELSE '💴 Entry Level'
END AS SalaryLevel
FROM Employees;

-- 🏢 Department mapping
SELECT FirstName, LastName, Department,
CASE Department
WHEN 'IT' THEN '💻 Technology'
WHEN 'HR' THEN '👥 Human Resources'
WHEN 'Finance' THEN '💰 Financial'
ELSE '🏢 Other'
END AS DepartmentCategory
FROM Employees;

```

### ⚡ IIF Function
```

-- 🎯 Quick conditional (SQL Server 2012+)
SELECT FirstName, LastName,
IIF(Salary > 60000, '⭐ High Earner', '📊 Standard') AS PayGrade
FROM Employees;

```

---

## 🔄 Subqueries

### 📊 Scalar Subqueries
```

-- 🎯 Above average earners
SELECT FirstName, LastName, Salary
FROM Employees
WHERE Salary > (SELECT AVG(Salary) FROM Employees);

```

### 🔗 Correlated Subqueries
```

-- 📈 Above department average
SELECT FirstName, LastName, Department, Salary
FROM Employees e1
WHERE Salary > (
SELECT AVG(Salary)
FROM Employees e2
WHERE e2.Department = e1.Department
);

```

### ✅ EXISTS
```

-- 📋 Employees with active projects
SELECT FirstName, LastName
FROM Employees e
WHERE EXISTS (
SELECT 1
FROM Projects p
WHERE p.EmployeeID = e.EmployeeID
AND p.Status = 'Active'
);

```

---

## 🪟 Window Functions

> **💡 Pro Tip**: Window functions are perfect for analytics without grouping!

```

-- 🏆 Salary rankings
SELECT FirstName, LastName, Salary,
ROW_NUMBER() OVER (ORDER BY Salary DESC) AS SalaryRank,
RANK() OVER (ORDER BY Salary DESC) AS Rank,
DENSE_RANK() OVER (ORDER BY Salary DESC) AS DenseRank
FROM Employees;

-- 🏢 Department rankings
SELECT FirstName, LastName, Department, Salary,
ROW_NUMBER() OVER (PARTITION BY Department ORDER BY Salary DESC) AS DeptRank
FROM Employees;

-- 📊 Moving comparisons
SELECT FirstName, LastName, Salary,
LAG(Salary, 1) OVER (ORDER BY Salary) AS PreviousSalary,
LEAD(Salary, 1) OVER (ORDER BY Salary) AS NextSalary,
Salary - LAG(Salary, 1) OVER (ORDER BY Salary) AS SalaryDifference
FROM Employees;

```

---

## 🌲 Common Table Expressions (CTEs)

### 📋 Basic CTE
```

-- 💰 High earners analysis
WITH HighEarners AS (
SELECT FirstName, LastName, Salary, Department
FROM Employees
WHERE Salary > 65000
)
SELECT Department,
COUNT(\*) as HighEarnerCount,
AVG(Salary) as AvgHighSalary
FROM HighEarners
GROUP BY Department
ORDER BY AvgHighSalary DESC;

```

### 🔄 Recursive CTE
```

-- 🌳 Organizational hierarchy
WITH EmployeeHierarchy AS (
-- 👑 Top-level (CEOs, Presidents)
SELECT EmployeeID, FirstName, LastName, ManagerID, 0 AS Level,
CAST(FirstName + ' ' + LastName AS VARCHAR(1000)) AS HierarchyPath
FROM Employees
WHERE ManagerID IS NULL

    UNION ALL

    -- 👥 Everyone else
    SELECT e.EmployeeID, e.FirstName, e.LastName, e.ManagerID, eh.Level + 1,
           eh.HierarchyPath + ' -> ' + e.FirstName + ' ' + e.LastName
    FROM Employees e
    INNER JOIN EmployeeHierarchy eh ON e.ManagerID = eh.EmployeeID
    )

SELECT Level,
REPLICATE(' ', Level) + FirstName + ' ' + LastName AS IndentedName,
HierarchyPath
FROM EmployeeHierarchy
ORDER BY Level, LastName;

```

---

## ⚡ Indexes

<div align="center">
<table>
<tr>
<th>🎯 Index Type</th>
<th>📝 Description</th>
<th>💡 Best For</th>
</tr>
<tr>
<td>Clustered</td>
<td>Physical order of data</td>
<td>Primary keys, frequently sorted columns</td>
</tr>
<tr>
<td>Non-Clustered</td>
<td>Logical pointers to data</td>
<td>Foreign keys, search columns</td>
</tr>
<tr>
<td>Composite</td>
<td>Multiple columns together</td>
<td>Multi-column WHERE clauses</td>
</tr>
</table>
</div>

```

-- 🔑 Primary key clustered index
CREATE CLUSTERED INDEX IX_Employee_ID ON Employees(EmployeeID);

-- 📧 Email lookup index
CREATE NONCLUSTERED INDEX IX_Employee_Email ON Employees(Email);

-- 🔍 Department + salary composite index
CREATE INDEX IX_Employee_Dept_Salary ON Employees(Department, Salary);

-- 📊 Index with included columns (covering index)
CREATE INDEX IX_Employee_Dept_Covering
ON Employees(Department)
INCLUDE (FirstName, LastName, Salary);

-- 🗑️ Remove index
DROP INDEX IX_Employee_Email ON Employees;

```

---

## 🏭 Stored Procedures

### 📋 Basic Procedure
```

-- 🔍 Get employees by department
CREATE PROCEDURE GetEmployeesByDepartment
@Department VARCHAR(50),
@MinSalary DECIMAL(10,2) = 0 -- Optional parameter with default
AS
BEGIN
SET NOCOUNT ON; -- 🚀 Performance optimization

    SELECT FirstName, LastName, Email, Salary
    FROM Employees
    WHERE Department = @Department
      AND Salary >= @MinSalary
    ORDER BY Salary DESC, LastName;
    END;

-- 🚀 Execute the procedure
EXEC GetEmployeesByDepartment @Department = 'IT', @MinSalary = 50000;

```

### 📊 Procedure with Output Parameters
```

-- 📈 Department statistics
CREATE PROCEDURE GetDepartmentStats
@Department VARCHAR(50),
@EmployeeCount INT OUTPUT,
@AvgSalary DECIMAL(10,2) OUTPUT,
@TotalBudget DECIMAL(12,2) OUTPUT
AS
BEGIN
SELECT @EmployeeCount = COUNT(\*),
@AvgSalary = AVG(Salary),
@TotalBudget = SUM(Salary)
FROM Employees
WHERE Department = @Department;

    -- Return status: 0 = success, 1 = no data found
    IF @EmployeeCount = 0
        RETURN 1;
    ELSE
        RETURN 0;
    END;

-- 📊 Execute with outputs
DECLARE @Count INT, @Avg DECIMAL(10,2), @Total DECIMAL(12,2), @Status INT;
EXEC @Status = GetDepartmentStats 'IT', @Count OUTPUT, @Avg OUTPUT, @Total OUTPUT;

SELECT
@Status AS StatusCode,
@Count AS EmployeeCount,
@Avg AS AvgSalary,
@Total AS TotalBudget;

```

---

## 🔄 Transactions

### 🔒 Basic Transaction
```

-- 💰 Salary adjustment with audit
BEGIN TRANSACTION SalaryAdjustment;

BEGIN TRY
-- 📈 Apply 10% raise to IT department
UPDATE Employees
SET Salary = Salary \* 1.1
WHERE Department = 'IT';

    -- 📋 Log the change
    INSERT INTO AuditLog (Action, TableName, RecordsAffected, Timestamp, UserName)
    VALUES ('Salary Increase', 'Employees', @@ROWCOUNT, GETDATE(), SYSTEM_USER);

    -- ✅ All good, make it permanent
    COMMIT TRANSACTION SalaryAdjustment;
    PRINT '✅ Transaction completed successfully - ' + CAST(@@ROWCOUNT AS VARCHAR(10)) + ' records updated';
    END TRY

BEGIN CATCH
-- ❌ Something went wrong, undo everything
IF @@TRANCOUNT > 0
ROLLBACK TRANSACTION SalaryAdjustment;

    PRINT '❌ Transaction failed: ' + ERROR_MESSAGE();
    PRINT '🔄 All changes have been rolled back';
    END CATCH;

```

### 🛡️ Transaction with Savepoints
```

BEGIN TRANSACTION MainTransaction;

-- 📍 Set savepoint
SAVE TRANSACTION SavePoint1;

UPDATE Employees SET Salary = Salary \* 1.05 WHERE Department = 'HR';

-- 🤔 If something specific goes wrong, rollback to savepoint
IF @@ERROR <> 0
BEGIN
ROLLBACK TRANSACTION SavePoint1;
PRINT '⚠️ Rolled back to savepoint, but transaction continues';
END

-- 📍 Another savepoint
SAVE TRANSACTION SavePoint2;

UPDATE Employees SET Salary = Salary \* 1.1 WHERE Department = 'IT';

-- ✅ Commit everything
COMMIT TRANSACTION MainTransaction;

```

---

## 🚀 Performance Tips

<div align="center">
<table>
<tr>
<td>🎯</td>
<td><strong>DO</strong></td>
<td>❌</td>
<td><strong>DON'T</strong></td>
</tr>
<tr>
<td>✅</td>
<td>Use specific column names</td>
<td>❌</td>
<td>SELECT *</td>
</tr>
<tr>
<td>✅</td>
<td>Create appropriate indexes</td>
<td>❌</td>
<td>Over-index tables</td>
</tr>
<tr>
<td>✅</td>
<td>Use WHERE clauses early</td>
<td>❌</td>
<td>Filter after SELECT</td>
</tr>
<tr>
<td>✅</td>
<td>Use EXISTS for existence checks</td>
<td>❌</td>
<td>Use IN with subqueries</td>
</tr>
<tr>
<td>✅</td>
<td>Use UNION ALL when possible</td>
<td>❌</td>
<td>Use UNION unnecessarily</td>
</tr>
</table>
</div>

### 🔍 Query Analysis
```

-- 📊 Enable query statistics
SET STATISTICS IO ON;
SET STATISTICS TIME ON;

-- Your query here
SELECT \* FROM Employees WHERE Department = 'IT';

-- 📈 Check execution plan
SET SHOWPLAN_ALL ON;

```

### 🎯 Optimization Examples

```

-- ❌ Slow - Function in WHERE clause
SELECT \* FROM Employees WHERE YEAR(HireDate) = 2023;

-- ✅ Fast - SARGable query
SELECT \* FROM Employees
WHERE HireDate >= '2023-01-01'
AND HireDate < '2024-01-01';

-- ❌ Slow - Leading wildcards
SELECT \* FROM Employees WHERE LastName LIKE '%son';

-- ✅ Fast - Leading characters
SELECT \* FROM Employees WHERE LastName LIKE 'John%';

```

---

## 📖 Quick Reference

<div align="center">

### 🎯 Essential Commands

| 📝 Operation | 💻 Syntax | 💡 Example |
|-------------|-----------|------------|
| **Select All** | `SELECT * FROM table` | `SELECT * FROM Employees` |
| **Filter** | `WHERE condition` | `WHERE Salary > 50000` |
| **Sort** | `ORDER BY column [DESC]` | `ORDER BY Salary DESC` |
| **Group** | `GROUP BY column` | `GROUP BY Department` |
| **Join** | `JOIN table2 ON condition` | `INNER JOIN Departments ON...` |
| **Insert** | `INSERT INTO table VALUES (...)` | `INSERT INTO Employees VALUES (...)` |
| **Update** | `UPDATE table SET col = val WHERE...` | `UPDATE Employees SET Salary = 60000` |
| **Delete** | `DELETE FROM table WHERE...` | `DELETE FROM Employees WHERE ID = 1` |

</div>

### 🔧 Data Types Reference

<details>
<summary><strong>Click to expand data types</strong></summary>

| Type | Description | Example |
|------|-------------|---------|
| `INT` | Integer numbers | `42` |
| `VARCHAR(n)` | Variable-length string | `'John Doe'` |
| `DECIMAL(p,s)` | Fixed-point numbers | `DECIMAL(10,2)` for currency |
| `DATE` | Date only | `'2023-12-25'` |
| `DATETIME` | Date and time | `'2023-12-25 14:30:00'` |
| `BIT` | Boolean (0/1) | `1` for true |

</details>

### 🔗 Useful System Functions

<details>
<summary><strong>Click to expand system functions</strong></summary>

```

-- 📊 Row count from last statement
SELECT @@ROWCOUNT;

-- 🆔 Last inserted identity value
SELECT @@IDENTITY;

-- 🕐 Current date/time
SELECT GETDATE();

-- 👤 Current user
SELECT SYSTEM_USER;

-- 🖥️ Server name
SELECT @@SERVERNAME;

```

</details>

---

<div align="center">

## 🤝 Contributing

Found an error or want to add more examples? Contributions are welcome!

[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/yourusername/sql-cheatsheet)

---

## 📚 Additional Resources

- 📖 [Microsoft SQL Server Documentation](https://docs.microsoft.com/en-us/sql/sql-server/)
- 🎓 [SQL Server Learning Path](https://docs.microsoft.com/en-us/learn/browse/?products=sql-server)
- 🛠️ [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/)

---

**⚠️ Note:** This cheatsheet covers SQL Server (T-SQL) specific syntax. Some functions and features may vary in other database systems like MySQL, PostgreSQL, or Oracle.

*Last updated: August 2025*

</div>
```

## Key Improvements Made:

1. **🎨 Visual Design**:
   - Added colorful badges and shields
   - Used emojis for better visual categorization
   - Added collapsible sections for better organization
   - Centered important elements
2. **📋 Better Navigation**:
   - Comprehensive table of contents with anchors
   - Collapsible sections for detailed content
   - Quick reference tables
3. **💡 Enhanced Content**:
   - Added practical tips and warnings
   - Included "Best Practices" callouts
   - Added performance optimization examples
   - More real-world examples with context
4. **🔧 Technical Improvements**:
   - Added data types reference
   - System functions reference
   - Query optimization examples
   - Savepoint transaction examples
5. **🤝 Professional Polish**:
   - Contributing guidelines
   - Additional resources
   - Proper versioning
   - Last updated information

This improved design makes the cheatsheet more engaging, easier to navigate, and more professional-looking while maintaining all the technical accuracy and completeness of the original content.

<div style="text-align: center">⁂</div>

[^1]: https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark@2x.png
