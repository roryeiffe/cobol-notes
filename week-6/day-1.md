## CICS
### Handling Files
- Don't have to link file in COBOL program
- Don't need to give layout in file section, we can give it in working storage
- Open/close file in CICS environment
- CEMT SET FILE
- We can use maps
- Write a program with 2 maps
    1. Should get user id
    2. Read matching record from KSDS
- Named Field
- Unnamed Field

#### Example
- Revature Data Systems
- Date: 10-10-2023                  Time
    - Id: ----
#### rEAD A RANDOM RECORD AND SEND IT AS A TEXT TO THE CICS region
1. DATA DIVISION
    - WORKING STORAGE SECTION,
    - 01 WS01-VARS.
        - LAYOUT OF THE KSDS
            - 05 WS05-RECORD
                - 10 FILE-ID PIC X(04).
                - 10 F       PIC X(01).
                - 10 FILE-NAME PIC X(04).
                - 10 FILLER    PIC X(01).
                - 10 FILE-LOC ...
                - ...
                - FILLER       PIC X(53).
            - 05 WS-RESP       PIC S9(8) COMP
2. PROCEDURE DIVISION.
- MOVE '1090' TO FILE-ID.
- EXEC CICS
    - READ
    - FILE(ARIO14F)
    - INTO ('WS05-REC')
    - RIDFLD(FILE-ID)
    - RESP(WS-RESP)
- END-EXEC.
- EVALUATE TRUE
- WHEN WS-RESP=DFHREP(NORMAL)
    - EXEC CICS SEND
        - FROM (WS05-RECORD)
        - ERASE
    - END-EXEC
- WHEN WS-RESP=DFHREP(NOTFND)
    - MOVE SPACES TO WS05-RECORD
    - MOVE 'THE RECORD IS NOT FOUND IN THE KSDS' TO WS05-RECORD
    - EXEC CICS SEND
        - FROM (WS05-RECORD)
        - ERASE
    - END-EXEC

#### Alwyn
Read a random record and send it as a text to the cics region.
 
```
Data division.
Working storage section.
01 ws01-vars.
** layout of the ksds
               05 Ws05-record.
                               10 file-id                              pic x(04).
                               10 f                                        pic x(01).
                               10 file-NAME                     pic x(04).
                               10 f                                        pic x(01).
                                               10 FILE-LOC
                                               10 FILLER                             PIC X(53).
                               05 WS-RESP                                        PIC .
 
PROCEDURE DIVISION.
               MOVE ‘1090’  TO FILE-ID. 
    EXEC CICS
               READ
               FILE(ARI014F)
               INTO('WS05-RECORD')
               RIDFLD(FILE-ID)
               RESP(WS-RESP)
END-EXEC.
EVALUATE TRUE
WHEN WS-RESP=DFHREP(NORMAL)
               EXEC CICS SEND
                               FROM( WS05-RECORD)
                               ERASE
               END-EXEC
WHEN WS-RESP=DFHREP(NOTFND)
               MOVE SPACES TO WS05-RECORD
               MOVE ‘ THE RECORD IS NOT FOUND IN THE KSDS’ TO WS05-RECORD
EXEC CICS SEND
                               FROM( WS05-RECORD)
                               ERASE
               END-EXEC
END-EVALUATE.
```