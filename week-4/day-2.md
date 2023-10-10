## SQL
### Functions
- Arithmetic Functions - operate on multiple data records
    - Aggregate Functions
        - AVG
        - COUNT
        - MAX
        - MIN
        - SUM
- Scalar Functions - operate on a single data record
    - CHAR
    - DATA
    - DAY
    - SUBSTR
    - Many More
- DISTINCT clause - eliminate duplicates in a SELECT output
- COALESCE
    - Imagine we want to select salary from employees
    - What if an employee's salary is NULL
        - Then the total salary could be 0
    - We can use COALESCE to replace NULL values with a particular value
    - COALESCE(colname, val)
        - COALESCE(COMM,0)
        - COALESCE(COMM,100)
        - COALESCE(COMM,1)
    - Not changing anything in the table, just modifying the results of the SELECT statement

### SELECT Revisited
- WHERE - filter records based on conditions
- GROUP BY - group records depending on duplicates of some other column
    - Kind of like SORT SUM fields from SORT utility
    - SELECT deptno, SUM(Salary) FROM emp GROUP BY Deptno;
        - display total salary paid for each department
- HAVING - apply a condition on the grouped up value (GROUP BY)
    - SELECT deptno, SUM(Salary) FROM emp GROUP BY Deptno HAVING SUM(Salary) > 10000;
    - If not using GROUP BY, cannot have HAVING clause
- ORDER BY - sort the records in some particular order
    - Will not sort the records in the table
    - It will just order the results of the SELECT statement
    - SELECT column_names FROM tablename ORDER BY columnname ASC/DESC
        - Default is ascending
    - Don't necessarily need to sort by columns you are selecting
- FETCH - limit your output to the first N rows
    - FETCH FIRST 2 ROWS ONLY;
    - No LAST option
#### Column Aliases
- Gives a column an alternative name for that attribute in the output
- Scope of the alias name is only within that SQL statement
- SELECT EMPNO as "EMPLOYEE NUMBER"
    - Need quotes if there is a space
- Don't need AS keyword for it to work
#### Concatenation Operator
- can use ||
- or use concat(col1, col2)
- SELECT first_name||last_name NICKNAME FROM EMPLOYEE;
    - If using char, might have spaces
### Complex SQL
- Retrieving data from more than one table
- A table can be referred to as more than one table by giving it more than alias name
- Imagine, want to know who is working for the department for which "Alwyn" is working
- Imagine, we want to the HOD of Tommy
    - Information that is from Employee and Department number
    - Where do we make this connection?
    - Find Department number from employee table and then finding HOD from department table
#### Sub Queries 
- Nested SELECT statement
- A Query inside a query
- Specified using the IN predicate, equality, comparative operators
- Nested Loop
- The known value query must be the inner query
- The expected columns should be the outer query

##### Correlated
- Top-Bottom
##### Non-Correlated
- 16 levels
- Bottom-Up
- ex: 
    - PROJ DETAILS?
    - KNOWN ? NAME
    - SELECT PROJLEAD,PROLOC FROM PROJ WHERE PID = 
        (
            SELECT PROJID FROM EMP WHERE ENAME='TIMMY'
        )
    - Display the second max Salary ?
        - SELECT MAX(SAL) FROM EMP where sal < (SELECT MAX(SAL) FROM EMP);
    - Display the third max Salary ?
        - SELECT MAX(SAL) FROM EMP WHERE SAL < (SELECT MAX(SAL) FROM EMP where sal < (SELECT MAX(SAL) FROM EMP));
    - Now, get all the data about that employee:
        - SELECT ENAME, EID, SAL FROM EMP WHERE SAL = (SELECT MAX(SAL) FROM EMP WHERE SAL < (SELECT MAX(SAL) FROM EMP where sal < (SELECT MAX(SAL) FROM EMP)));
#### Joins
#### Unions