## Project Tips:
- exec cics syncpoint end-exec
    - Execute this statement when an insert/update/delete. Saves the transaction
- CEDF
    - DIAGNOSTIC FACILITY
    - EDF MODE ON
    - THEN, GIVE TRANSID
    - HIT ENTER, WILL FAST-FORWARD TO FIRST CICS STATEMENT
        - WILL GET RETURN CODE FOR IT
    - ENTER AGAIN, WILL GO TO NEXT CICS STATEMENT
- Check database
    - Option 9, SP - SPUFI
    - Commands 7
    - Cmd 1: -dis database(ari005db) spacenam(ari005ts)
        - Status should be RW, if not let Alwyn know