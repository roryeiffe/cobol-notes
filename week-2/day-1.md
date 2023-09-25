## Topics:
- Sort if then when
    - Change/introduce a field depending on a value in another field
    - 1001 tommy chennai 90 A
    - 2345 jerry Disney  50 B
    - 1224 Mohan India   23 C
    - 50-69 B, 70-100 A, 0-49, C
    - Code:
        - SORT FIELD=COPY
        - OUTREC    IFTHEN=(WHEN(12,7,CH,EQ,C'CHENNAI'),
                            OVERLAY=(30:C'ASIA')),
                    IFTHEN=(WHEN=(12,6,CH,EQ,C'LONDON'),
                            OVERLAY=(30:C'BRITAIN')),
                    IFTHEN=(WHEN=NONE,
                            OVERLAY=(30:C'AMERICA'))

- Sort Split - sample
    - Evenly distribute the records
        - OUTFIL FILES=01,STARTREC=1,SAMPLE=2  => (1,3,5,7,9,ETC.)
        - OUTFIL FILES=02,STARTREC=2,SAMPLE=2  => (2,4,6,8,10,ETC.)
- SORT MERGE
    - Merging data from multiple data sets into a single output dataset.
    - Can be done in 2 ways:
        1. Data will be appended (first file1, then file2, etc.)
        2. Merging (take data from all files, sort it, and then store)
            - prerequisite: data in all the input datasets must be in sorted ascending order based on the field which the user wants to merge
            - If they are not in sorted order, we get an "Out of Sequence" error
    - Syntax:
        - Use SORTIN01, SORTIN02, SORTIN03, ETC. to define input files
        - MERGE FIELDS=(1,4,CH,A)
- SORT ARITHMETIC OPERATION WITH UFF AND SFF
    - 12.34
    - 45.67
    - When trying to use arithmetic operations on this data, we will get SOC7 abend
    - To handle data with physical decimal point, data must be considered as UFF (Unsigned Free Format)
    - To handle data with physical decimal point and data is represented with a sign, data must be considered as SFF (Signed Free Format)
    - Formats ->    ch,zd,ps,uff,sff
    - ex:
        - want to add 99.99 to some data
            - OUTREC OVERLAY=(30:20,6,UFF,ADD,+09999.EDIT=(TTT.TT))
                - Adds 99.99 to the columns 20-26 and places result in column 30
                - Note that we do NOT give the decimal in the actual number to add
        - Add 2 fields together
            - OUTREC OVERLAY=(35:20,6,UFF,ADD,27,6,UFF,EDIT=(TTT.TT))
            - wITH SIGNS:
                - OUTREC OVERLAY=(38:20,7,SFF,ADD,28,7,SFF,EDIT=(sTTT.TT),SIGNS=(+,-))
                    - Note the extra parameter to specify SIGNS
                        - If we change these symbols, it will change what symbol is used to denote positive/negative numbers
                            - ex: SIGNS=(*,#)
                    - Also note the 's' in the EDIT parameter. Otherwise, we don't get the sign in the result
- Overlay vs Fields Recap
    - Overlay - all columns will be brought, just mention the change
    - Fields - must mention the columns you want
        - won't be automatically brought over
- COND parameter
    - Job Card parameter, we have a parameter COND
    - In Job Step, we can have COND parameter
    - COND in step: In a multi-step job, where the execution of a step depends on any previous steps in the same job, COND parameter can be used in this step
    - COND in Job Card: In a multi-step job, where the nomination of a step depends on any steps in the same job, COND parameter can be used in this job card
        - a step can be nominated depending on the rc of a separate step
    - NOTE: The CONDition MUST FAIL for a step to be executed
        - This is the opposite of an if-statement:
        - IF(a > b)
            - Display "hi"
        - ELSE
            - DISPLAY "hello"
        - END-IF
    - Anti-If-Statement
    - COND=EVEN -> Even if any of the previous steps fail, this step will be executed
    - COND=ONLY -> This step will be executed only if any of the previous step fails/abends
        - //JOBCARD
        - //STEP1
        - //STEP2 COND=EVEN
            - Even if step 1 abends, step 2 executes
            - This is different from the normal flow where is a step abends then the following jobs will not execute
        - //STEP3 COND=ONLY
            - Only if any of the previous steps abend, then execute this step
            - Like a substitute player on a sports team
    - COND=(RC VALUES, OPRTR, STEPNAME)
        - Params:
            - RC VALUE=0 (successful), 4(successful with warnings/info), 8, 12, 16, 20, 22, 4096
            - OPRTRS-> LT,LE,GT,GE,EQ,NE,AND,OR
            - STEPNAME -> any previous step name based on RC this step may/may not be executed
        - Scenario: Step 2 must be executed ONLY if the step1 is successfully executed
        - Analysis: If step 1 is successful, could have 0 and 4 return codes
            - 0,4 -> the condition must fail. For value GE8, the condition must be true
            - //JOBCARD
            - //STEP1
            - //STEP2 COND=(8,LE,step1)
                - 8 LE 0 - No, so executes
                - 8 LE 4 - No, so executes
                - 8 LE 8 - Yes, so doesn't execute
            - //STEP3
    - COND=(RC VALUES,OPRTR) -> will consider all of the previous steps
        - Cannot execute previous steps
- GDG - GENERATION DATA GROUP - IDCAMS
    - It is a collection of datasets in chronological order. The datasets are called as members/generations. Each dataset CAN BE OF DIFFERENT TYPES
        - Different from a PDS because of the different types. 
        - The name of the GDG base will be reused for its generations. 
        - The system generates a unique generation number and suffixes it to the members. (G001V00)
    - Why do we need GDG?
        - For backup files
            - Imagine having to do this the manual way, naming each file like this:
                - ARI001.REVAT.WEEKLY.BACKUP
                - ARI001.REVAT.WEEKLY.BACKUP1
                - ARI001.REVAT.WEEKLY.BACKUP2
            - GDG makes this automatic
    - Steps
        1. Define a GDG Base -> register a name to the system to be reused.
            - ARI001.REVAT.BACKUP
            - //JOBCARD
            - //STEP1 EXEC PGM=IDCAMS (we've used IDCAMS for empty file handling)
            - //SYSPRINT DD SYSOUT=*
            - //SYSOUT DD SYSOUT=*
            - //SYSIN DD *
            -         DEFINE GDG(-
            -              NAME(ARI001.RORY.REVAT.BACKUP)- 
            -               LIMIT(3) - 
            -               EMPTY/NOEMPTY -
            -               SCRATCH/NOSCRATCH -
            )
            - Parameters
                - LIMIT(n) -> max number of members that can be in the catalogue
                    - Though, when we gave a limit of 2, we could still create a 3rd generation 
                        - Note, LIMIT is how many members can be in te catalogue, not how many can be created
                    - By default, max number of generations that can be created is 255.
                - EMPTY -> when a new generation is created after reaching the limit, ALL THE OLDER GENERATION ARE UNCATALOGUED
                    - removed from catalogue and placed in main directory
                - NOEMPTY -> when a new generation is created after reaching the limit, ONLY THE OLDEST GENERATION IS UNCATALOGUED
                    - removed from catalogue and placed in main directory
                - SCRATCH -> THE UNCATALOGUED GENERATIONS ARE PHYSICALLY DELETED
                - NOSCRATCH -> THE UNCATALOGUED GENERATIONS ARE RETAINED
        2. GDG-BASE-NAME(+1) -> Create a new member
            - ex: if we give 5, skip 5 in the numbering
        3. GDG-BASE-NAME(0) -> Refers to recent current member
        4. GDG-BASE-NAME(-1) -> Previous members
        
