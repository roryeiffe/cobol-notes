## DB2
- So far, we've seen a PS
    - Every line is a record
    - Don't have strict structures
    - We want another way of handling data
    - VSAM was invented
        - ESDS
        - KSDS - key is a column of the record, should be unique and in ASC order
        - RRDS
    - COBOL - can read in KSDS and handle data in variables
        - Must validate the record when it comes in
    - Want to structure data in a table
    - DBMS - a software that lets you store data in a well-structured manner
    - RDBMS - Relation Database Management System

#### Example
- Employee, What fields do we need?
    1. FNAME
    1. MNAME
    1. LNAME
    1. DEPRT
    1. SAL
    1. EMPID
    1. LOC
    1. AINC
    1. BANK AC
    1. CON1
    1. CON2
    1. TADD
    1. PADD
    1. SSN
    1. NATIONALITY
    1. HOMETOWN
    1. LANGUAGE
    1. 10TH GRADE
    1. 12TH GRADE
    1. UG-DEPRT
    1. GRADE
    1. PG-DEPRT
    1. GRADE
    1. SKILLS
    1. HEIGHT
    1. WEIGHT
    1. COMPLEXION
    1. DOB
    1. BIRTH CERTIFICATE
    1. EXP
    1. DOJ
    1. NO. OF DEPENDENTS
    1. STRIKES
    1. BIRTHMARK
    1. MARITAL STATUS
    1. GENDER
 ### Normalization
 - Break one table into smaller tables with common rules so that anyone can follow
 - Dr. E.F. Codd (https://en.wikipedia.org/wiki/Edgar_F._Codd)   
 - Detect Redundancies in a given table
 - It's used to check the data and/or design tables
 - Paper/Pen work, not a program
- Steps:
    - Norm1 - Eliminate Repeating Groups
    - Norm2 - Eliminate all columns that depend only on part of the kay
    - Norm3 - Eliminate columns not dependent on the key at all
- The rules for normalization are cumulative. If a database adheres to Norm3, it must also adhere to Norm1 and Norm2
### Terms:
- "Relation" is just a mathematical term for a table
- Null - unknown, undefined
- Storage Group - (Stogroup) a set of volumes on a single DASD
- Database - contains a Stogroup and a bufferpool
- Bufferpool - a small, predefined temporary memory that's available in a database
- Tablespace - logical address space on secondary storage, can contain one or more tables
    - Simple
    - Partitioned
    - Segmented
- Table - relation
- Record - Tuple/Row
- Attribute - Field/Column
- Domain - range of values if same column

### Codd's 12 rules
- Information Rule
- Guaranteed Access Rule - should not have data that cannot be fetched
- Systematic Treatment of Nulls - should handle null in a proper way
- Physical Data Independence
- Logical Data Independence
- Distribution Independence
- Non-subversion rule

### Database
- Database - ARI001 => ARI001DB
    - Just add DB to end of HLQ
- TableSpace - ARI001 => ARI001TS
    - Just add TS to end of HLQ


### DB2
- DB2 is an abbreviation for IBM DATABASE 2
- it is a subsystem of the z/OS Operating System
- Database - a self describing collection of integrated data
    - Self-Describing
        - NAME: ALWYN
- Table-Like structure
1. Interacting with DB2
    - Using SPUFI (SQL Processing Using File Inputs)
        - Option 9
        - SP - SPUFI 
        - Option 1
        - Enter your PDS and PS names for fields 1 and 4 respectively
            - PDS should have a member name, ex: ARI001.RORY.REVAT.DB2.PDS(SQLIN)
        - Hit enter and you should be taken to the PDS member
        - Enter the SQL code
        - Hit F3 and hit enter to submit
        - Queries are saved in the file that we write them in
    - QMF (Query Management Facility)
        - Option 9
        - QM - QMF
        - Cannot save/store the queries
    - Embedded SQL Program (COBOL)
### SQL - 
- Every SQL statement will return SQLCODE
    - Signed 3 digits
    - From 000 to -999, 000 to +999
    - If we get SQL code 000, operation is successful
    - If sqlcode is negative
        - We have an error, use Google
    - If sqlcode is positive
        - We have a warning or information
        - +100 -> end of table
- Defining, accessing, and managing relational databases
- Flexible
- Sub-languages:
    - DDL - Data Definition Language
    - DML - Data Manipulation Language
    - DCL - Data Control Language
    - DCL - Transaction Control Language
    - DQL - Data Query Language (takes SELECT from DML)
#### DDL
- Create - create schema objects, the structure of the table
- Alter - change schema object
- Drop - delete schema objects
- Truncate - empty the table
- Rename - rename the schema object
#### DML
- Insert - add new rows to the table
- Update - modify values in existing rows
- Delete - remove already existing rows
- Select - retrieve records from the table
#### TCL
- Commit - saves the transaction until that point where commit command is given
- Rollback - like UNDO command, saves only until the previous commit
#### DCL
- Grant - grant privileges/roles
- Revoke - take away privileges
#### Data Types
- Numeric
    - Small Int
    - Decimal
    - Inteer
    - Number
    - Float
    - Real
- String
    - Characters  
        - Char
        - VARCHAR
        - Long Varchar
    - Graphics
        - graphic
        - vargraphic
- Date/Time
- For more info, refer to slides
- Char(05) <- sam, will store 'sam  ', 5 bytes
- Varchar(05) <- sam, will store 'sam', 3 bytes
    - no buffer spaces
### Sub-Language Syntax:
#### DDL
- Create
    - CREATE TABLE/VIEW/TRIGGER/STOGROUP/UNIQUE INDEX -NAME-
        - (
        -    OTHER ATTRIBUTES
        - );
    - CREATE TABLE TB_EMP
        - (
            - 
        - ) IN ARI001DB.ARI001TS
- Every attribute of a table must have
    - The name of the column
    - The data type of the column
    - [Constraints(set of rules that you want to fix for the column)]
        - UNIQUE - no duplicates allowed in the column
        - NOT NULL - no null values allowed in column, demands a value
            - like filling a form with a red star (required)
            - Default - allows null value
                - NOT NULL WITH DEFAULT
                    - It stores that column with default values depending on its data type (0, '')
                - NOT NULL WITH DEFAULT 12345
                    - Accepts value from the user, if the user did not mention any values, the system stores 12345 as the value for that column of the record
        - PK (primary key)
            - any unique column can be defined as PK, for which the unique index must be created, which acts as a pivot access point to the records
            - Note: there can only be 1 PK for a table
        - Foreign Key - referential constraint
        - Check constraint
            - allows the user to give a range of allowable values for the column
            - eg: Gender(m,f,o)
        - Note: For the PK column and for all the unique columns, UNIQUE INDEX MUST BE CREATED. If not created, then 'The definition of the table is incomplete.'
            - CREATE UNIQUE INDEX IDX9 ON TABLENAME(COLUMN-NAME)
- C ALL ' ' '-' 1 2
    - Changes all spaces to columns in columns 1 and 2
#### DML
- Insert
    - INSERT INTO TABLENAME VALUES (VAL1,VAL2,VAL3);
- Update
    - UPDATE TABLENAME SET COLUMNNAME=VALUE/EXPRESSION, COLUMN2=VALUE2/EXPRESSION2;
- Delete
    - DELETE FROM TABLENAME;
        - this statement will remove all records from the table (no WHERE clause)
- SELECT
    - SELECT COL1, COL2, COL3 FROM TABLENAME;
        - this statement will select all records from the table (no WHERE clause)
##### The "WHERE" clause
- Helps us to focus/specify what we are looking for
- Can use with UPDATE, DELETE, SELECT
- Operators
    - BETWEEN
    - LIKE
    - IS NULL
    - IS NOT NULL
    - RELATIONAL OPERATORS (<, >, =, <=, >=)
    - IN
    - LOGICAL OPERATORS (AND, OR NOT)


### Referential Integrity
- TB_DEPT
- DID   HOD   DNAME
- D001
- D002
- D003
- D004
- D005
- D006

- TB_EMPLOYEE
- EID         ENAME            DID         SALARY         DOJ          GENDER
- CHAR(04)    VARCHAR(15)      CHAR(04)    DECIMAL(7,2)   DATE         CHAR(1) M,F,O
- E001        TOMMY            D009        12345.67       2001-01-01   
    - There is an issue with this record, because department D009 does not exist
    - Before inserting, we'd like to check if the department id does exist before actually making the change
- Referential Constraint - building relationship between tables
    - Define child table - table that has FOREIGN KEY
- If the value does not exist in parent, it cannot exist in child
- Not all values that the parent has, must the child have
    - Parent can have multiple values not utilised by the child
- What happens if we delete department D001
    - What would happen to the employees from that department? (ON DELETE RULES)
        - Can close department and terminate (ON DELETE CASCADE)
        - Can close department and put them on bench (no projects assigned) (ON DELETE SET NULL)
        - Don't allow the department to close if there is at least 1 person in department (ON DELETE RESTRICT)
- When an INSERT SQL statement is executed on the child table, the referential constraint will check if the value (being inserted in the child table) is in the parent table
    - If it is there, then the insertion is allowed in the child table
    - If not, the insertion is rejected
- When a DELETE SQL statement is performed on the parent table
    - ON DELETE rules are activated
        - CASCADE - allow deletion in the parent table and delete all the depending records from the child table
        - SET NULL - allow deletion from the PT and set NULL value to the depending records' foreign key column in the child table
        - REJECT - do not allow deletion if there are at least 1 depending record in the child table
- If a table has a column that refers to another table, it is a child table
#### Quick note on Decimals
- DECIMAL(X,Y)
- X -> TOTAL NUMBER OF DIGITS
- Y -> AFTER DECIMAL POINT


### Error Codes
- -181 - not a valid datetime value
- -530 - index violation
- -545 - row does not satisfy check constraint
- -532 - relationship DNO restricts the deletion
- -122 - column or expression in the select list is not valid
    - When using the GROUP BY clause, you must use a function
    - No other columns can be selected apart from the columns that are involved in the GROUP BY clause and the columns involved in the function
