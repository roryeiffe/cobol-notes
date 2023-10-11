## DB2
### EMBEDDED SQL
- Points to be considered
1. SQLCA - SQL Communication Area - a built-in set of variables in the DB2 sub-system
    - A medium through which DB2 subsystem communicates with the program
    - SQLCODE - status code of the SQL operation will be reflected in this code
        - Like file status variable
    - Including SQLA makes SQLCODE available with the updated status on the recent SQL operation
2. DCLGEN variables: DECLARATION GENERATOR (NOT FOR CREATE STATEMENT)
    - Host Variables
    - the layout of the table
        - Make a DCLGEN entry for the table that you're planning to access
    - Note: when a host variable is mentioned inside an SQL delimiter, it must be prefixed by ':'.
3. DELIMITER
    - All the SQL statements must be encapsulated by SQL delimiter
    - EXEC SQL
    - END-EXEC.
4. DSNTIAR
    - It is a built-in sub-program
    - A built-in sub program. WE need to call this subprogram to capture the SQL error message and display the same in our SPOOL
    - 05 WS05-ERR-MSG.
        - 10 WS10-ERR-LEN PIC S9(04) COMP VALUE 800.
        - 10 WS10-ERR-TXT PIC X(80)  OCCURS 10 times
    - 05 WS05-ERR-LRECL   PIC S9(09) COMP VALUE 80.
    - CALL 'DSNTIAR' USING SQLCA WS05-ERR-MSG WSO5-ERR-LRECL
    - DISPLAY WS05-ERR-MSG
5. The status of SQL statement
    - EVALUATE TRUE
    - WHEN SQLCODE=0
- FOR A TABLE WITH UNIQUE CONSTRAINT AND PRIMARY KEY CONSTRAINT, UNIQUE INDEX MUST BE CREATED FOR THOSE COLUMNS. THE TABLE DEFINITION REMAINS INCOMPLETE UNTIL UNIQUE INDEXES ARE CREATED FOR THOSE COLUMNS.

### DB2I MENU DCLGEN
- OPTION 2 DCLGEN
- Option 1, give source table name, the table for which you want to convert column names into variable names
- Option 4, give the DB2 PDS and a member starting with DCL
- Option 6, REPLACE
- Option 7-8,10 ignore these
- Option 9, HV-
- Option 11, YES
- Then, go check the PDS member and it should have converted table columns into COBOL variables (Host Variables)
- Prefixes are important because sometimes the column names we pick are reserved words in COBOL

### Scenario
- Insert
- Select

### Scenario:
- Delete Records from COBOL programs
- Move 'e001' to hv-id
- EXEC SQL
    - DELETE FROM TB_COB_EMP
    - WHERE ID = hv-id
- END-EXEC
- Note: multiple record can be deleted using delete statement

### Scenario
- Update Records from COBOL programs
- Move 'e001' to hv-id
- EXEC SQL
    - UPDATE TB_COB_EMP
    - set salary = salary*1.1
    - WHERE ID = hv-id
- END-EXEC
- Note: multiple record can be updated using update statement (without using where where clause)

### Task of the day:
- INPUT FILE: HLQ.DB2.PS
    - ENTER 10 RECORDS LIKE THIS:
    - 1001 TOMMY CHENNAI 12345.67 2000-10-10 M
- OUTPUT IS TB_COP_EMP;
- Write a program to read all records from the PS and insert each of them into the DB2 table
- Hints:
    - Think of environment division
        - organization sequential, access sequential
        - Declare file layout
    - Include SQLCA
    - Follow standard para structure (main para, open para, read para, close para etc.)
