postgres=# CREATE DATABASE testdb;
CREATE DATABASE
postgres=# \l
List of databases
Name | Owner | Encoding | Locale Provider | Collate | Ctype | Locale | ICU Rules | Access privileges  
-----------+----------+----------+-----------------+---------+-------+--------+-----------+-----------------------
postgres | postgres | UTF8 | libc | C | C | | |
template0 | postgres | UTF8 | libc | C | C | | | =c/postgres +
| | | | | | | | postgres=CTc/postgres
template1 | postgres | UTF8 | libc | C | C | | | =c/postgres +
| | | | | | | | postgres=CTc/postgres
testdb | postgres | UTF8 | libc | C | C | | |
(4 rows)

postgres=# \c testdb
You are now connected to database "testdb" as user "postgres".
testdb=# CREATE TABLE students(
testdb(# id SERIAL PRIMARY KEY,
testdb(# name TEXT,
testdb(# age INT
testdb(# );
CREATE TABLE
testdb=# \dt
List of tables
Schema | Name | Type | Owner  
--------+----------+-------+----------
public | students | table | postgres
(1 row)

testdb=# INSERT INTO student (name, age)
testdb-# VALES ('Omkar', 21);
ERROR: syntax error at or near "VALES"
LINE 2: VALES ('Omkar', 21);
^
testdb=# INSERT INTO student (name, age)
VALUES ('Omkar', 21);
ERROR: relation "student" does not exist
LINE 1: INSERT INTO student (name, age)
^
testdb=# INSERT INTO students (name, age)
VALUES ('Omkar', 21);
INSERT 0 1
testdb=# \dt
List of tables
Schema | Name | Type | Owner  
--------+----------+-------+----------
public | students | table | postgres
(1 row)

testdb=# SELECT \* FROM students;
id | name | age
----+-------+-----
1 | Omkar | 21
(1 row)

testdb=# INSERT INTO students (name, age)
testdb-# VALUES ('SUTAR', 20);
INSERT 0 1
testdb=# SELECT \* FROM students;
id | name | age
----+-------+-----
1 | Omkar | 21
2 | SUTAR | 20
(2 rows)

testdb=# INSERT INTO students (name, age)
testdb-# VALUES ('Bhatkar', 22);
INSERT 0 1
testdb=# INSERT INTO students (name, age)
testdb-# VALUES ('Tushar', 20),
testdb-# VALUES ('Samarth', 21),
testdb-# VALUES ('Pranav', 21);
ERROR: syntax error at or near "VALUES"
LINE 3: VALUES ('Samarth', 21),
^
testdb=# INSERT INTO students (name, age)
VALUES ('Tushar', 20),
('Samarth', 21),
('Pranav', 21);
INSERT 0 3
testdb=# SELECT \* FROM students;
id | name | age
----+---------+-----
1 | Omkar | 21
2 | SUTAR | 20
3 | Bhatkar | 22
4 | Tushar | 20
5 | Samarth | 21
6 | Pranav | 21
(6 rows)

testdb=# DELETE FROM students
testdb-# WHERE id = 5;
DELETE 1
testdb=# SELECCT _ FROM students
testdb-# ;
ERROR: syntax error at or near "SELECCT"
LINE 1: SELECCT _ FROM students
^
testdb=# SELECT \* FROM students
;
id | name | age
----+---------+-----
1 | Omkar | 21
2 | SUTAR | 20
3 | Bhatkar | 22
4 | Tushar | 20
6 | Pranav | 21
(5 rows)

testdb=# DELETE FROM students
testdb-# WHERE id = 4,
testdb-# id = 3;
ERROR: syntax error at or near ","
LINE 2: WHERE id = 4,
^
testdb=# DELETE FROM students
WHERE id = 4, 3;
ERROR: syntax error at or near ","
LINE 2: WHERE id = 4, 3;
^
testdb=# DELETE FROM students
WHERE id = 4,3;
ERROR: syntax error at or near ","
LINE 2: WHERE id = 4,3;
^
testdb=# SELECT \* FROM students
testdb-# ;
id | name | age
----+---------+-----
1 | Omkar | 21
2 | SUTAR | 20
3 | Bhatkar | 22
4 | Tushar | 20
6 | Pranav | 21
(5 rows)

testdb=# UPDATE students
testdb-# SET age = 22
testdb-# WHERE id = 6;
UPDATE 1
testdb=# SELECT \* FROM students;
id | name | age
----+---------+-----
1 | Omkar | 21
2 | SUTAR | 20
3 | Bhatkar | 22
4 | Tushar | 20
6 | Pranav | 22
(5 rows)

testdb=# UPDATE students
testdb-# SET age = 22
testdb-# WHERE age = 20;
UPDATE 2
testdb=# SELECT \* FROM students
testdb-# ;
id | name | age
----+---------+-----
1 | Omkar | 21
3 | Bhatkar | 22
6 | Pranav | 22
2 | SUTAR | 22
4 | Tushar | 22
(5 rows)

testdb=# UPDATE students
testdb-# SET name = 'Omkar Patkar',
testdb-# age = 22
testdb-# WHERE id = 1;
UPDATE 1
testdb=# SELECT \* FROM students
testdb-# ;
id | name | age
----+--------------+-----
3 | Bhatkar | 22
6 | Pranav | 22
2 | SUTAR | 22
4 | Tushar | 22
1 | Omkar Patkar | 22
(5 rows)

testdb=# SELECT \* FROM students
testdb-# ORDER by name;
id | name | age
----+--------------+-----
3 | Bhatkar | 22
1 | Omkar Patkar | 22
6 | Pranav | 22
2 | SUTAR | 22
4 | Tushar | 22
(5 rows)

testdb=# SELECT \* FROM students
ORDER by id;
id | name | age
----+--------------+-----
1 | Omkar Patkar | 22
2 | SUTAR | 22
3 | Bhatkar | 22
4 | Tushar | 22
6 | Pranav | 22
(5 rows)

testdb=# UPDATE students
testdb-# SET age = 21
testdb-# WHERE age = 22;
UPDATE 5
testdb=# SELECT \* FROM students
ORDER by id;
id | name | age
----+--------------+-----
1 | Omkar Patkar | 21
2 | SUTAR | 21
3 | Bhatkar | 21
4 | Tushar | 21
6 | Pranav | 21
(5 rows)

testdb=# UPDATE students
testdb-# SET name = 'Nikhil'
testdb-# WHERE id = 2;
UPDATE 1
testdb=# SELECT \* FROM students
ORDER by id;
id | name | age
----+--------------+-----
1 | Omkar Patkar | 21
2 | Nikhil | 21
3 | Bhatkar | 21
4 | Tushar | 21
6 | Pranav | 21
(5 rows)

testdb=# SELECT _ FROM student
testdb-# LIMIT 3;
ERROR: relation "student" does not exist
LINE 1: SELECT _ FROM student
^
testdb=# SELECT \* FROM students
LIMIT 3;
id | name | age
----+---------+-----
3 | Bhatkar | 21
6 | Pranav | 21
4 | Tushar | 21
(3 rows)

testdb=# SELECT \* FROM students
testdb-# ORDER BY id DESC
testdb-# LIMIT 3;
id | name | age
----+---------+-----
6 | Pranav | 21
4 | Tushar | 21
3 | Bhatkar | 21
(3 rows)

testdb=# SELECT COUNT(\*) FROM students;
count

---

     5

(1 row)

testdb=# SELECT COUNT(\*) FROM students
testdb-# WHERE age = 21;
count

---

     5

(1 row)

testdb=#
testdb=#
testdb=# \q
