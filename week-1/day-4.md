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




- Null Statement -
    - If we have a line with just 2 slashes and nothing else, it is called a null statement
    - It will be treated as the end of the job
    - Any statements after it will not be executed