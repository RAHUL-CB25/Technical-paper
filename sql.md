# SQL Technical Notes
## 1. ACID
ACID makes sure database transactions work safely.

- Atomicity → All operations happen, or none happen.
- Consistency → Data stays correct.
- Isolation → Transactions work separately without causing problems.
- Durability → Saved data is not lost after a failure.

## 2. CAP Theorem
CAP is mainly used when talking about distributed databases.
* Consistency → Every user gets the latest data.
* Availability → System keeps responding.
* Partition Tolerance → System continues working even when servers cannot communicate properly.

CAP means **Consistency, Availability, and Partition Tolerance**.

## 3. Joins

JOIN is used to get related data from two or more tables.

```sql
SELECT e.name, d.department
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;
```

Common joins:
* INNER JOIN → matching rows from both tables.
* LEFT JOIN → all rows from left table.
* RIGHT JOIN → all rows from right table.
* FULL JOIN → all rows from both tables.
* CROSS JOIN → every possible combination.
* SELF JOIN → table joined with itself.
```text
INNER → Matching
LEFT  → Left + matching
RIGHT → Right + matching
FULL  → Everything
```

## 4. Aggregate Functions

Used to perform calculations on multiple rows.

* `COUNT()` → counts rows.
* `SUM()` → calculates total.
* `AVG()` → calculates average.
* `MIN()` → smallest value.
* `MAX()` → largest value.

```sql
SELECT AVG(salary)
FROM employees;
```

`GROUP BY` creates groups.

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department;
```

`WHERE` filters rows, while `HAVING` filters groups.

```sql
SELECT department, COUNT(*)
FROM employees
GROUP BY department
HAVING COUNT(*) > 2;
```

`ORDER BY` is used to sort the result.

## 5. Normalization

Normalization is used to organize tables and reduce duplicate data.

- 1NF → Each column should contain a single value, not multiple values.
- 2NF → Every non-key column should depend on the complete primary key.
- 3NF → A non-key column should not depend on another non-key column.



```text
1NF → Atomic values
2NF → No partial dependency
3NF → No transitive dependency
```

## 6. Primary Key and Foreign Key

* Primary Key → Uniquely identifies each row.
* Foreign Key → Connects one table with another table.

Example:

```sql
CREATE TABLE department (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE employee (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES department(id)
);
```

## 7. Index

An index helps the database find data faster.

```sql
CREATE INDEX idx_employee_name
ON employees(name);
```

Indexes can improve `SELECT` performance, but they need extra storage and can slow down `INSERT`, `UPDATE`, and `DELETE`.

## 8. Transactions

A transaction is a group of SQL operations treated as one unit.

```sql
BEGIN;

UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;

UPDATE accounts
SET balance = balance + 1000
WHERE id = 2;

COMMIT;
```

* `BEGIN` → starts transaction.
* `COMMIT` → saves changes.
* `ROLLBACK` → undoes changes.
* `SAVEPOINT` → creates a point for partial rollback.

## 9. Locking

Locking controls how multiple transactions access the same data.

A shared lock is mainly used for reading, while an exclusive lock is used when data is changed.

Example in PostgreSQL:

```sql
SELECT *
FROM accounts
WHERE id = 1
FOR UPDATE;
```

`FOR UPDATE` locks the selected row.

A deadlock can happen when two transactions wait for each other.

```text
Transaction A → Row 1 → Wants Row 2
Transaction B → Row 2 → Wants Row 1
```

## 10. Isolation Levels

Isolation levels control how much one transaction can see from another transaction.

* READ UNCOMMITTED → Can read uncommitted changes.
* READ COMMITTED → Reads only committed data.
* REPEATABLE READ → Same row gives a consistent result during the transaction.
* SERIALIZABLE → Strongest isolation level.

Higher isolation gives more consistency but can reduce concurrency.

## 11. Triggers

A trigger automatically runs when an event happens in a table.

Common events are `INSERT`, `UPDATE`, and `DELETE`.

Example:

```text
UPDATE Employee
       ↓
    Trigger
       ↓
Audit Table
```

Triggers are commonly used for audit logs and automatic actions.

## Reference
1. SQLite Official Documentation — https://www.sqlite.org/docs.html
2. SQLite SQL Language Reference — https://www.sqlite.org/lang.html
3. YouTube — SQL Tutorial for Beginners by Rishabh Mishra: https://www.youtube.com/watch?v=On9eSN3F8w0
