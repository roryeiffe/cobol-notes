### JCL Recap
- We have jobs that are scheduled
- Jobs are submitted for batch execution
- Event-based vs time-based
    - Event-based, job will wait for a particular file
    - Time-based, a stream of jobs that will run in a particular time

## CA7
- When running a job through CA7, job needs a Schid (schedule id)
- MIPS - Million Instructions Per Second

## CICS
- ASKTIME
    - used to obtain current date and time
    - Syntax:
        - EXEC CICS ASKTIME[ABSTIME(data-area)]
        - END-EXEC
- FORMATTIME
    - EXEC CICS FORMATTIME ABSTIME(data-ref)
        - [YYDDD(data-area)]
        - [YYMMDD(data-area)], ..etc.
    - END-EXEC
- DELAY
    - Used to delay the processing of a task
    - Syntax:
        - EXEC CICS DELAY
            INTERVAL(hhmmss) | TIME(hhmmss)
        - END-EXEC
- START
    - EXEC CICS START
        - TRANSID(transid)
        - [TERMID(termid)
        - TIME(hhmmss) | INTERVAL(hmmss) ]
    - END-EXEC
    - Conditions: INVREQ, LENGERR, TERMIDERR, TRANSIDERR
- Other Interval Control Commands
    - POST - request noficiation when the specified time has expired
    - WAIT EVENT - wait for an event to occur
    - RETRIEVE - retrieve data passed by START
    - CANCEL - cancel the interval control requests
    - SUSPEND - suspend a task
    - ENQ - gain exclusive control over a resource
    - DNQ - free the exclusive control from the resource gained by ENQ
### Terminal Conversation
- Conversational
    - A dialogue between program and terminal based on sending/receiving messages within the same task
- Pseudo-Conversational
    - dialogue between program and terminal which appears as a continuous conversation but is actually carried by a series of tasks
### Queues
- https://www.tutorialspoint.com/cics/cics_temporary_storage.htm#:~:text=Temporary%20Storage%20Queue%20(TSQ)%20is,is%20used%20to%20identify%20TSQ.
#### Transient Data Queue
- TDQ                      : TD and last 2 char of your userid EX: TD01
- Data can be stored/queue for subsequent internal/external processing
- Stored data can be routed to symbolic destinations
- TDQs require a DCT entry
- Intra-partitioned
    - association within the same CICS subsystem
- Extra-partitioned
    - association external to the CICS subsystem
- Operations
    - Write data to a transient data queue
    - Read data
    - Delete queue, deletes all entries in the queue
- Destination Control Table
    - DCT used to register the information of all TDQs
    - Destination Control Program (DCP) uses DCT to identify all TDQs and perform all I/O operations
#### Temporary Storage Queue
- created and deleted dynamically
- No CICS table entry required if recovery not required
- Each record in TSQ identified by relative position, called the item number