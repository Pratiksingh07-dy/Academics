# 🗄️ Database Normalization

> **Normalization** is the process of organizing data in a relational database to reduce redundancy, eliminate anomalies, and improve data integrity.

---

## 🎯 Why Normalization?

A poorly designed database can lead to:

🔴 **Insertion Anomaly**

> Unable to insert data without adding unrelated information.

🔴 **Update Anomaly**

> Same information exists in multiple rows, causing inconsistencies.

🔴 **Deletion Anomaly**

> Deleting one record may accidentally remove important data.

### Example

```text
CustomerID | CustomerName | Product
------------------------------------
101        | Alice        | Keyboard
101        | Alice        | Mouse
```

If Alice's name changes, every row must be updated.

---

# 🧠 Functional Dependency (FD)

> A Functional Dependency describes a relationship where one attribute uniquely determines another.

```text
A → B
```

Meaning:

> If two records have the same value of **A**, they must also have the same value of **B**.

### Example

```text
StudentID → StudentName
```

Knowing the Student ID uniquely identifies the Student Name.

---

# 🚀 Normalization Roadmap

```text
UNF
 ↓
1NF
 ↓
2NF
 ↓
3NF
 ↓
BCNF
 ↓
4NF
 ↓
5NF
```

---

# 1️⃣ First Normal Form (1NF)

### Definition

> A table is in First Normal Form if every column contains atomic (indivisible) values and there are no repeating groups.

### Rules

✔ Atomic Values

✔ Unique Rows

✔ Unique Columns

✔ No Multi-Valued Attributes

### Before 1NF ❌

| StudentID | Subjects     |
| --------- | ------------ |
| 101       | DBMS, OS, CN |

### After 1NF ✅

| StudentID | Subject |
| --------- | ------- |
| 101       | DBMS    |
| 101       | OS      |
| 101       | CN      |

### Interview Shortcut

> **1NF removes repeating groups and multi-valued attributes.**

---

# 2️⃣ Second Normal Form (2NF)

### Definition

> A table is in Second Normal Form if it is already in 1NF and every non-key attribute depends on the entire primary key.

### Removes

❌ Partial Dependency

### Example

```text
(StudentID, CourseID) → Grade
StudentID → StudentName
```

StudentName depends only on StudentID, not the entire composite key.

### Solution

Split the table into:

```text
Student
Course
Enrollment
```

### Interview Shortcut

> **2NF removes partial dependencies.**

---

# 3️⃣ Third Normal Form (3NF)

### Definition

> A table is in Third Normal Form if it is already in 2NF and contains no transitive dependencies.

### Removes

❌ Transitive Dependency

### Example

```text
EmpID → DeptID
DeptID → DeptName
```

Therefore:

```text
EmpID → DeptName
```

This is a transitive dependency.

### Solution

Create a separate Department table.

### Interview Shortcut

> **3NF ensures non-key attributes depend only on the primary key.**

---

# 4️⃣ Boyce-Codd Normal Form (BCNF)

### Definition

> BCNF is a stronger version of 3NF where every determinant must be a super key.

### Rule

For every dependency:

```text
A → B
```

A must be a Super Key.

### Removes

❌ Anomalies not handled by 3NF

### Interview Shortcut

> **Every determinant must be a Super Key.**

---

# 5️⃣ Fourth Normal Form (4NF)

### Definition

> A table is in Fourth Normal Form if it is in BCNF and contains no multi-valued dependencies.

### Example

```text
EmpID →→ Skill
EmpID →→ Hobby
```

Both facts are independent.

### Solution

```text
EmployeeSkill
EmployeeHobby
```

### Interview Shortcut

> **4NF removes multi-valued dependencies.**

---

# 6️⃣ Fifth Normal Form (5NF)

### Definition

> A table is in Fifth Normal Form if it cannot be decomposed further without losing information.

### Removes

❌ Join Dependency

### Used In

* Data Warehousing
* Research Systems
* Complex Relational Models

### Interview Shortcut

> **5NF removes join dependency.**

---

# ⚖️ 3NF vs BCNF

| Feature                       | 3NF | BCNF |
| ----------------------------- | --- | ---- |
| Removes Transitive Dependency | ✅   | ✅    |
| Determinant Must Be Super Key | ❌   | ✅    |
| Stricter Form                 | ❌   | ✅    |

---

# 📌 Quick Revision

| Normal Form | Removes                    |
| ----------- | -------------------------- |
| 1NF         | Repeating Groups           |
| 2NF         | Partial Dependency         |
| 3NF         | Transitive Dependency      |
| BCNF        | Non-Super Key Determinants |
| 4NF         | Multi-Valued Dependency    |
| 5NF         | Join Dependency            |

---

# 🎤 Viva Questions

### What is Normalization?

> The process of organizing data to reduce redundancy and improve integrity.

### Why is Normalization required?

> To eliminate insertion, update, and deletion anomalies.

### Difference Between 2NF and 3NF?

> 2NF removes partial dependency, whereas 3NF removes transitive dependency.

### Difference Between 3NF and BCNF?

> BCNF is stricter and requires every determinant to be a super key.

---

## 🏆 One-Line Summary

```text
1NF → Atomic Values

2NF → No Partial Dependency

3NF → No Transitive Dependency

BCNF → Determinant = Super Key

4NF → No Multi-Valued Dependency

5NF → No Join Dependency
```

---

<div align="center">

### ⭐ Star this repository if it helped you learn DBMS!

</div>
