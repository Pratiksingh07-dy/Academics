# 🔑 Keys in DBMS

> A **Key** is an attribute (or set of attributes) used to uniquely identify a row (tuple) in a table, or to establish relationships between tables.

---

## 🎯 Why Do We Need Keys?

🔴 Without a unique identifier, the same data could get duplicated

🔴 Without a way to link tables, related data can't be connected

🔴 Without constraints, invalid or orphaned data can creep in

### Example

```text
Task                                    | Key Used
-----------------------------------------------------
Uniquely identify a student              | Primary Key
Link an order to a customer              | Foreign Key
Ensure no two students share an email    | Unique Key / Candidate Key
Identify a row using multiple columns    | Composite Key
```

---

# 🧠 The Key Family Tree

```text
Keys in DBMS
 ↓
 ├── Super Key
 ├── Candidate Key
 ├── Primary Key
 ├── Alternate Key
 ├── Foreign Key
 ├── Composite Key
 └── Unique Key
```

---

# 1️⃣ Super Key

### Definition

> A Super Key is any combination of attributes (one or more) that can uniquely identify a row in a table. It may contain extra, unnecessary attributes.

### Rules

✔ Every set of attributes that uniquely identifies a row is a super key

✔ A table can have many super keys

✔ Adding any attribute to a key still keeps it a super key

### Example

```text
Table: Student(StudentID, Name, Email, Phone)

Super Keys:
{StudentID}
{StudentID, Name}
{StudentID, Email}
{StudentID, Name, Email, Phone}
```

### Interview Shortcut

> **Super Key = any attribute combo that uniquely identifies a row (minimal or not).**

---

# 2️⃣ Candidate Key

### Definition

> A Candidate Key is a **minimal** super key — it has no unnecessary (redundant) attributes. Removing any attribute from it would break uniqueness.

### Rules

✔ Must be unique for every row

✔ Must be minimal (no extra attributes)

✔ A table can have multiple candidate keys

✔ One candidate key is eventually chosen as the Primary Key

### Example

```text
Table: Student(StudentID, Email, AadharNumber, Name)

Candidate Keys:
{StudentID}
{Email}
{AadharNumber}
```

> All three can independently identify a student uniquely — each is a candidate key.

### Interview Shortcut

> **Candidate Key = minimal super key. Every candidate key is a super key, but not every super key is a candidate key.**

---

# 3️⃣ Primary Key

### Definition

> The Primary Key is the candidate key **selected by the database designer** to uniquely identify each row in a table.

### Rules

✔ Cannot contain NULL values

✔ Must be unique across all rows

✔ Only **one** primary key per table

✔ Automatically creates a unique index

### Example

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    Name VARCHAR(50) NOT NULL,
    Email VARCHAR(50)
);
```

| StudentID (PK) | Name | Email |
| ---------------- | ------ | ------- |
| 101 | Alice | alice@mail.com |
| 102 | Bob | bob@mail.com |

### Interview Shortcut

> **Primary Key = the chosen candidate key. No NULLs, no duplicates, only one per table.**

---

# 4️⃣ Alternate Key

### Definition

> An Alternate Key is a candidate key that was **not chosen** as the primary key.

### Example

```text
Candidate Keys: {StudentID}, {Email}, {AadharNumber}

If StudentID is chosen as Primary Key →
Email and AadharNumber become Alternate Keys.
```

### Interview Shortcut

> **Alternate Key = leftover candidate keys (the ones not picked as PK).**

---

# 5️⃣ Foreign Key

### Definition

> A Foreign Key is an attribute in one table that refers to the Primary Key of another (or the same) table — used to establish and enforce a link between two tables.

### Rules

✔ Can contain NULL values (unlike primary key)

✔ Can contain duplicate values

✔ Enforces **referential integrity** — values must exist in the referenced table

✔ A table can have multiple foreign keys

### Example

```sql
CREATE TABLE Department (
    DeptID INT PRIMARY KEY,
    DeptName VARCHAR(50)
);

CREATE TABLE Employee (
    EmpID INT PRIMARY KEY,
    EmpName VARCHAR(50),
    DeptID INT,
    FOREIGN KEY (DeptID) REFERENCES Department(DeptID)
);
```

```text
Department                      Employee
-----------------                -------------------------
DeptID (PK) | DeptName           EmpID (PK) | EmpName | DeptID (FK)
D01         | IT                 E01        | Alice   | D01
D02         | HR                 E02        | Bob     | D01
                                  E03        | Carol   | D02
```

> Every value in Employee.DeptID **must already exist** in Department.DeptID — this is referential integrity.

### Interview Shortcut

> **Foreign Key = a bridge between two tables. References a Primary Key, can be NULL, can repeat.**

---

# 6️⃣ Composite Key

### Definition

> A Composite Key is a key made up of **two or more attributes** that together uniquely identify a row, even though no single attribute can do it alone.

### Example

```text
Table: Enrollment(StudentID, CourseID, Grade)

Primary Key = {StudentID, CourseID}
```

| StudentID | CourseID | Grade |
| ----------- | ---------- | ------- |
| 101 | C01 | A |
| 101 | C02 | B |
| 102 | C01 | C |

> Neither StudentID nor CourseID alone is unique, but the combination is.

### Interview Shortcut

> **Composite Key = two or more columns combined to form uniqueness.**

---

# 7️⃣ Unique Key

### Definition

> A Unique Key ensures that all values in a column are different — similar to a Primary Key, but it **allows one NULL value** and a table can have multiple unique keys.

### Rules

✔ Allows one NULL value (some databases allow multiple NULLs)

✔ A table can have multiple unique keys

✔ Does not automatically become the row identifier like a primary key

### Example

```sql
CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    Email VARCHAR(50) UNIQUE,
    Phone VARCHAR(15) UNIQUE
);
```

<p align="center">
  <img src="./assets/Different-types-of-keys.png" alt="Different Types of Keys in DBMS" width="900">
</p>

<p align="center">
  <sub><i>Image Reference: GeeksforGeeks</i></sub>
</p>
### Interview Shortcut

> **Unique Key = like Primary Key, but allows one NULL and you can have several per table.**

---

# ⚖️ Primary Key vs Foreign Key vs Unique Key

| Feature | Primary Key | Foreign Key | Unique Key |
| -------- | -------------- | -------------- | ------------- |
| Uniqueness | Always unique | Can repeat | Always unique |
| NULL allowed | Never | Yes | Yes (typically one) |
| Count per table | Only one | Multiple allowed | Multiple allowed |
| Purpose | Identify each row | Link to another table | Enforce uniqueness on a column |
| Index created | Yes (automatic) | No (unless added manually) | Yes (automatic) |

---

# ⚖️ Super Key vs Candidate Key vs Primary Key

| Feature | Super Key | Candidate Key | Primary Key |
| -------- | ----------- | ---------------- | -------------- |
| Minimal? | Not necessarily | Yes | Yes |
| Count per table | Many | Many | Only one |
| Chosen by designer? | No | No | Yes (from candidate keys) |
| Relationship | Every candidate key is a super key | Every primary key is a candidate key | Subset of candidate keys |

---

# 📌 Quick Revision

| Key Type | Core Idea |
| ---------- | ----------- |
| Super Key | Any attribute set that uniquely identifies a row |
| Candidate Key | Minimal super key |
| Primary Key | The chosen candidate key — no NULL, no duplicates |
| Alternate Key | Candidate keys not chosen as primary |
| Foreign Key | Links to another table's primary key |
| Composite Key | Multiple columns combined for uniqueness |
| Unique Key | Like primary key, but allows one NULL |

---

# 🎤 Viva Questions

### What is a key in DBMS?

> An attribute or set of attributes used to uniquely identify a row in a table or to establish relationships between tables.

### What is the difference between a Super Key and a Candidate Key?

> A Super Key is any attribute combination that ensures uniqueness, even with redundant attributes. A Candidate Key is a minimal Super Key — no attribute can be removed without losing uniqueness.

### What is the difference between Primary Key and Unique Key?

> A Primary Key cannot contain NULL values and only one exists per table. A Unique Key can contain one NULL value and a table can have multiple unique keys.

### Can a Foreign Key contain NULL values?

> Yes, unlike a Primary Key, a Foreign Key can contain NULL values, meaning the relationship is optional for that row.

### What is referential integrity?

> A constraint ensuring that a Foreign Key value in one table must match an existing Primary Key value in the referenced table (or be NULL).

### What is a Composite Key?

> A key formed by combining two or more attributes, where no single attribute alone can guarantee uniqueness.

### What is an Alternate Key?

> A candidate key that was not selected as the Primary Key for the table.

### Can a table have more than one Primary Key?

> No, a table can have only one Primary Key, though that key can be composite (made of multiple columns).

### Why can't a Primary Key contain NULL values?

> Because the Primary Key's purpose is to uniquely identify every row, and NULL represents an unknown value — it cannot guarantee uniqueness or identity.

### What happens if you try to insert a Foreign Key value that doesn't exist in the referenced table?

> The database rejects the insert/update, throwing a referential integrity / foreign key constraint violation error.

---

## 🏆 One-Line Summary

```text
Super Key      → Any unique combo (not necessarily minimal)

Candidate Key  → Minimal unique combo

Primary Key    → Chosen candidate key (no NULL, no duplicates)

Alternate Key  → Leftover candidate keys

Foreign Key    → Links to another table's Primary Key

Composite Key  → Multiple columns combined for uniqueness

Unique Key     → Like Primary Key, but allows one NULL
```

---

## References

1. Korth, Silberschatz, Sudarshan — *Database System Concepts*, 6th Edition, McGraw-Hill
2. Elmasri and Navathe — *Fundamentals of Database Systems*, 5th Edition, Pearson
3. G. K. Gupta — *Database Management Systems*, McGraw-Hill
4. Peter Rob and Carlos Coronel — *Database Systems: Design, Implementation and Management*, 5th Edition

---

<div align="center">

### ⭐ Star this repository if it helped you learn DBMS!

</div>