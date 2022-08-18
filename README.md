# SQL Test

Test your SQL skills with this SQL test

## Instructions

Follow [this](https://www.sqlitetutorial.net/download-install-sqlite/) tutorial to install SQLite and create an empty database.

Open your favourite SQL editor to query the database you created above.

## Questions

### Write sql query to get the second highest salary among all employees?

Given Employee Table with two columns ID, Salary 10, 2000 11, 5000 12, 3000

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID, Salary) AS (
    VALUES (10, 2000), (11, 5000), (12, 3000)
)
SELECT
    MAX(Salary)
FROM Employee WHERE Salary <> (SELECT MAX(Salary) FROM Employee);
```
</details>

### Write sql query to find max salary and department name from each department.

Given Employee table with three columns ID, Salary, DeptID 10, 1000, 2 20, 5000, 3 30, 3000, 2

<details>
    <summary>Solution</summary>

```sql
WITH Employee (ID, Salary, DeptID) AS (
    VALUES (10, 1000, 2), (20, 5000, 3), (30, 3000, 2)
),
Department (ID, DeptName) AS (
    VALUES (1, 'Marketing'), (2, 'IT'), (3, 'Finance')
)
SELECT
    d.DeptName
    , MAX(e.Salary)
FROM Department d
LEFT OUTER JOIN Employee e ON e.DeptId = d.ID
GROUP BY DeptName;
```
</details>

### Write sql query to find records in table a that are not in table b without using not in operator.

<details>
    <summary>Solution</summary>

```sql
WITH table_a (ID) AS (
    VALUES (10), (20), (30)
)
, table_b (ID) AS (
    VALUES (15), (30), (45)
)
SELECT
    tbl_a.ID
FROM table_a AS tbl_a
LEFT JOIN table_b AS tbl_b ON (tbl_a.ID = tbl_b.ID)
WHERE tbl_b.ID IS NULL;
```
</details>

### What is the result of following query?

```sql
SELECT CASE WHEN null = null THEN "True" ELSE "False" END AS Result;
```

<details>
    <summary>Solution</summary>

    <p>
    In SQL null can not be compared with itself. There fore null = null is not true. We can compare null with a non-null value to check whether a value is not null. Therefore the result of above query is False. The correct way to check for null is to use IS NULL clause. Following query will give result True.
    </p>

```sql
SELECT CASE WHEN null IS NULL THEN "True" ELSE "False" END AS Result;
```
</details>

### Write sql query to find employees that have same name and email.

Employee table: ID NAME EMAIL 10 John jbaldwin 20 George gadams 30 John jsmith

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID, NAME, EMAIL) AS (
    VALUES (10, 'John', 'jbaldwin'), (20, 'George', 'gadmas'), (30, 'John', 'jsmith')
)
SELECT
    name
    , email
    , COUNT(*)
FROM Employee
GROUP BY
    name
    , email
HAVING
    COUNT(*) > 1;
```
</details>

### Write sql query to find max salary from each department.

Given Employee table with three columns ID, Salary, DeptID 10, 1000, 2 20, 5000, 3 30, 3000, 2

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID, Salary, DeptID) AS (
    VALUES (10, 1000, 2), (20, 5000, 3), (30, 3000, 2)
)
SELECT
    DeptID
    , max(Salary) AS max_salary
FROM employee
GROUP BY
    DeptID
ORDER BY
    max_salary DESC;
```
</details>

### Write sql query to get the nth highest salary among all employees.

Given Employee Table with two columns ID, Salary 10, 2000 11, 5000 12, 3000

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID, Salary) AS (
    VALUES (10, 2000), (11, 5000), (12, 3000)
)
SELECT *
FROM employee emp1
WHERE (N-1) = (
    SELECT
        COUNT(DISTINCT(emp2.salary))
    FROM employee emp2
    WHERE emp2.salary > emp1.salary
);
```
</details>

### How can you find 10 employees with odd number as employee id?

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID) AS (
    VALUES (1), (2), (3), (4), (5),
        (6), (7), (8), (9), (10),
        (11), (12), (13), (14), (15),
        (16), (17), (18), (19), (20), (21),
        (22), (23), (24), (25), (26), (27),
        (28), (29), (30)
)
SELECT
    ID
FROM employee
WHERE ID%2 <> 0
LIMIT 10;
```
</details>

### Write sql query to get the names of employees whose date of birth is between 01/01/1990 to 31/12/2000.

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID, Name, DOB) AS (
    VALUES (1, 'John', date('1990-10-09')), (2, 'Jane', date('2005-07-06')), (3, 'Max', date('1995-04-02')), (4, 'Jim', date('1899-2-23'))
)
SELECT
    Name, date(DOB)
FROM employee
WHERE DOB BETWEEN date('1990-01-01') AND date('2000-12-31');
```
</details>

### Write sql query to get the quarter from date.

<details>
    <summary>Solution</summary>

```sql
WITH dates (date) AS (
    VALUES (date('2005-02-06')), (date('2005-04-06')), (date('2005-08-06')), (date('2005-11-06'))
)
SELECT
    CASE
        WHEN CAST(strftime('%m', d.date) AS INTEGER) BETWEEN 01 AND 03 THEN 1
        WHEN CAST(strftime('%m', d.date) AS INTEGER) BETWEEN 04 AND 06 THEN 2
        WHEN CAST(strftime('%m', d.date) AS INTEGER) BETWEEN 07 AND 09 THEN 3
        WHEN CAST(strftime('%m', d.date) AS INTEGER) BETWEEN 10 AND 12 THEN 4
        ELSE -1
    END AS quarter
FROM dates AS d;
```
</details>

### Write query to find employees with duplicate email.

Employee table: ID NAME EMAIL 10 John jsmith 20 George gadams 30 Jane jsmith

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID, NAME, EMAIL) AS (
    VALUES (10, 'John', 'jsmith'), (20, 'George', 'gadmas'), (30, 'Jane', 'jsmith')
)
SELECT
    NAME
    , COUNT(EMAIL)
FROM Employee
GROUP BY
    EMAIL
HAVING
    COUNT(EMAIL) > 1;
```
</details>

### Write a query to find all employee whose name contains the word "rich", regardless of case.

E.g. Rich, RICH, rich.

<details>
    <summary>Solution</summary>

```sql
WITH employee (ID, NAME) AS (
    VALUES (10, 'Richard'), (20, 'RICHARD'), (30, 'richard'), (40, 'Richy Rich'), (50, 'Bob')
)
SELECT *
FROM employee
WHERE UPPER(NAME) like "%RICH%";
```
</details>
