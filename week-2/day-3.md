## Recap
- Min size for Control Interval - 512 bytes
- Max - 32 KB
- How to access records from ESDS - 
    - RBA - Relative Byte Address

## Schedule
- Morning - Finish VSAM topics
- Afternoon - Next case study


## IDCAMS Functions:
- AIX - Alternate Index - When the user wants to have another column/field of the record as alternate index
    - Can be unique or non-unique
    - Only for LSDS/KSDS
    - 1001 tommy Chennai 89098907
    - Must relate index to base cluster
    - DEFINE:
        - //sysin    dd *
        - //   DEFINE AIX( -
        -        NAME(ARIOO1.RORY.REVAT.VSAM.LOC.AIX)
        -        RELATE(ARI001.RORY.REVAT.VSAM.KSDS) -
        -        TRACKS(2,3) -
        -        RECORDSIZE(80,80) -
        -        FREESPACE(10,10) -
        -        KEYS(LENGTH,OFFSET) -
        -        NONUNIQUEKEY -
        - )
        - /*
    - Build the Index:
        - //sysin    dd *
        -    BLDINDEX INDATASET(ARI--1.RORY.REVAT.VSAM.KSDS) -
        -    OUTDATASET(ARI001.RORY.REVAT.VSAM.LOC.AIX)
    - Path - will allow the user to view the full record in the base cluster, depending on the Alternate key
        - We pass in the AIX name to the path entry
        - Steps
            - //sysin dd *
            - define path ( -
                - Name(ARI001.RORY.REVAT.VSAM.LOC.PATH) -
                - PATHENTRY(ARI001.RORY.REVAT.VSAM.LOG.AIX)
            )
    - Deleting just the base cluster, the AIX and path will also be deleted