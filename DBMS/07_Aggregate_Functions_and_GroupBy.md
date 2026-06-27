# 📊 Aggregate Functions & GROUP BY

> **Aggregate Functions** perform a calculation on a set of values and return a single summarizing value, while **GROUP BY** organizes rows into groups so aggregates can be applied per group instead of across the whole table.

---

## 🎯 Why Do We Need Aggregates and Grouping?

🔴 Sometimes you don't want individual rows — you want a summary (total, average, count)

🔴 Sometimes that summary needs to be calculated **per category**, not for the whole table

🔴 Without GROUP BY, aggregate functions can only summarize the entire result set at once

### Example

```text
Task                                          | Function Needed
------------------------------------------------------------------
Total number of employees                       | COUNT
Average salary across the company                | AVG
Total sales per department (not the whole company) | SUM + GROUP BY
Highest salary in each department                  | MAX + GROUP BY
```

---

# 🧠 The Aggregate Family

```text
Aggregate Functions
 ↓
 ├── COUNT()   → Counts rows
 ├── SUM()     → Adds up values
 ├── AVG()     → Calculates average
 ├── MIN()     → Finds smallest value
 └── MAX()     → Finds largest value
```

---

## 📋 Sample Table Used Throughout

**Employee**

| EmpID | EmpName | DeptID | Salary |
| ------- | --------- | -------- | -------- |
| 1 | Alice | 10 | 50000 |
| 2 | Bob | 10 | 60000 |
| 3 | Carol | 20 | 45000 |
| 4 | David | 20 | 55000 |
| 5 | Eve | 30 | 70000 |

---

# 1️⃣ COUNT()

### Definition

> COUNT returns the number of rows that match a specified condition (or the total number of rows if no condition is given).

### Syntax

```sql
SELECT COUNT(*) AS TotalEmployees
FROM Employee;
```

### Result

| TotalEmployees |
| ----------------- |
| 5 |

### Notes

```text
COUNT(*)         → Counts all rows, including NULLs
COUNT(ColumnName) → Counts only non-NULL values in that column
COUNT(DISTINCT ColumnName) → Counts unique non-NULL values
```

### Interview Shortcut

> **COUNT = how many rows. COUNT(*) includes NULLs, COUNT(column) doesn't.**

---

# 2️⃣ SUM()

### Definition

> SUM adds up all the numeric values in a specified column.

### Syntax

```sql
SELECT SUM(Salary) AS TotalSalary
FROM Employee;
```

### Result

| TotalSalary |
| -------------- |
| 280000 |

### Interview Shortcut

> **SUM = total of a numeric column.**

---

# 3️⃣ AVG()

### Definition

> AVG calculates the average (mean) of all numeric values in a specified column.

### Syntax

```sql
SELECT AVG(Salary) AS AverageSalary
FROM Employee;
```

### Result

| AverageSalary |
| ----------------- |
| 56000 |

### Interview Shortcut

> **AVG = SUM divided by COUNT, automatically calculated.**

---

# 4️⃣ MIN() and MAX()

### Definition

> MIN returns the smallest value in a column; MAX returns the largest value in a column.

### Syntax

```sql
SELECT MIN(Salary) AS LowestSalary, MAX(Salary) AS HighestSalary
FROM Employee;
```

### Result

| LowestSalary | HighestSalary |
| ---------------- | ----------------- |
| 45000 | 70000 |

### Interview Shortcut

> **MIN = smallest value. MAX = largest value.**

---

# 5️⃣ GROUP BY

### Definition

> GROUP BY arranges rows that have the same values in specified columns into groups, allowing aggregate functions to be applied **separately to each group** instead of the whole table.

### Rules

✔ Every non-aggregated column in SELECT must appear in GROUP BY

✔ Aggregate functions operate within each group, not across the whole table

✔ Grouping happens **before** aggregation is displayed

### Syntax

```sql
SELECT DeptID, SUM(Salary) AS DeptTotalSalary
FROM Employee
GROUP BY DeptID;
```

### Result

| DeptID | DeptTotalSalary |
| -------- | ------------------- |
| 10 | 110000 |
| 20 | 100000 |
| 30 | 70000 |

> Notice: SUM is now calculated **per department**, not for the entire table.

### Visual Idea

```text
Employee Table
 ↓ GROUP BY DeptID
 ├── Dept 10: [Alice-50000, Bob-60000]   → SUM = 110000
 ├── Dept 20: [Carol-45000, David-55000] → SUM = 100000
 └── Dept 30: [Eve-70000]                → SUM = 70000
```

### Interview Shortcut

> **GROUP BY = bucket rows into groups, then apply aggregates to each bucket separately.**

---

# 6️⃣ GROUP BY with Multiple Columns

### Definition

> You can group by more than one column, creating more specific subgroups based on the combination of those columns.

### Example

```sql
SELECT DeptID, JobTitle, AVG(Salary) AS AvgSalary
FROM Employee
GROUP BY DeptID, JobTitle;
```

> Groups are now formed based on the unique combination of `DeptID` AND `JobTitle` together.

### Interview Shortcut

> **Multiple GROUP BY columns = more granular subgroups.**

---

# 7️⃣ HAVING Clause

### Definition

> HAVING filters groups **after** aggregation has been applied — unlike WHERE, which filters rows **before** aggregation.

### Rules

✔ Used specifically to filter on aggregate function results

✔ Cannot use aggregate functions in WHERE — must use HAVING instead

✔ Often used together with GROUP BY

### Example

```sql
SELECT DeptID, SUM(Salary) AS DeptTotalSalary
FROM Employee
GROUP BY DeptID
HAVING SUM(Salary) > 90000;
```

### Result

| DeptID | DeptTotalSalary |
| -------- | ------------------- |
| 10 | 110000 |
| 20 | 100000 |

> Department 30 (70000) is excluded since it doesn't satisfy the HAVING condition.

### Interview Shortcut

> **HAVING filters groups after aggregation. WHERE filters rows before aggregation.**

---

# ⚖️ WHERE vs HAVING

| Feature | WHERE | HAVING |
| -------- | ------- | -------- |
| Applies to | Individual rows | Groups (after GROUP BY) |
| Used with Aggregate Functions | No | Yes |
| Execution Order | Before grouping | After grouping |
| Example Use | `WHERE Salary > 50000` | `HAVING SUM(Salary) > 90000` |

---

# 📌 Quick Revision

| Function/Clause | Core Idea |
| ------------------- | ----------- |
| COUNT() | Number of rows |
| SUM() | Total of values |
| AVG() | Mean of values |
| MIN() / MAX() | Smallest / Largest value |
| GROUP BY | Groups rows for per-group aggregation |
| HAVING | Filters groups after aggregation |

---

# 🎤 Viva Questions

### What are Aggregate Functions in SQL?

> Functions that perform a calculation on a set of values and return a single summarizing value, such as COUNT, SUM, AVG, MIN, and MAX.

### What is the purpose of the GROUP BY clause?

> GROUP BY organizes rows with the same values in specified columns into groups, so aggregate functions can be calculated separately for each group instead of the entire table.

### What is the difference between WHERE and HAVING?

> WHERE filters individual rows before grouping/aggregation occurs, and cannot use aggregate functions. HAVING filters groups after aggregation has been applied, and is specifically designed to work with aggregate functions.

### What is the difference between COUNT(*) and COUNT(ColumnName)?

> COUNT(*) counts all rows including those with NULL values. COUNT(ColumnName) counts only the rows where that specific column has a non-NULL value.

### Can you use an aggregate function in a WHERE clause? Why or why not?

> No, because WHERE filters rows before aggregation is performed, but aggregate functions operate on already-grouped data — so HAVING must be used instead.

### What happens if you SELECT a column that isn't in the GROUP BY clause or wrapped in an aggregate function?

> Most databases will throw an error, because it's ambiguous which value of that column to display when multiple rows are grouped together.

### How does GROUP BY with multiple columns work?

> It creates groups based on the unique combination of values across all the specified columns, resulting in more granular subgroups than grouping by a single column.

### What does AVG() calculate internally?

> It calculates the SUM of the values divided by the COUNT of those values, automatically handling NULLs (which are excluded from both the sum and the count).

### Write a query to find departments with more than 2 employees.

> ```sql
> SELECT DeptID, COUNT(*) AS EmpCount
> FROM Employee
> GROUP BY DeptID
> HAVING COUNT(*) > 2;
> ```

### Does GROUP BY need to be used with an aggregate function?

> Not strictly — GROUP BY can be used alone to get distinct groups — but it's most commonly paired with aggregate functions to calculate summary values per group.

---

## 🏆 One-Line Summary

```text
COUNT()    → Number of rows

SUM()      → Total of values

AVG()      → Mean of values

MIN/MAX()  → Smallest / Largest value

GROUP BY   → Buckets rows into groups for per-group aggregation

HAVING     → Filters groups after aggregation (WHERE filters before)
```

---

## References

1. Korth, Silberschatz, Sudarshan — *Database System Concepts*, 6th Edition, McGraw-Hill
2. Elmasri and Navathe — *Fundamentals of Database Systems*, 5th Edition, Pearson
3. G. K. Gupta — *Database Management Systems*, McGraw-Hill

---

<div align="center">

### ⭐ Star this repository if it helped you learn DBMS!

</div>
