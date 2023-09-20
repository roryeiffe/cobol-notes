## Recap
- Jobs
- PDS - lrecl=80, recfm = fb, pds(member) -> write jobs -> submit <- jobid, condition code -> spool report

- Job Card -> jobname (userid, JOB, parameter (notify))

- JOBSTEP (255), STEPNAME EXEC PGM = IEFBR
- DD Statements (3000 +) -> NAME, DD, PARAMETERS
    - Dataset Definition

## Jobs

1. Job Card - read more on this yesterday in [day 2](day-2.md)
2. Job Step (255)
// STEPNAME EXEC PGM = UTILITY, PARAMETERS
- UTILITY
    - SYSTEM
    - USER
        - IEFBRE14 -> Dummy utility
            - We used it yesterday to just run a simple job
            - Every job must have a step
        - IEBGENER -> Copy from a PS to a PS
            - only from a flat file to a flat file
        - IEBCOPY -> Copy from PDS to a PDS
            - Note that we can do this using edit commands as well, but now we are using jobs
        - IEBEDIT -> Edit datasets 
        - SORT -> Data Processing
        - IDCAMS -> Catalog data set
- Parameters (optionals)
    - TIME = means the same as jobcard but this is for an individual step
        - Time = 2 -> minutes
        - Time = (,3) -> seconds
            - purposefully skipping first parameter
    - REGION = means the same as in jobcard but this is for individual step
    - COND = more on this later
    - PARM = Compiler options , input to COBOL
3. DD statements (3273)
//DDNAME DD PARAMETERS
- Two types of DD statements
    1. User Defined
        - // DDNAME DD
            - DDNAME: depends on the utility that is being used
                - IEFBR14, IDCAMS - user defined, you can give any name
                    - Normally go with DD1, DD2, DD3
                - SORT - SORTIN, SORTIN01, SORTIN02 -> input
                    - SORTOUT, SORTOF01, SORTOF02 -> output
                - IEBGENER/IEBCOPY - SYSUT1 -> input
                    - SYSUT2 -> output
        - Parameters:
            - Mandatory
                - DSN - Dataset Name
                - DISP = (D1, D2, D3)- Disposition
                    - D1 -> current status of the data set
                        - NEW -> it allocates a new dataset
                        - OLD -> the DS exists, if you want to read from it
                            - or if you want to write into it
                            - We hardly use Old in practice
                        - SHR -> the DS exists, if you want to read from it
                            - if you want to write into it
                                - keep in mind it writes into the dataset by emptying it first
                        - MOD -> 
                            - if the file exists, if it used for output file
                                - It appends the data
                            - if the file doesn't exist, it acts as NEW, allocates a new dataset
                    - D2 -> status of the dataset after the successful execution of the job 
                        - KEEP - keep the file in the local directory
                        - DELETE - physically remove the file from memory
                        - CATLG - the DS is moved to the main directory
                        - UNCATLG - Remove from the CATLG and put in local directory
                        - PASS - retain the file in the job for consecutive steps to access it
                            - In consecutive steps, pass will retain the dataset in the execution region
                            - *COMMON INTERVIEW QUESTION*
                    - D3 -> status of the DS after a job failure
                        - KEEP
                        - DELETE
                        - CATLG
                        - UNCATLG
                    - When DISP allocates a new dataset
                        - SPACE - (SU, (PQ, SQ, DB), CONTIG, RLSE, ROUND)
                            - SU - TRK, CYL, B
                            - PQ - 2
                            - SQ - 3
                            - DB - 2 - Only for PDS
                            - CONTIG - Contiguous Space Units 
                            - RLSE - release unused space automatically
                            - ROUND - release the memory rounding it to SPACE UNITS
                    - Default Values
                        - DISP = NEW -> DELETE, DELETE
                        - DISP = OLD, SHR -> KEEP, KEEP
                        - 
                - DCB - Directory Control Block - (LRECL=80,BLKSIZE=800,RECFM=FB,DSORG=PS)
                    - DSORG - can be written in DCB or as a separate parameter
                        - PS -> PS
                        - PO -> PDS
                        - LIB -> Library


    2. System (mandatory)
        - OUTPUT:
            - SYSPRINT DD SYSOUT=*
                - SYSPRINT - represents the system printed outputs, reports
                - SYSOUT=* - put that output in the SPOOL
                - All together, this statement is telling the system to put the reports in the SPOOL
            - SYSOUT DD SYSOUT=*    SYSOUT -> Sort Card Errors
        - INPUT:
            ```
            SYSIN DD *
            SORT FIELDS = (1,4,CH,A)
            /*
            ```
            - A note on syntax: the * represents that inputs are on the next line (data or control statements)
                - The delimiter of /* represents the end
            - Note: when a data or the control cards are given in the job it is called INSTREAM DATA/CARDS
            - SYSIN DD dummy -> no inputs/control cards are given in the job
        
- Comments
    - //* will make the entire line a comment (Recommended)
    - add a space after the command and the rest of the line will be a comment

- Instead of taking option 3 and then option 4, we can type 3.4 to do those 2 steps at once
- HI JCL -> will highlight JCL syntax
- Spool, delete old records
    - type //p at upper limit of lines to purge
    - type // at lower limit
    - Hit enter a few times