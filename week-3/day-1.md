## Paragraphs
- Paragraph starts in margin A
    - 1000-INIT-PARA
    - 3000-PROC-PARA
    - 4000-TERM-PARA

## Perform Statements
- Out of the line Perform statement
    - When a perform statement performs/calls a paragraph that is/not in the sequence of the statements. The perform statement jumps to the paragraph and executes the statements in the paragraph and after doing so it comes back to the next from where it went.

## Move
- Move corresponding grp1 to grp2
    - It will move the values from source members to the destination members ONLY IF THEIR NAME MATCHES, irrespective of their sequence
    - If trying to move group to another with no matching member names, it will have no effect
- Move grp1 to grp2
    - It moves values in the sequence

## Copy Statements
- A group of reusable statements can be written in any PDS member. 
    - They can be COPY'd into any division of your COBOL program
- Note: We need to mention the PDS in which the copy member is kept, in the compile JCL
    - COMPILE.SYSLIB DD 
    - The member is called a COPY BOOK
    - The PDS is called COPY LIBRARY
    - Can put the skeleton in a COPY BOOK and copy it every time

## Handling Files
- Files -> Transaction, Master, Batch
- Processing data from a dataset and sending it to another dataset
    1. Link the file with program
        - Defined in the Environment Division
        - ENVIRONMENT DIVISION.
        - INPUT-OUTPUT SECTION.
        - FILE-CONTROL.
            - SELECT TI001-PS ASSIGN TO DDNAME
            -                 ORGANIZATION IS SEQUENTIAL
            -                 ACCESS IS SEQUENTIAL
            -                 FILE STATUS IS WS05-FST-TI001.
            - ORGANIZATION -> SEQUENTIAL PS, ESDS
            -              -> INDEXED KSDS
            -              -> RELATIVE RRDS
            -              -> LINEAR LDS
            - ACCESS -> SEQUENTIAL, RANDOM, DYNAMIC
            - Logical file name for the file is given here
            - Accessing Mode
            - Organization of the file (PS, KSDS, ESDS, RRDS, etc.)
    2. File Variables. File Layout
        - Declare variables for the fields in the file
        - 1001 tommy chennai   0300.89
        - Which division does this happen in?
            - Data Division
            - Need to know the layout of the file
        - DATA DIVISION
        - FILE SECTION
        - FD TI001-PS.
        - 01 TI001-PS-REC.
            - 05 TI001-ID       PIC 9(04).
            - 05 FILLER         PIC X(01).
            - 05 TI001-NAME     PIC A(05).
            - 05 FILLER         PIC X(01).
            - 05 TI001-LOC      PIC A(09).
            - 05 FILLER         PIC X(01).
            - 05 TI001-SAL      PIC 9(05).9(02).
            - 05 FILLER         PIC X(51).
    3. Open file in appropriate mode (read, write, etc.)
        - SYNTAX: OPEN MODE LOGICAL-FILENAME
            - INPUT -> reading from it
            - OUTPUT -> writing into it
            - I-O -> reading and re-writing
            - EXTEND -> append records to the file
        - PROCEDURE DIVISION
    4. Read, Write, Rewrite, Delete, Start, Read Next
        - PROCEDURE DIVISION
        - READ FILENAME -> one record from the dataset is copied into the file layout
        - AT END 
            - IMPERATIVE
        - NOT AT END
            - IMPERATIVE
        - END-READ.
        - WRITE RECORD-NAME -> The values in the layout are moved into the file
        - DELETE FILENAME -> The matching record would be removed from the file
            - Not every file allows you to do every operation, this works with KSDS
        - REWRITE RECORD-NAME -> modifies the record
        - START, READ-NEXT -> dynamic access
    5. Close the file
        - PROCEDURE DIVISION
            - CLOSE FILENAME
    6. File Operation Status
        - We need to know the status of the operation
        - STATUS CODE: 
            - 00 - OPERATION SUCCESS
            - 10 - END OF FILE IS REACHED
    - Scenario - 
        - Sweep/read all records from the PS and validate the records with the below validations:
            - The id field must only have numbers, no alphabet allowed
            - The salaries must have numbers before and after the decimal points
            - The decimal point must be a period (no commas ,etc.)
        - Display all the valid records
        - Don't display the invalid records
        - DATA FLOW:
            - OPEN THE FILE
                - FAILURE - KILL THE PROGRAM
                - SUCCESSFUL - READ UNTIL STATUS CODE 10 (EOF)
                    - EACH READ, PERFORM VALIDATION
                        - VALID -> DISPLAY
                        - INVALID -> SKIP
            - CLOSE THE FILE
        - PROCEDURE DIVISION
        - 0000-MAIN-PARA.
            - PERFORM 1000-INIT-PARA
            - PERFORM 3000-PROC-PARA
                - THRU 3000-PROC-PARA-EXIT
            - PERFORM 9000-TERM-PARA
        - 1000-INIT-PARA.
            - EXIT
            - .
        - 3000-PROC-PARA.
            - PERFORM 3100-OPEN-PARA
            -   THRU 3100-OPEN-PARA-EXIT
            - PERFORM 3200-READ-PARA 
                - THRU 3200-READ-PARA-EXIT
                - UNTIL WS05-FST-TI001=10
            - PERFORM 3300-CLOSE-PARA
                - THRU 3300-CLOSE-PARA-EXIT
            - .
        - 3000-PROC-PARA-EXIT.
            - EXIT
            - .
        - 9000-TERM-PARA.
            - STOP RUN
            - .
        - 3100-OPEN-PARA
            - OPEN INPUT TI001-PS
            - IF(WS05-FST-TI001 = 0) THEN
                - DISPLAY 'TI001 OPEN SUCCESS'
            - ELSE
                - DISPLAY 'TI001 OPEN FAILED: ' WS05-FST-TI001'.
                - PERFORM 9000-TERM-PARA.
            - END-IF
        - 3100-OPEN-PARA-EXIT
            - EXIT
            - .
        - 3200-READ-PARA
            - MOVE SPACES TO TI001-PS-REC TO001-OS-REC    <--- EMPTY OUT THE RECORD FIRST
            - MOVE 0 TO WS05-NEW-SAL
            - READ TI001-PS
            - EVALUATE TRUE
            - WHEN WS05-FS-TI001 = 00
                - PERFORM 3210-VALID-PARA
                    - THRU 3210-VALID-PARA-EXIT
            - WHEN OTHER
                - DISPLAY ' TI001- READ FAILED   ' WS05-FST-TI001 
            - END-EVALUATE
        - 3200-READ-PARA-EXIT
            - EXIT
            - .
        - 3210-VALID-PARA
            - EVALUATE TRUE
            - WHEN TI001-ID      IS NUMERIC AND
                - TI001-SAL(1:5) IS NUMERIC
                - TI001-SAL(6:1) = '.' AND
                - TI001-SAL(7:2) IS NUMERIC
                    - DISPLAY 'THE RECORD IS A VALID RECORD'
                    - DISPLAY 'THE ID: ' TI001-ID
                    - DISPLAY 'THE NAME: ' TI001-NAME
                    - DISPLAY 'THE LOCATION: ' TI001-LOC
                    - DISPLAY 'THE SALARY: ' TI001-SAL
            - WHEN OTHER
                - DISPLAY 'INVALID RECORD'
                - DISPLAY 'THE ID: ' TI001-ID
        - 3300-CLOSE-PARA
            - 
        - 3300-CLOSE-PARA-EXIT
            - EXIT
            - .
