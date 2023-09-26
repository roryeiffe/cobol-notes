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
        - RRDS:
            - FROMNUMBER(3) TOADDRESS(3)
    3. Dynamic -> Combination of random and sequential
        - Start at a random RBA/RRN/KEY and then reading consecutive records sequentially until EOF or another RBA/RRN/KEY
        - ESDS:
            - FROMADDRESS(400) TOADDRESS(752)
            - FROMADDRESS(400) -> TILL EOF
        - RRDS:
            - FROMNUMBER(3) TONUMBER(8)
            - FROMNUMBER(5) -> TILL EOF
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
            - REPRO INFILE (DDNAME) -
            -       OUTFILE (DDNAME) -
            -       SKIP(N) -
            -       COUNT(N)
        - NOTE: ESDS accepts any number of REPROs from same input dataset or from different input dataset   
    3. Print the data by giving its RBA
        - PRINT INFILE(DD1) -
        - CHAR -
        - FROMADDRESS(400) TOADDRESS(672)
- Cannot open an empty ESDS, will get MAXCC 12
- PRINT CH IDS(/) - will display records with RBA
    - Note that there is a record from 400-479 but not at 480
    - This is because the control interval is only 512 bytes, not enough room for 80 bytes at position 480
    - Think of someone who can't fit on the end of the bench, would have to move to the next bench (control interval)
- DELETE - will delete the ESDS
- What is a spanned record?
    - a record that is stored in more than one Control Interval
    - UNSPANNED is default, meaning we can't split records across control intervals
### 2. Relative Record Data Set RRDS
- Give the RRN and take the record
- Like chairs, each slot/chair can hold a person, rather than a bench
- First record takes first slot, second takes second, etc.
- Records are stored in empty slots
- Every slot is given a sequence number called Relative Record Number (RRN)
    - NUMBERED - this is the word that lets us create an RRDS
- How to create an RRDS
    1. Define
        - SYSIN DD *
            - DEFINE CLUSTER ( -
            - NAME(ARI001.RORY.REVAT.VSAM.RRDS) -
            - TRACKS(3,2) -
            - RECORDSIZE = (80,80) -
            - CONTROLINTERVALSIZE(512) -
            - NUMBERED -
            - )
    2. Repro
        - //SYSIN DD *
        - // REPRO INDATASET(ARI001.RORY.REVAT.VSAM.DATA) -
        -          OUTDATASET(ARI001.RORY.REVAT.VSAM.RRDS)
        -          SKIP(N)
        -          COUNT(N)
        - Other option:
            - REPRO INFILE (DDNAME) -
            -       OUTFILE (DDNAME)
    3. Print
        - //SYSIN DD *
            - INDATASET(rrds) -
            - char -
            - Fromnumber(x) tonumber(x)
        - sequential, random, and dynamic accepted
- Note: RRDS accepts REPRO ONLY ONCE
- It accepts duplicate records
- Skip(n) Count(m)
### 3. KEY SEQUENCED DATA SET (KSDS)
- (Give the key, take the record)
- Like a classroom with a bench, but we keep an index of who sits where, based on some identifier
    - ex: EMPID=4 sits here, EMPID=7 sits there
- Notes: the data field must be known even before defining the cluster
- Consider and define any unique part of the record as the key
- 1001 tommy Chennai 90000 9988987
- The REPRO is allowed only if the key column of the input dataset contains sorted records
- KSDS will have 2 components
    - index -> will contain the key of the record and the RBA of the record
    - data -> data
- How to set up a KSDS
    1. Define -     
        - //sysin dd *
        -   DEFINE CLUSTER (-
        - NAME(ARI001.RORY.REVAT.VSAM.KSDS) -
        - TRACKS(3,2) -
        - RECORDSIZE = (80,80) -
        - CONTROLINTERVALSIZE(512) -
        - FREESPACE(10,10)-
        - INDEXED -
        - KEY(4,0)) -
        -  -
        - )
        - OFFSET -> STARTPOS - 1
        - KEY(LENGTH,OFFSET)
        - FREESPACE(CI%,CA%)
    2. Repro
        - Repro is allowed any number of times
            - Provided:
                - No duplicate keys
                - No key out of sequence - the lowest key in the input dataset must be higher than the highest key in the ksds cluster
                - 
        - If data keys are out of sequence, will get MAXCC=12, out of sequence error
        - Duplicate key records are skipped (only one will show up)
        - //SYSIN DD *
        - // REPRO INDATASET(ARI001.RORY.REVAT.VSAM.DATA) -
        -          OUTDATASET(ARI001.RORY.REVAT.VSAM.KSDS)
        -          SKIP(N)
        -          COUNT(N)
        - Other option:
            - REPRO INFILE (DDNAME) -
            -       OUTFILE (DDNAME)
    3. Print
        - //SYSIN DD *
            - INDATASET(rrds) -
            - char -
            - FROMKEY(1001) TOKEY(2034)
        - sequential, random, and dynamic accepted 
### 4. Linear Data Set (LDS)
- LDS is like a warehouse where every record is dumped
- We will create LDS when we get to SQL
- How to set up n LDS
    1. Define -     
        - //sysin dd *
        -   DEFINE CLUSTER (-
        - NAME(ARI001.RORY.REVAT.VSAM.KSDS) -
        - TRACKS(3,2) -
        - LINEAR -
        - )
    2. Repro
        - Repro does not work with LDS
    3. Print
        - //SYSIN DD *
            - INDATASET(rrds) -
            - char -
            - FROMKEY(1001) TOKEY(2034)
        - sequential, random, and dynamic accepted 

### Using File Manager to Edit VSAM
- Under IBMTOOLS (9)
    - Go to FM (File Manager)  
    - Option 2
    - Type in name of file
- ESDS
    - New records cannot be inserted in ESDS
        - BUT you can append records at the end
    - Can modify record
    - Cannot delete any records (not even the last one)
- RRDS
    - Cannot insert record but can append record
    - Can delete a record
        - But the slot is still there, cannot be used
    - Record can be modified
- KSDS
    - Note that key is highlighted in a different color
    - We can insert a record
        - We can even insert a record with a key out of sequence, it will be auto-sorted
    - We can delete a record
        - Records are shifted, no empty slot
    - We can modify records
- When inserting into a KSDS
    - Free space is used when there is a control interval split or control area split
        - Helps to speed up the data processing
    - System sorts based on the key
    - If the CI is full, Control Interval Split happens:
        - CI split breaks a CI into 2 halves and moves the records in the later half into the new CI
        - The new record will be stored accordingly
    - A CI split may trigger a Control Area split
- KSDS 2 Index:
    1. Sequence Set - low level index for every control area: it contains the highest key of every CI in that CA
    2. Index Set - high level index for every sequence set: it contains the highest key of every sequence set

Control Interval
```
-----------------------------------------------------
| R1 = 80 | R2 = 80 | R3 = 80 | Unused | CIDF | RDS |
-----------------------------------------------------
```
RDF -> record description gield
    - the number of records of similar size
    - 80|2
    - 90
CIDF -> CI Description Field
    - RBA of the free space
    - Length of the free space


### IDCAMS Functions
- DEFINE - GDG, CLUSTER, AIX
- REPRO - copy data from a data set to another data set
    - from PS to ESDS, from ESDS to PS, from KSDS to ESDS, etc.
- PRINT
- DELETE
- ALTER - Modify the name of a cluster
- LISTCAT
- VERIFY - Ensures that the dataset is ready and available to be accessed
- BLDINDEX
- PATH ENTRY
- EXPORT -> CLUSTER CAN BE COMPRESSED AND WRITTEN IN A PS(GDG) FOR BACKUP (NOT HUMEN READABLE)
- IMPORT -> THE EXPORTED DATA CAN BE IMPORTED TO YOUR CLUSTER TO PROCESS