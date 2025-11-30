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

## 15. Deleting MULTIPLE Rows (Safe Way)

✅ You **can delete multiple rows at once** — but only using a `WHERE` condition.

### Example 1: Delete everyone with `age = 21`

```sql
DELETE FROM students
WHERE age = 21;
```

If 3 students match, output will be:

```
DELETE 3
```

Meaning:

- ✅ 3 rows were deleted
- ✅ Table still exists

---

### Example 2: Delete using a range (greater than)

```sql
DELETE FROM students
WHERE age > 22;
```

This deletes **all students older than 22**.

---

### ⚠️ Dangerous Command (Never Run by Mistake)

```sql
DELETE FROM students;
```

This will:

- ❌ Delete **ALL rows** from the table
- ✅ Keep the table structure
- ❌ `SELECT * FROM students;` → will show empty

**Golden Rule:**

> ✅ Always use `DELETE ... WHERE ...`

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

## 15. Updating Existing Data (UPDATE)

To change data in an existing row:

```sql
UPDATE students
SET age = 25
WHERE id = 1;
```

Meaning:

- `UPDATE students` → choose the table
- `SET age = 25` → change the value
- `WHERE id = 1` → apply change to only one specific row

Check result:

```sql
SELECT * FROM students;
```

---

## 16. Multiple Updates (Rows & Columns)

### A) Update MULTIPLE ROWS at once

Use a condition that matches many rows:

```sql
UPDATE students
SET age = 22
WHERE age = 21;
```

If 3 students match:

```
UPDATE 3
```

---

### B) Update MULTIPLE COLUMNS in one row

Change name **and** age together:

```sql
UPDATE students
SET name = 'Omkar Patkar',
    age = 24
WHERE id = 1;
```

---

### ⚠️ Dangerous Update (Never run by mistake)

```sql
UPDATE students
SET age = 30;
```

This updates **EVERY row** in the table.

**Golden Rule:**

> ✅ Always use `UPDATE ... WHERE ...`

---

## 16. Filtering Data with WHERE

### Get students with age greater than 20

```sql
SELECT * FROM students
WHERE age > 20;
```

### Get student with a specific name

```sql
SELECT * FROM students
WHERE name = 'Omkar';
```

---

## 17. Sorting Data with ORDER BY

### Sort by age (ascending)

```sql
SELECT * FROM students
ORDER BY age;
```

### Sort by age (descending)

```sql
SELECT * FROM students
ORDER BY age DESC;
```

---

## 18. Why Row Order Changes After UPDATE / DELETE (Very Important)

📌 **Real Observation from Practice:**

After running an `UPDATE`, the result of:

```sql
SELECT * FROM students;
```

appeared in a different order like:

```text
 3 | Bhatkar
 6 | Pranav
 2 | SUTAR
 4 | Tushar
 1 | Omkar Patkar
```

This is **NOT a bug** and **NOT caused by UPDATE directly**.

---

### ✅ The Real Reason

> **SQL tables are UNORDERED by default.**

If you do not use `ORDER BY`, PostgreSQL is free to return rows in **any order** it finds fastest.

---

### 🧠 Why This Happens (Simple Intern-Level Explanation)

- Data is stored inside **disk blocks**
- When you run:

  - INSERT
  - DELETE
  - UPDATE
    PostgreSQL may:

- Move rows internally
- Reorganize storage
- Fetch rows in a different physical sequence

So the database thinks:

> “You didn’t say HOW to sort, so I’ll return rows however it’s convenient.”

---

### ✅ The Correct Way to Always Get Sorted Data

By `id` (normal order):

```sql
SELECT * FROM students
ORDER BY id;
```

By `id` (latest first):

```sql
SELECT * FROM students
ORDER BY id DESC;
```

By `name`:

```sql
SELECT * FROM students
ORDER BY name;
```

---

### ✅ Golden Rule for Real Projects

> ❗ **Never trust default `SELECT *` order**
> ✅ Always use `ORDER BY` when order matters (UI, APIs, reports)

---

## ✅ What I Have Learned Till UPDATE, WHERE & ORDER BY (Intern-Ready Core)

- ✅ Create databases and tables
- ✅ Insert one and multiple rows
- ✅ Read data using SELECT
- ✅ Update existing data
- ✅ Update multiple rows and columns
- ✅ Filter data using WHERE
- ✅ Sort data using ORDER BY
- ✅ Delete single and multiple rows safely
- ✅ Understand how SERIAL IDs behave
- ✅ Understand why row order changes without ORDER BY

---

## 🚀 PostgreSQL Status: INTERN-READY

At this point, I can:

- Verify Django data manually using `psql`
- Debug wrong API data using `SELECT`
- Fix incorrect data using `UPDATE`
- Remove bad data using `DELETE`
- Understand what teammates mean by table, row, primary key, and filter queries

This is **everything required for Internship Database Usage** ✅

---

with this I learned one more key skill for **Data Management**,

### POSTGRESQL - Data modeling, CRUD, filtering & sorting

---

## TO integrate Postgres in Django

1. create a db in pgadmin4 databse folder

2. In django main app, in settings.py; paste ->

```
DATABASES = {
    # "default": {
    #     "ENGINE": "django.db.backends.sqlite3",
    #     "NAME": BASE_DIR / "db.sqlite3",
    # }
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'db_name',
        'USER': 'db_admin_name',
        'PASSWORD': 'db_admin_password',
        'HOST': 'localhost',
        'PORT': '',
}
}
```

3. Make Migrations then in terminal:

```
python manage.py makemigrations (optional 1st line, research first to use ts ln!)
python manage.py migrate
```

This will migrate models.py model into the postgresdb

4. To acess db:

```
python manage.py dbshell
```

5. inside terminal:

```
chaidb#= \dt
```

Ts display all the tables in the particular db

## postgres is integrated in Django and the migrations are made!!!!
