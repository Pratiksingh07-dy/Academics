# 🔒 Concurrency Control & Locking

> **Concurrency Control** is a set of techniques used by a DBMS to manage simultaneous access to data by multiple transactions, ensuring data consistency and integrity even when transactions overlap in time.

---

## 🎯 Why Do We Need Concurrency Control?

🔴 Multiple users accessing and modifying the same data at the same time can corrupt it

🔴 Without control, transactions can overwrite each other's changes

🔴 Real-world databases (banking, ticket booking) handle thousands of simultaneous transactions

### Example

```text
Scenario: Two travelers booking the last train seat at the same time

Without Concurrency Control:
  Traveler A reads: 1 seat available
  Traveler B reads: 1 seat available
  Both book the same seat → Overbooking!

With Concurrency Control:
  Traveler A locks the seat record, books it, releases the lock
  Traveler B must wait, then sees 0 seats available
  → No conflict, data stays accurate
```

---

# 🧠 The Concurrency Control Roadmap

```text
Concurrency Control
 ↓
 ├── Problems Without Control   → Lost Update, Dirty Read, Unrepeatable Read
 ├── Lock-Based Protocols         → Pessimistic Locking (Read Lock, Write Lock)
 ├── Optimistic Concurrency Control → Detect conflicts late, not early
 ├── Timestamp Ordering             → Order transactions using timestamps
 └── Problems to Avoid                → Deadlock, Livelock
```

---

# 1️⃣ Problems Without Concurrency Control

### Lost Update Problem

> Two transactions read the same data and update it, but one update overwrites the other, causing the first update to be lost entirely.

```text
T1 reads Balance = 1000
T2 reads Balance = 1000
T1 writes Balance = 1000 + 500 = 1500
T2 writes Balance = 1000 + 300 = 1300   ← T1's update is lost!
```

### Dirty Read Problem

> A transaction reads data that has been modified by another transaction that **hasn't been committed yet** — if that transaction rolls back, the read data was never valid.

```text
T1 updates Balance to 2000 (not yet committed)
T2 reads Balance = 2000
T1 rolls back → Balance reverts to original value
T2 has now read a value that never actually existed (dirty read)
```

### Unrepeatable Read Problem

> A transaction reads the same row twice and gets **different values** each time, because another transaction modified the data in between.

```text
T1 reads Balance = 1000
T2 updates Balance to 1500 and commits
T1 reads Balance again = 1500   ← Different value within the same transaction!
```

### Interview Shortcut

> **Lost Update = overwritten changes. Dirty Read = reading uncommitted data. Unrepeatable Read = same query, different results.**

---

# 2️⃣ Lock-Based Concurrency Control

### Definition

> Lock-Based Protocols use **locks** to control access to data items, ensuring that only permitted transactions can read or write a locked item at any given time.

### Types of Locks

**🔹 Shared Lock (Read Lock / S-Lock)**
```text
✔ Allows multiple transactions to read the same data simultaneously
✔ Does NOT allow any transaction to write while the lock is held
```

**🔹 Exclusive Lock (Write Lock / X-Lock)**
```text
✔ Allows only ONE transaction to read AND write the data
✔ No other transaction can read or write until the lock is released
```

### Lock Compatibility

| | Shared (S) | Exclusive (X) |
| - | ------------ | ---------------- |
| **Shared (S)** | ✔ Compatible | ✘ Not Compatible |
| **Exclusive (X)** | ✘ Not Compatible | ✘ Not Compatible |

### Interview Shortcut

> **Shared Lock = many can read, none can write. Exclusive Lock = one can read+write, no one else can touch it.**

---

# 3️⃣ Two Strategies: Pessimistic vs Optimistic Locking

### Pessimistic Locking

> Assumes conflicts are **likely** — locks the data item for the entire duration it's being used, preventing other transactions from accessing it until the lock is released.

```text
✔ Guarantees safety — no conflicting access possible
✘ Other transactions must wait, reducing concurrency/performance
✘ Risk of deadlock if not managed carefully
```

### Optimistic Locking

> Assumes conflicts are **rare** — does NOT lock data upfront. Instead, it checks for conflicts only at the time of committing, and resolves them if they occur.

```text
✔ Higher concurrency — no waiting for locks
✔ Better performance when collisions are infrequent
✘ Transactions may need to retry if a collision is detected at commit time
```

### Interview Shortcut

> **Pessimistic = lock first, ask later. Optimistic = ask later, lock never (just verify at the end).**

---

# 4️⃣ Two-Phase Locking (2PL) Protocol

### Definition

> 2PL is a concurrency control protocol where a transaction's locking activity is divided into two distinct phases — **Growing** and **Shrinking** — ensuring serializability.

### The Two Phases

```text
Growing Phase    → Transaction may acquire locks, but cannot release any
Shrinking Phase  → Transaction may release locks, but cannot acquire any new ones
```

### Visual Idea

```text
Number of Locks Held
   │
   │        ___________
   │       /            \
   │      /              \
   │     /                \
   │____/                  \____
   └─────────────────────────────── Time
        Growing Phase   Shrinking Phase
              ↑
        Lock Point (max locks held)
```

### Interview Shortcut

> **2PL = acquire all locks first (growing), then release them all (shrinking). Never mix the two.**

---

# 5️⃣ Deadlock

### Definition

> A Deadlock occurs when two or more transactions are each waiting for the other to release a lock, resulting in all of them being stuck forever with no progress possible.

### Example

```text
T1 holds a lock on Item A, waits for Item B
T2 holds a lock on Item B, waits for Item A

→ Neither can proceed. They wait on each other indefinitely.
```

### Handling Deadlocks

```text
✔ Deadlock Prevention  → Ensure the system never enters a deadlock state (e.g., ordering resources)
✔ Deadlock Detection     → Periodically check for deadlocks using a "wait-for graph"
✔ Deadlock Recovery        → Abort one of the transactions involved to break the cycle
```

### Interview Shortcut

> **Deadlock = circular waiting. Solved by prevention, detection, or recovery (abort one transaction).**

---

# 6️⃣ Livelock

### Definition

> Livelock is similar to deadlock, but instead of being stuck waiting, the transactions keep **actively changing state** in response to each other without making any actual progress.

### Example

```text
The system keeps selecting the same transaction for rollback
repeatedly, so that transaction never gets to finish executing —
it's "alive" and active, but never actually completes.
```

### Interview Shortcut

> **Livelock = transactions stay busy but never progress (unlike deadlock, where they're frozen).**

---

# 7️⃣ Timestamp-Based Concurrency Control

### Definition

> Instead of using locks, this method assigns a unique **timestamp** to each transaction (usually based on start time), and uses these timestamps to decide the order of conflicting operations — eliminating the possibility of deadlock entirely.

### Rules

✔ Each data item maintains a Read-Timestamp and Write-Timestamp

✔ Older transactions (smaller timestamp) generally get priority

✔ No locks used → deadlock is impossible

### Problems It Can Face

```text
Late Read  → A transaction tries to read a value already overwritten by a younger transaction
Late Write → A transaction tries to write a value already read by a younger transaction

Solution: Roll back the transaction and assign it a new timestamp.
```

### Interview Shortcut

> **Timestamp Ordering = no locks, uses transaction age to resolve conflicts. Eliminates deadlock by design.**

---

# ⚖️ Lock-Based vs Timestamp-Based Concurrency Control

| Feature | Lock-Based | Timestamp-Based |
| -------- | ------------ | ------------------- |
| Mechanism | Uses Shared/Exclusive locks | Uses transaction timestamps |
| Deadlock Possible? | Yes | No |
| Overhead | Lock management overhead | Timestamp comparison overhead |
| Rollback Frequency | Lower (waits instead) | Higher (rolls back on conflict) |

---

# 📌 Quick Revision

| Concept | Core Idea |
| --------- | ----------- |
| Lost Update | One transaction's update overwritten by another |
| Dirty Read | Reading data that hasn't been committed yet |
| Unrepeatable Read | Same query gives different results within one transaction |
| Shared Lock | Multiple readers allowed, no writers |
| Exclusive Lock | Only one transaction can read/write |
| Pessimistic Locking | Lock early, prevent conflicts |
| Optimistic Locking | Check for conflicts only at commit time |
| 2PL | Growing phase (acquire) then Shrinking phase (release) |
| Deadlock | Circular waiting, no progress possible |
| Livelock | Constant activity, but no real progress |
| Timestamp Ordering | Resolves conflicts using transaction age, no locks |

---

# 🎤 Viva Questions

### What is Concurrency Control in DBMS?

> A set of techniques used to manage simultaneous access to data by multiple transactions, ensuring data consistency and preventing conflicts.

### What is a Lost Update problem?

> A scenario where two transactions read the same data and update it, but one transaction's update overwrites the other's, causing data to be lost.

### What is a Dirty Read?

> When a transaction reads data that was modified by another transaction that hasn't been committed yet — if that transaction is rolled back, the read data was never actually valid.

### What is the difference between a Shared Lock and an Exclusive Lock?

> A Shared Lock allows multiple transactions to read data simultaneously but blocks writing. An Exclusive Lock allows only one transaction to both read and write, blocking all other access.

### What is the difference between Pessimistic and Optimistic Locking?

> Pessimistic Locking locks data upfront to prevent any possible conflict, reducing concurrency. Optimistic Locking allows transactions to proceed without locking and only checks for conflicts at commit time, offering higher concurrency.

### What is the Two-Phase Locking (2PL) Protocol?

> A protocol where a transaction's locking is split into a Growing Phase (acquiring locks only) and a Shrinking Phase (releasing locks only), which together guarantee serializability.

### What is a Deadlock?

> A situation where two or more transactions are each waiting for the other to release a lock, resulting in all of them being permanently stuck.

### What is the difference between Deadlock and Livelock?

> In a Deadlock, transactions are completely stuck and inactive. In a Livelock, transactions remain active and keep changing state, but still never make actual progress.

### How does Timestamp-Based Concurrency Control avoid deadlock?

> Since it doesn't use locks at all and instead resolves conflicts using the transaction's timestamp (age), there's no possibility of circular waiting, which eliminates deadlock entirely.

### What are the three approaches to handling deadlocks?

> Deadlock Prevention (avoid entering a deadlock state), Deadlock Detection (periodically check using a wait-for graph), and Deadlock Recovery (abort one of the involved transactions).

---

## 🏆 One-Line Summary

```text
Shared Lock         → Many can read, none can write

Exclusive Lock        → One transaction, full read+write access

Pessimistic Locking      → Lock early, prevent conflicts upfront

Optimistic Locking         → Check for conflicts only at commit time

2PL                           → Growing phase (acquire) → Shrinking phase (release)

Deadlock                        → Circular waiting, no progress

Timestamp Ordering                 → No locks, resolves conflicts via transaction age
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
