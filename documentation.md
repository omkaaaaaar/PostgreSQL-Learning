# PostgreSQL Basics – Intern-Ready Notes (From Zero to DELETE)

> These notes are based on my **hands-on learning journey for an internship** with the stack:
>
> - Django Backend
> - PostgreSQL Database
> - Next.js Frontend
>
> Goal: Become **intern-ready**, not a database expert.

---

## 1. What is PostgreSQL and `psql`?

- **PostgreSQL** is a **database** that stores data in tables.
- **`psql`** is the **terminal tool** to talk to PostgreSQL.
- You don’t click files or folders — you use **commands**.

Think of it like this:

- Python → You work with `.py` files and folders
- PostgreSQL → You work with **commands and tables**

---

## 2. Where Are Databases Stored?

📌 **Important Realization (Q&A from learning):**

**Q:** Where does the database get stored? Like Python files?

**Answer I understood:**

- PostgreSQL stores data **inside the system on its own**
- More particularly at:

```
/Library/PostgreSQL/18/data/
```

- Data is stored in **PostgreSQL’s own binary format**
- It is **not human-readable like `.txt` or `.db` files**
- We should **never manually edit those files**

---

## 3. Entering PostgreSQL (`psql`)

From normal terminal:

```bash
psql -U postgres
```

If successful, you will see:

```
postgres=#
```

This means:
✅ You are now **inside PostgreSQL**

---

## 4. Basic `psql` Helper Commands

These are NOT SQL queries — they are **psql shortcuts**.

| Command     | Meaning                         |
| ----------- | ------------------------------- |
| `\l`        | List all databases              |
| `\c dbname` | Connect to a database           |
| `\dt`       | List tables in current database |
| `\q`        | Exit PostgreSQL                 |

---

## 5. Creating a Database

```sql
CREATE DATABASE testdb;
```

Check if it exists:

```sql
\l
```

Connect to it:

```sql
\c testdb
```

Now the prompt becomes:

```
testdb=#
```

✅ You are inside your own database now.

---

## 6. Creating a Table

```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT,
    age INT
);
```

Expected output:

```
CREATE TABLE
```

Check tables:

```sql
\dt
```

---

## 7. Understanding Table Columns (Important Q&A)

### `id SERIAL PRIMARY KEY`

- `id` → column name
- `SERIAL` → auto-increment number (1, 2, 3…)
- `PRIMARY KEY` → unique identifier, cannot be empty

### `name TEXT`

- Stores names (strings)

### `age INT`

- Stores whole numbers only

### Graphical View of Table

```
+----+------+-----+
| id | name | age |
+----+------+-----+
```

---

## 8. Inserting One Row

```sql
INSERT INTO students (name, age)
VALUES ('Omkar', 21);
```

Output:

```
INSERT 0 1
```

### Meaning of `INSERT 0 1`

📌 **Important Q&A:**

**Q:** What does `INSERT 0 1` mean?

**A:**

- The number `1` means → ✅ **1 row was inserted successfully**
- The `0` can be ignored at beginner level

---

## 9. Viewing Data

```sql
SELECT * FROM students;
```

Example Output:

```
 id |  name  | age
----+--------+-----
  1 | Omkar  |  21
```

---

## 10. Inserting Multiple Rows

✅ Correct Way:

```sql
INSERT INTO students (name, age)
VALUES
('Tushar', 20),
('Samarth', 21),
('Pranav', 21);
```

Output:

```
INSERT 0 3
```

---

## 11. Common Mistake While Inserting Multiple Rows

❌ Wrong (What I did first):

```sql
VALUES ('Tushar', 20),
VALUES ('Samarth', 21),
VALUES ('Pranav', 21);
```

✅ Correct Rule:

- `VALUES` is written **only once**
- Multiple rows are separated by commas

---

## 12. Final Table After All Inserts

```text
 id |  name   | age
----+---------+-----
  1 | Omkar   |  21
  2 | SUTAR   |  20
  3 | Bhatkar |  22
  4 | Tushar  |  20
  5 | Samarth |  21
  6 | Pranav  |  21
```

---

## 13. Deleting a Row

```sql
DELETE FROM students
WHERE id = 2;
```

Output:

```
DELETE 1
```

Check again:

```sql
SELECT * FROM students;
```

---

## 14. Important Realization About SERIAL IDs (Q&A)

**Q:** The row is deleted, but why didn’t the ID numbers change?

**Answer I learned:**

- `SERIAL` uses a **counter (sequence)**
- Once a number is used, it is **never reused**
- IDs **do not rearrange after DELETE**

This is done to prevent:

- Data corruption
- Broken references in real-world systems

---

## ✅ What I Have Learned Until DELETE

- ✅ Enter PostgreSQL using `psql`
- ✅ Create databases
- ✅ Create tables
- ✅ Understand columns and data types
- ✅ Insert one row
- ✅ Insert multiple rows
- ✅ Read data using `SELECT`
- ✅ Delete a row
- ✅ Understand how `SERIAL` really works

---

## 🎯 Next Step (Not Included Here)

- UPDATE
- WHERE
- ORDER BY

These will complete the **intern-ready PostgreSQL core**.
