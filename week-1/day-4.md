## IEBCOPY
- Helps us copy from a PDS to a PDS
- DD
    - Input DD Name -> SYSUT1
    - Output DD Name -> SYSUT2
    - (Similar to IEBGENER)
- This is how to copy all members
- If we want to copy only specific members or exclude members, we need to use control card
- The DD names are of the user's choice
- SYSIN dd*
    - COPY OUTDD=SYSUT1
        - INDD=SYSUT2
    - SELECT MEMBER=(list of members separated by commas)
    - EXCLUDE MEMBER=(list of members separated by commas)
- Note, if the input PDS and the output PDS are the same, then this job will compress the PDS

## Control Library
- Recap:
    - SYSIN DD * - this is the instream data card
        - DD * lets us give data on the next line
            - as opposed to DSN which lets us specify the file name
    - SYSUT1 - Input for IEBGENER
    - SYSUT2 - Output for IEBGENER
- Control Library is a technique in which the control statements (card/sort cards) are written in a different PDS member and that PDS member can be mentioned in the SYSIN DD DSN= of the job
    - The PDS is called the Control Library
    - The member -> Control Book
- DSN
    - DSN is used to reference a data file
        - Can be a PS
        - Can also be a PDS member

## Procedures
- A collection of reusable steps that will be invoked by a job step
    - EXEC PROC=PROCNAME
        - Note, we use PROC instead of PGM
    - A procedure can have a max of 15 steps
    - Syntax:
        - PROC PROC1
        - PSTEP1
        - PSTEP2
        - PEND
- INSTREAM
    - Defined in the job where it is invoked i.e. The job and the procedure are written in the same member. 
    - Scope: only within the job
- CATALOG
    - Defined in another PDS member: Any job from any PDS can invoke this procedure
    - Note: The name of the member in which the procedure is defined and the name of the procedure must be the same
    - The definition of the procedure must be done AFTER THE JOB CARD AND BEFORE THE FIRST JOB STEP
    - The location of the catalog procedure is mentioned with JCLLIB ORDER = PDS
    - After the job card and before the first Job step.

## Overriding DD Statements
- When the user wants to dummy a DD name mentioned in the procedure and wants to give a different dataset from the invoking job, it can be done via the technique called "Overriding DD statement"
    - Syntax:
        - PSTEP.SYSUT1 DD DSN=ARI001.RORY.REVAT.OUTPUT.PS,DISP=SHR

## Sort Utility
- SORT - default sort utility on your system
    - Capabilities:
        1. Sort
        2. Reformat the data (Changing the Fields, introducing new fields, removing fields, arithmetic operations, etc.)
        3. Sum Up Values
        4. Eliminate Duplicates
        5. Split records into multiple output fields
        6. Merge Data
        7. Choose records by condition (filtering)
        8. Generate reports
    - DDNAMES
        - INPUT -> SORTIN, SORTIN01, SORTIN02, ETC.
        - OUTPUT -> SORTOUT, SORTOF01, SORTOF3, ETC.
        - SORTWK01 -> Temporary file, where the processing happens (optional)
        - SYSOUT DD SYSOUT=* - mandatory for sorting utility
        - SYSIN DD *
            - SORT CARDS
        - /*
```
1001 TOMMY CHENNAI 800000
1234 JERRY DISNEY  700000
```
    - To sort records in some order based on a field
        - Sort Fields = (STPOS, LENGTH, TYPE, ORDER)
            - STPOS -> the starting column of the field by which to sort
            - LENGTH -> the number of characters starting from STPOS, that makes up the field
            - TYPE -> CH (CHAR), ZD (Zone Decimal), PD (Packed Decimal)
                - For numbers use ZD
            - ORDER -> A (Ascending) or D (Descending)
        - Sum Fields = (STPOS, LENGTH, TYPE)
- DFSORT - paid utility that doesn't come default
- Choosing records base on conditions:
    - LOGICAL - AND, OR
    - COMPARATIVE - LT, GT, GE, LE, EQ, NE
    - SYSIN DD *
        - SORT FIELDS=(1,4,CH,A)
        - INCLUDE COND=(STPOS,LENGTH,CH,EQ,C'STRNG')
        - INCLUDE COND=(STPOS,LENGTH,ZD,EQ,1234)
        - OMIT COND=
    - For a range of salaries
        - SORT FIELDS=(1,4,CH,A)
        - INCLUDE COND=(22,6,ZD,GE,050000,AND,22,6,ZD,LE,080000)




- Null Statement -
    - If we have a line with just 2 slashes and nothing else, it is called a null statement
    - It will be treated as the end of the job
    - Any statements after it will not be executed