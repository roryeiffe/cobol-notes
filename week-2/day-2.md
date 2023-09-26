## Questions
- How do you know that PS had the wrong record length?
    - Use i for detailed information about the file
    - With some experience, able to predict that this was the issue

## VSAM
- PS -> Physical Sequential File/Flat File
    - STPOS,LENGTH
    - This is a pretty primitive way to write data
- PDS -> Partitioned Data Set, like a folder
    - PDS Members are similar to PS, we can edit them in similar ways
    - We write jobs in PDS members and we write data in PS 
- We want some other way to access data besides stpos, etc.
- VIRTUAL STORAGE ACCESS METHODS
- Four different ways of accessing data
- Four different types of data sets, each with their own way of storing/accessing data
- If we can store the data in an organized way, we will have a more efficient fetch
- Catalogued -> Store in the main directory
- VSAM Data Sets are catalogued in a different way as well
    - Still stored in main directory, but also
    - Meta Data - information about the data set is stored
    - When we store a record, the record itself is stored but also information about the record
- Cannot open VSAM data sets normally (using E)
- VSAM Data Sets are referred to as clusters
    - RECFM/LRECL -> RECORD SIZE
    - BLKSIZE -> CONTROL INTERVAL(CI)
    - SPACE -> TRACK/CYLINDER(PQ,SQ)
- Cluster - a collection of Control Areas
- Control Area - a collection of Control Intervals
- Control Records - a collection of records
- Imagine a school:
    - School = Cluster
    - Classroom = Control Area
    - Row of Desks = Control Interval
    - Desk = Record
- IDCAMS (Integrated Data Catalogue/Clusters Access Method Services)
- Cluster Allocation Parameters
    - DSN = Data Set Name = HLQ.Q2.Q3, same naming conventions
    - VOLUME/DEVICE/SERIAL - we won't have to mention this but some environments/situations require you to
    - SPACE UNIT -> Tracks(PQ,SQ) How many times will SQ be extended? 123?
        - https://www.ibm.com/docs/en/zos/2.1.0?topic=sets-using-vsam-extents
    - ControlIntervalSize/CISIZE(512 bytes) multiples of 512 up to 8 kb
        - Once 8kb is reached, must increase in increments of 2kb, max is 32 kb
    - RECORDSIZE(AVG,MAX) - a combination of RECFM and LRECL
        - The system can infer the LRECL and Record Format
        - (80,80) - F (fixed)
        - (20,80) - V
    - INDEXED/NONINDEXED/NUMBERED/LINEAR - which type of data set is it going to be
- Access Methods
    1. Sequential -> default -> from the first record till EOF (End Of File)
        - Don't need an RBA, key, RRN or anything
    2. Random -> Can fetch any one record with is RBA, key, or RRN
        - ESDS:
            - FROMADDRESS(80) TOADDRESS(80)
            - FROMADDRESS(50) -> terminated (50 is not a multiple of 80, which is our record size)
    3. Dynamic -> Combination of random and sequential
        - Start at a random RBA/RRN/KEY and then reading consecutive records sequentially until EOF or another RBA/RRN/KEY
        - ESDS:
            - FROMADDRESS(400) TOADDRESS(752)
            - FROMADDRESS(400) -> TILL EOF
### 1. Entry Sequenced Data Set (ESDS)
- Give the RBA and retrieve the data
- The records are stored in the sequence in which they are entered
- Every record is organized by giving it a unique RBA
- RBA - Relative Byte Address - sum of the sizes of the previous records in the control interval
- Think of people lining up on a bench
- 0-79|80-159|160-239
- Record 1 - rba: 0
- Record 2 - rba: 80
- Record 3 - rba: 160
- Record 4 - rba: 240
- When querying for a record, we give the RBA, we can't, for example, ask for all records whose location is "New Jersey"
- How to create an ESDS:
    1. Define a cluster
        - //jobcard
        - //STEP1 EXEC PGM=IDCAMS
        - //SYSPRINT DD SYSOUT=*
        - SYSOUT DD SYSOUT=*
        - SYSIN DD *
            - DEFINE CLUSTER ( -
            - NAME(ARI001.RORY.REVAT.VSAM.ESDS) -
            - TRACKS(3,2) -
            - RECORDSIZE = (80,80) -
            - CONTROLINTERVALSIZE(512) -
            - NONINDEXED -
            - )
    2. Repro - populates the dataset with data from another data set
        - Like IEBGENER but for VSAM data sets
        - //SYSIN DD *
        - // REPRO INDATASET(ARI001.RORY.REVAT.VSAM.DATA) -
        -          OUTDATASET(ARI001.RORY.REVAT.VSAM.ESDS)
        - Other option:
            - REPO INFILE (DDNAME) -
            -      OUTFILE (DDNAME)
        - NOTE: ESDS accepts any number of REPROs from same input dataset or from different input dataset   
    3. Print the data by giving its RBA
        - 
- Cannot open an empty ESDS, will get MAXCC 12
- PRINT CH IDS(/) - will display records with RBA
    - Note that there is a record from 400-479 but not at 480
    - This is because the control interval is only 512 bytes, not enough room for 80 bytes at position 480
    - Think of someone who can't fit on the end of the bench, would have to move to the next bench (control interval)
- DELETE - will delete the ESDS
- What is a spanned record?
    - a record that is stored in more than one Control Interval
    - UNSPANNED is default, meaning we can't split records across control intervals
### 2. 

### 3. 

### 4. 