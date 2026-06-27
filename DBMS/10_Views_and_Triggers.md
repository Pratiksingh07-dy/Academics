# 👁️ Views & Triggers

> A **View** is a virtual table based on the result of a stored SQL query, while a **Trigger** is a stored procedure that automatically executes in response to certain events on a table.

---

## 🎯 Why Do We Need Views and Triggers?

🔴 Sometimes users should only see a limited part of a table — not the entire thing

🔴 Repeating the same complex query over and over is inefficient and error-prone

🔴 Certain actions (like auditing or validation) need to happen automatically, without relying on the application to remember to do it

### Example

```text
Need                                          | Solution
-------------------------------------------------------------
Show only Name and Department to HR staff       | View
Hide salary details from non-managers              | View
Automatically log every salary update                | Trigger
Prevent invalid data from being inserted               | Trigger
```

---

# 🧠 The Views & Triggers Roadmap

```text
Database Customization
 ↓
 ├── Views        → Virtual tables (custom, restricted, or simplified data)
 └── Triggers       → Automatic procedures fired on INSERT/UPDATE/DELETE
```

---

# 1️⃣ Views

### Definition

> A View is a **virtual table** created from the result of a SQL query — it doesn't store data itself, but dynamically pulls data from one or more underlying base tables whenever it's queried.

### Rules

✔ Does not store actual data — only the query definition

✔ Always reflects the latest data from the base table(s)

✔ Can be queried just like a normal table

✔ Used for security, simplification, and abstraction

### Creating a View

```sql
CREATE VIEW EmployeePublicInfo AS
SELECT EmpID, EmpName, DeptID
FROM Employee;
```

### Using a View

```sql
SELECT * FROM EmployeePublicInfo;
```

> This hides sensitive columns like `Salary` from anyone querying the view — they simply don't exist in its result set.

### Modifying a View

```sql
CREATE OR REPLACE VIEW EmployeePublicInfo AS
SELECT EmpID, EmpName, DeptID, JoinDate
FROM Employee;
```

### Dropping a View

```sql
DROP VIEW EmployeePublicInfo;
```

### Interview Shortcut

> **View = a saved SELECT query that behaves like a table. No data is physically duplicated.**

---

# 2️⃣ Why Use Views?

```text
✔ Security          → Restrict access to specific rows/columns
✔ Simplification      → Hide complex joins behind a simple "table"
✔ Abstraction           → Underlying schema can change without breaking consumers
✔ Reusability             → Avoid rewriting the same complex query repeatedly
```

### Example — Simplifying a Complex Join

```sql
CREATE VIEW EmployeeDeptView AS
SELECT E.EmpName, D.DeptName, E.Salary
FROM Employee E
JOIN Department D ON E.DeptID = D.DeptID;
```

> Anyone can now run `SELECT * FROM EmployeeDeptView;` instead of writing the join every time.

### Interview Shortcut

> **Views simplify complex queries into a reusable, simple interface.**

---

# 3️⃣ Updatable Views

### Definition

> Some views allow `INSERT`, `UPDATE`, or `DELETE` operations to pass through to the underlying base table — but only under certain conditions.

### Conditions for an Updatable View

```text
✔ View is based on a single table (not multiple joined tables)
✔ View does not use GROUP BY, DISTINCT, or aggregate functions
✔ View includes the primary key of the base table
```

### Interview Shortcut

> **Updatable Views = simple, single-table views without aggregation. Complex views (joins, GROUP BY) usually aren't updatable.**

---

# 4️⃣ Triggers

### Definition

> A Trigger is a stored procedure that is **automatically executed ("fired")** by the database when a specified event — INSERT, UPDATE, or DELETE — occurs on a particular table.

### Rules

✔ Defined on tables, not views (with some exceptions)

✔ Can run BEFORE or AFTER the triggering event

✔ Can target INSERT, UPDATE, DELETE, or a combination

✔ Should be used sparingly — overuse leads to complex, hard-to-maintain systems

### Parts of a Trigger

```text
✔ Triggering Event/Statement  → What action causes it to fire (INSERT/UPDATE/DELETE)
✔ Trigger Restriction            → An optional condition (WHEN clause)
✔ Trigger Action                   → The actual procedure/logic that executes
```

### Syntax

```sql
CREATE [OR REPLACE] TRIGGER trigger_name
{BEFORE | AFTER | INSTEAD OF}
{INSERT [OR] | UPDATE [OR] | DELETE}
[OF col_name]
ON table_name
[REFERENCING OLD AS o NEW AS n]
[FOR EACH ROW]
WHEN (condition)
BEGIN
   -- SQL statements
END;
```

### Example — Logging Salary Changes

```sql
CREATE OR REPLACE TRIGGER Track_Salary_Changes
BEFORE UPDATE ON Employee
FOR EACH ROW
WHEN (NEW.EmpID > 0)
DECLARE
   sal_diff NUMBER;
BEGIN
   sal_diff := :NEW.Salary - :OLD.Salary;
   DBMS_OUTPUT.PUT_LINE('Old: ' || :OLD.Salary || ' New: ' || :NEW.Salary || ' Diff: ' || sal_diff);
END;
```

> This trigger fires automatically every time an employee's row is updated — no application code needs to remember to log it.

### Interview Shortcut

> **Trigger = automatic procedure fired by INSERT/UPDATE/DELETE. Runs without the application explicitly calling it.**

---

# 5️⃣ Types of Triggers

### Row-Level Trigger

> Fires **once for every row** affected by the triggering statement.

```text
UPDATE Employee SET Salary = Salary * 1.1 WHERE DeptID = 10;
→ If 5 rows match, a row-level trigger fires 5 times.
```

### Statement-Level Trigger

> Fires **only once**, regardless of how many rows are affected by the statement.

```text
The same UPDATE above would fire a statement-level trigger
exactly once, no matter how many rows were updated.
```

### BEFORE vs AFTER Triggers

```text
BEFORE Trigger  → Fires before the triggering action happens (e.g., for validation)
AFTER Trigger    → Fires after the triggering action completes (e.g., for logging)
```

### INSTEAD OF Trigger

> Used specifically on **views** (not base tables) to define what should actually happen when an INSERT/UPDATE/DELETE is attempted on a non-updatable view.

### Interview Shortcut

> **Row-Level = fires per row. Statement-Level = fires once per statement. INSTEAD OF = used on views.**

---

# ⚖️ Views vs Triggers

| Feature | Views | Triggers |
| -------- | ------- | ---------- |
| Purpose | Present/restrict data | Automate actions on data changes |
| Stores Data? | No (virtual, computed on query) | N/A (it's a procedure, not data) |
| Fires Automatically? | No — only runs when queried | Yes — fires on INSERT/UPDATE/DELETE |
| Used For | Security, simplification | Auditing, validation, automation |
| Defined On | A SELECT query | A table (mostly) |

---

# 📌 Quick Revision

| Concept | Core Idea |
| --------- | ----------- |
| View | Virtual table from a stored SELECT query |
| Updatable View | Single-table view without aggregation, can pass through INSERT/UPDATE/DELETE |
| Trigger | Procedure that auto-fires on INSERT/UPDATE/DELETE |
| Row-Level Trigger | Fires once per affected row |
| Statement-Level Trigger | Fires once per statement, regardless of row count |
| BEFORE / AFTER | Controls whether the trigger runs before or after the event |
| INSTEAD OF | Special trigger type used on views |

---

# 🎤 Viva Questions

### What is a View in DBMS?

> A View is a virtual table based on the result of a stored SQL query — it doesn't store data itself but dynamically retrieves it from the underlying base table(s) whenever queried.

### Does a View store data physically?

> No, a View only stores the query definition. The actual data always comes from the underlying base table(s) at query time.

### What is an Updatable View?

> A view that allows INSERT, UPDATE, or DELETE operations to pass through to the underlying base table, typically only possible for views based on a single table without aggregation.

### What is a Trigger in DBMS?

> A stored procedure that automatically executes in response to a specific event (INSERT, UPDATE, or DELETE) on a table, without needing to be explicitly called.

### What is the difference between a Row-Level Trigger and a Statement-Level Trigger?

> A Row-Level Trigger fires once for every row affected by the triggering statement, while a Statement-Level Trigger fires only once, regardless of how many rows were affected.

### What is the difference between a BEFORE trigger and an AFTER trigger?

> A BEFORE trigger fires before the triggering action is executed (often used for validation), while an AFTER trigger fires after the action completes (often used for logging or auditing).

### What is an INSTEAD OF trigger used for?

> It's used specifically on views to define what should actually happen when an INSERT, UPDATE, or DELETE is attempted on a view that isn't normally updatable.

### Can a Trigger be defined on a View?

> Generally, triggers are defined on tables, not views — except for INSTEAD OF triggers, which are specifically designed to work on views.

### Why should triggers be used carefully and sparingly?

> Excessive use of triggers can create complex interdependencies between tables and procedures, making the system harder to understand, debug, and maintain.

### Give a real-world use case for a View and a Trigger.

> View: Creating a restricted view that hides salary information from non-HR employees. Trigger: Automatically logging the old and new salary whenever an employee's salary is updated.

---

## 🏆 One-Line Summary

```text
View              → Virtual table from a saved SELECT query, no data stored

Updatable View      → Single-table view, allows INSERT/UPDATE/DELETE passthrough

Trigger                → Auto-fires on INSERT/UPDATE/DELETE

Row-Level Trigger        → Fires once per affected row

Statement-Level Trigger    → Fires once per statement

INSTEAD OF Trigger           → Used specifically on views
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
