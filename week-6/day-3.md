## CA7
- Force Complete vs Cancel?
    - Cancel will remove the job from the CA-7 queue
    - Force Complete will ensure it finishes, failed job is marked with CA-7 as normal completion. Jobs waiting for this job to complete can now run
    - Restart - restart the job in the step which has failed
    - Re-run - 
- SCHID - a schedule or job stream can have many different variations. 
- HOLD - A job can be placed on hold before or after it enters the CA-7 queue. The job will remain in hold, meaning it will not run, until the hold is removed
- What happens if we cancel the job in the middle?
- ADDRQ - add a new request
- When the job is in CA7 Queue Add a user requirement to the job
- Hold on job can be put when the job is in the queue and when the job is not in the CA7 queue.
- Three main queues in CA-7
    - Request Queue - LREQ
    - Ready Queue - LRDY
    - Active Queue - LACT
- Job Status Late - it is waiting for some other requirement to finish
- Dead line Time - it is the time by which the Job should meet the requirements and should start successfully

## Handling DB2 with CICS
- Remember, we have DCLGEN (declaration generator) - generate variables based on table in SQL, bring it over to a member where it can be access by COBOL code
    - Create table layout
    - COBOL only has 3 data types while SQL has about 15
    - DCLGEN is a built-in tool that takes table as input and converts the table name and column names into COBOL adaptable variables
        - These variables can be used in our program
    - Host Variables - prefixed with HV, these are the COBOL-adaptable variables that are generated via DCLGEN
    - If I'm handling files, need File Layout
    - When value comes up from a map, what is the map layout we have?
- Dynamic Positioning - 
- Can move a -1 to the length variable and then send a map