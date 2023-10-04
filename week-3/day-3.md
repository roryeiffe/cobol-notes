## Recap
- If the requirement demands reading all the records from the KSDS (from the first to the last), then the access can be sequential
- When writing KSDS, access should be random, random is better choice
    - Can write sequential but you must promise that input is in sorted order
- If you want to read a matching record, delete a matching record, modify a matching record in the KSDS, then the access must be random

## Files (Continued)
- Reading, Modifying, and Deleting records in a KSDS
    - Can only be done in KSDS
### Delete
- Keep the key value in the key variable
- Example, if you want to delete record with key 9909
    - Then, you must move this value to the key variable
    - Remember in the environment division, RECORD KEY IS VAR-NAME
    - ex: MOVE 9909 TO TI002-ID
- DELETE FILENAME.
- EVALUATE TRUE
- WHEN WS05-FST-TI002 = 00
    - DISPLAY 'DELETED'
- WHEN OTHER
    - DISPLAY 'DELETE FAILED ' TI002-ID
    - DISPLAY WS05-FS-TI002
- END-EVALUATE
### READ
- Keep the key value in the key variable
- Example, if you want to read record with key 9909
    - Then, you must move this value to the key variable
    - Remember in the environment division, RECORD KEY IS VAR-NAME
    - ex: MOVE 9909 TO TI002-ID
- READ FILENAME.
- EVALUATE TRUE
- WHEN WS05-FST-TI002 = 00
    - DISPLAY 'RECORD FOUND IN KSDS'
    - DISPLAY TI002-KSDS-REC
- WHEN OTHER
    - DISPLAY 'MATCHING RECORD READ FAILED ' TI002-ID
    - DISPLAY WS05-FS-TI002
- END-EVALUATE

### MODIFY
- Keep the key value in the key variable
- Example, if you want to read record with key 9909
    - Then, you must move this value to the key variable
    - Remember in the environment division, RECORD KEY IS VAR-NAME
    - ex: MOVE 9909 TO TI002-ID
1. READ -> SUCCESSFUL -> DO THE CHANGES TO THE COLUMNS THAT YOU WANT TO MODIFY -> REWRITE RECORDNAME

- READ FILENAME.
- EVALUATE TRUE
- WHEN WS05-FST-TI002 = 00
    - DISPLAY 'RECORD FOUND IN KSDS'
    - DISPLAY TI002-KSDS-REC
- WHEN OTHER
    - DISPLAY 'MATCHING RECORD READ FAILED ' TI002-ID
    - DISPLAY WS05-FS-TI002
- END-EVALUATE


- Logical File Name -> Name that you give in the select statement when you assign the DDName
    - We give this name in the environment division
    - It is a logical name, we give it a name so we can remember what it stands for, KSDS, PS, Input, Output etc.
    - INPUT-OUTPUT SECTION
    - FILE-CONTROL
        - SELECT TI001-PS ASSIGN DDNAME
- Record Name
- DATA DIVISION
- FILE SECTION
- FD MI01-PS
- 01 MI01-PS-REC
    - 05 MI01-ID


- Scenario 5 -
- HLQ.RORY.REVAT.COBOL.KSDS -> DATABASE
    - ID PIC 9(04), NAME PIC A(05) LOC PIC A(09) SAL PIC 9(05).9(02)
        - 1 filler between fields
- Transaction File:
    - HLQ.RORY.OPRTN.PS
    - ID PIC 9(04) F OPRTN PIC A(01) FILLERS
    - 1002 D
    - 1234 R
    - 3456 D
    - 7987 M
    - 5676 Z
    - 8987 M
- Requirement
    - Sweep all the records from PS and perform the operations on KSDS.
        - Evalute True
        - When the operation is D
            - Delete the matching record
        - When the operation is M
            - Read -> do the changes to te columns -> rewrite
            - Modify the matching record
                - Change the location -> New Jersy
                - Change the salary -> increased by 10%
        - When the operation is R
            - Read the matching record and display the same in the spool
        - When Other
            - Display 'invalid operation'
        - End-Evaluate
- Analysis
    - PS SELECT TI001-PS ASSIGN TO DD1
        - Access is Sequential
        - Organization is Sequential
        - Declare a variable for File status: WS05-FST-TI001
    - SELECT DB01-KSDS ASSIGN TO DD2
        - ACCESS IS RANDOM
        - ORGANIZATION IS INDEXED
        - RECORD KEY IS DB01-ID
        - FILE STATUS IS WS05-FST-DB01
- OPEN INPUT TI001-PS
- OPEN I-O DB01-KSDS
- DATA FLOW:
- PD

- 3000-PROC-PARA.
    - OPEN-PARA
    - READ-PS-PARA UNTIL EOF
    - CLOSE-PARA
- 3000-PROC-PARA-EXIT.
    - EXIT
    - .
- READ -> BRANCH-PARA .
    - -> D -> DELETE PARA
    - -> R -> READ PARA
    - -> M -> MODIFY PARA
    - OTHER -> DISPLAY 'INVALID'
- BRANCH-PARA.
    - EVALUATE TRUE
    - WHEN TI001-OPRTN = 'D'
        - PERFORM 3211-DELETE-PARA
            - THRU 3211-DELETE-PARA-EXIT
    - WHEN TI001-OPRTN = 'R'
        - PERFORM 3211-READ-PARA
            - THRU 3211-READ-PARA-EXIT
    - WHEN TI001-OPRTN = 'M'
        - PERFORM 3211-MODIFY-PARA
            - THRU 3211-MODIFY-PARA-EXIT
    - WHEN OTHER 
        - DISPLAY 'INVALID OPERATION' TI001-OPRTN.
    - END-EVALUATE


## SUB-PROGRAM
- Remember, procedure is a set of repeatable steps that we can invoke in a job
- A sub-program is a reusable program that can be called from any program
- A sub-program MUST NOT HAVE STOP RUN but it can have GOBACK, END PROGRAM
    - If there is a STOP RUN in the sub-program, the control will never return to the calling program
    - If values are to be received and sent from and to the sub program LINKAGE SECTION must be defined in the sub-program
    - The values from the main program WILL ONLY BE AUTOMATICALLY MAPPED TO THE LINKAGE SECTION variables in the subprogram.
- Types of Procedures:
    - Catalog - procedure is written somewhere else
    - In-Stream - written in the same job where it is invoked
- Types of Sub-Programs:
    - Catalogued - subprogram is written in a different member
        - Subprogram is written in a different member
            1. Subprogram must have GOBACK instead of END PROGRAM
            1. Compile the sub-program first
            1. Compile the main program with a new ddname
                - LKED.SYSLIB DD DSN=LOADLIB(SUBPGM),DISP=SHR
            1. Run the main program
    - In-stream - the subprogram is written after the last statement of the main program in the same member
- Linkage Section Variables:
    - Are not readily available to the procedure division
    - Because the linkage section variable lives somewhere in the common buffer area between the main program and the sub-program
    - To use the linkage variables in the procedure division, write the USING clause in the procedure division along with a list of variables in the exact same sequence 
- Two ways of calling the sub-program:
    1. Call by Reference
        - By Default
        - The calling program's variables and the called program's variables (linkage section) share the same memory. Changing its values by a sub-program is reflected in the main program. 
    2. Call by Content
        - The initial values are copied from the main program. But when those values are changed in the sub-program, it is not reflected in the main program. 
        - The call by content variables have a different memory, hence the changes in the sub-program will not be reflected in the main
    - Syntax:
        - CALL 'SUBPGM' BY CONTENT USING WS05-A WS05-B
        -               BY REFERENCE USING WS05-C.


## Misc:
- File status codes:
    - 00 - success
    - 10 - end of file
    - 22 - duplicate key
    - 23 - record not found
    - 35 - An OPEN statement with the INPUT, I-O, or EXTEND phrase was attempted on a non-optional file that was unavailable.
    - 39 - 	The OPEN statement was unsuccessful because a conflict was detected between the fixed file attributes and the attributes specified for that file in the program.
- Reference: https://www.mainframestechhelp.com/tutorials/cobol/file-status-codes.htm
- ABEND CODE S806
    - Module does not exist
