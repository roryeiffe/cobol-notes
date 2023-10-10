## Intro Mainframe
1. What is a Mainframe?
1. What is the difference between a Mainframe and a Super Computer?
1. What is a DASD?
1. What is TSO?
1. What is ISPF?
1. What is meant by batch processing?
1. List and explain the parameters we need when allocating a dataset.
1. List and explain the TSO edit commands. 
1. Explain group commands. 
1. What is a PDS?
1. What is a PS?

## JCL
1. What is a job?
1. What is JES?
1. What is the SPOOL?
1. What are condition codes and return codes?
1. What is meant be Abend?
1. What is a Job Statement?
1. List and explain parameters of a job statement. 
1. What is the difference between a positional and keyword parameter?
1. Explain the COND parameter.
1. What is an EXEC Statement?
1. What is a DD Statement?
1. Explain the parameters used when defining a DD statement.
1. For the following utilities, explain their purpose and which DD statements we need to define:
    1. IEBGENER
    1. IEBCOPY
    1. IEFBR14
    1. SORT
    1. IDCAMS
1. Explain how to use the set command in a job.
1. What is a JCL procedure?
1. Explain how to write/use a JCL procedure.
1. What is meant by overriding DD statements?
1. How do we override DD statements?
1. What is a backward reference?
1. What is a GDG?
1. Explain what GDGs are used for.

## VSAM
1. What is VSAM?
1. What is a cluster?
1. Explain the different types of clusters.
1. List and explain the cluster allocation parameters. 
1. What is the difference between Sequential, Random, and Dynamic access?
1. What is REPRO?
1. What is PRINT?
1. What is a Control Interval?
1. What is a Control Area?
1. What is an AIX?
1. What are the steps of creating an AIX?
1. List and define the other IDCAMS functions?

## COBOL
1. Describe the structure of COBOL. What are the different divisions, sections?
1. Explain the column format of COBOL. Which commands go in which columns/areas?
1. What are the different data types of COBOL?
1. What is the PIC clause?
1. What is the difference between leading and trailing signed variables?
1. What does the word "separate" do when defining decimal variables.
1. Explain reference modification.
1. What is a group item?
1. What is a member item?
1. Explain how group items and member items are written and organized.
1. What arithmetic operations are available in COBOL?
1. Write some COBOL code to add/subtract/multiply/divide a number and a variable. 
1. List and explain the different types of conditions we can check in COBOL.
1. Describe the syntax of an if-statement in COBOL.
1. Describe the syntax of an evaluate statement in COBOL.
1. List and describe the 4 types of PERFORM statements. 
1. What is a COPY statement?
1. What is a COPY LIBRARY?
1. List and describe the 6 steps when handling files (read/write) in a COBOL program?
1. What kind of operations can we perform on a KSDS file in a COBOL program?
1. What is the best type of access for these operations?
1. What is a sub-program?
1. How do we create an in-stream sub-program?
1. How do we create a catalogued sub-program?
1. What is the purpose of the linkage section?
1. How do we modify a record in a KSDS using COBOL?
1. How do we delete a record in a KSDS using COBOL?
1. How do we create an array in COBOL?
1. What is a simple array?
1. What is an indexed array?
1. What are the 2 main types of searching arrays in COBOL?
1. What is the difference between Search and Search All?
1. What conditions must be met in order to perform a search using the Search function?
1. What conditions must be met in order to perform a search using the Search All function?
1. What is a manual search?
1. How do we use the SET command when working with arrays?
1. How do we create a multi-dimensional array?
1. How do concatenate strings in COBOL?
1. What is the DELIMITED BY clause used for in the STRING function?
1. What is the WITH POINTER clause user for?
1. What is the USAGE clause used for in the STRING function?
1. How do we split a string into multiple sub-strings?
1. What is a condition name in COBOL?
1. List and define some COBOL intrinsic functions.
1. What is an Edited Picture clause?
1. What do Z, B, 0, CR/DB, $ all stand for when dealing with Edited Picture clauses?

## DB2/SQL
1. What is DB2? What does it stand for?
1. What is a Tablespace?
1. What is a Buffer Spool?
1. What is SPUFI? Why is it useful?
1. What is normalization?
1. What is de-normalization?
1. What is an RDBMS?
1. What is a Database?
1. What is a table?
1. What is a record?
1. What is an entity?
1. List and explain the data types of SQL (at least the main categories and some common examples of each)
1. What are some constraints we can play on a column?
1. What is a primary key?
1. What is a foreign key?
1. What is referential integrity? Give an example.
1. What are the ON DELETE rules?
1. What is SQL?
1. What is DDL? What commands fall under it?
1. What is DML? What commands fall under it?
1. What is TCL? What commands fall under it?
1. What is DCL? What commands fall under it?
1. What is the GRANT command?
1. What is the REVOKE command?
1. What is the WHERE clause? Which operators can we use with it?
1. What is an aggregate function?
1. List and define some arithmetic functions in SQL.
1. What is a scalar function?
1. List and define some scalar functions in SQL. 
1. What are the three types of Complex Queries?
1. What is a correlated sub-query?
1. What is a non-correlated sub-query?
1. List and compare/contrast the different kinds of joines:
    1. Inner
    1. Full Outer
    1. Left Outer
    1. Right Outer
1. What is a set operation?
1. What are DB2 Privileges?
1. What are implicit/explicit privileges?
1. What are DB2 utilities?
1. What is a View?
1. What are locks?
1. What are the different modes of locking?
1. Why do we need locking?
1. Explain isolation levels.
1. What are the different types of reads? Explain them.
1. What are active and archive logs?