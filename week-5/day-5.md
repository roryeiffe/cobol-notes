## CICS
### Handling files
- Can handle the following VSAM datasets:
    - ESDS
    - KSDS - can give key and take record
    - RRDS
- Access Methods:
    - BDAM
    - VSAM
- BAsic Operations
    - Add a record
    - Modify an exiting record
    - Delete
    - Browsing one or select all records
- CICS provides:
    - Exclusive control (record level locking)
    - Data Independence
    - Journaling
    - Opening and Closing Files
- Defining Files
    - Use IDCAMS job, create file
    - Re-indexing, create new indexes, using IDCAMS
- Defining a file to CICS
    - File should be defined in FCT
    - FCT contains info about a file
    - Defining Files can be done by CEDA Transaction of DFHFCT Macro
### CICS Commands:
#### Set file in FCT
- CEMT SET FILE (ARI001F) 
- Enter
- Empty or Not Closed - cannot open another file without closing previous file
- Can replace OPE with CLS to close
    - vice versa to open
#### Open File
- Be sure file is opened
- Can we open an already opened file?
    - File is open in CICS region so we can't open in TSO
    - Need to close it in CICS first to be able to open it
#### Random Read
- When we handle files in COBOL:s 
1. Link (Environment Division)
2. FD Entry, File Layout (Data Division, File Section)
3. Open File (Procedure Division)
4. Operation, read, write (Procedure Division)
5. Close File (Procedure Division)
6. File Status
- When we handle files in CICS
1. File must be in FCT by CICS command: CEMT SET FILE(FILENAME) OPEN,ENA
2. File Layout given as normal working storage section variables
3. Open - already opened by the CEMT command before writing the program
4. Operations (Procedure Division)
5. Don't need to close in the program, use FCT and change OPE to CLS
6. File Status -> ws-resp
- EXEC CICS READ
    - RIdfld - KSDS
    - RRn - RRDS
    - RBa - ESDS
    - GEneric - look at part of the key
    - GTEQ - greater than or equal to
    - More found on powerpoint
#### Sequential Access
- Three step process
    - Start Browse
    - Read Next/Read Previous
    - End Browse
#### Write
- EXEC CICS WRITE FILE
#### Rewrite
- Should have already read the record
- Then can rewrite the record
#### Delete
#### Unlock