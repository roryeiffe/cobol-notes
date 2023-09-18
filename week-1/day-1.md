## Mainframe
- Mainframe is a gigantic computer
- 1000s of users can access it, it is a server
- Once we access it, it is like a browser, can access data that is stored -somewhere-
- We need an emulator to access mainframe
    - Emulator reflects what is happening on the mainframe server
- PC acts as a dummy
- 3270 is a dumb terminal
- From here, we will send commands to mainframe server and get the output
- Benefits
    - Availability
        - Programs in Mainframe machines that are running non-stop for 30 years
        - 7 9's = 99.9999999% of the time it will work
    - Security
        - Virus 
            - occupies more memory
            - executes some unwanted program so that processor isn't able to do what you want it to do
            - exe file that keeps executing in the processor
            - In mainframe, we don't have .exe files, no extensions
        - If you write a job, you can submit it
        - Data - records
        - Job - a program, will only execute when you want it to execute
            - the time that a job will run is also allocated ahead of time
        - Cannot be a program that is running infinitely
        - ex: the 30-year program is given very special privileges
        - Statically allocated memory, when allocating data set, specify how much space we want for the file
            - We will learn how to measure sizes ahead of time
    - Allows us to process enormous amounts of data
    - 20 years back, TBs of memory
    - COBOL supports 7-dimensional array and was invented in 1950's :O
- MVS - Multiple Virtual Storage
- z/OS
    - z - zero down-time
### SuperComputer vs Mainframe
- Supercomputer is very complicated processes but data is limited
- Mainframe can process unlimited data but processing is simple
    - Think of a bank, we only do addition, subtraction, multiplication, division
    - Don't do crazy stuff like sine, cosine, fourier transformations  
    - Millions of transactions per day/minute


- Files - Datasets - Flat - PS (Physical Sequential)
- Folders - Partitioned DataSet - PDS
    - Folder cannot have any folders inside
    - Files in a folder are called members
- Job - Program
- JCL - Job Control Language

- Mainframe 
- Some softwares that come with it
- Every mainframe is different/customized from one another
- Softwares - Subsystems - takes care of a particular work
    1. TSO - Time Sharing Options - login, allows us to logon to Mainframe
    2. ISPF/PDF - Interactive System Productivity Facility/Program Development Facility, the interactive system that lets us choose different options, lots of menus
    3. JES - Job Entry Subsystem - receives a job from the user, takes care of all the work
    4. DB2 - RDBMS Mainframe
    5. CICS - Customer Information Control System
        - Allows us to have OLTP (can interact with system while it is running)
    6. IMS - Information Management System - not covered in this training
    1. RACF, SMF, SMS - many other utilities for admin

- Batch Processing
    - When the program is being executed, user cannot interact with the system
    - Mainframe is batch processing

- OLTP - Online Transaction Processing
    - Online not as in referring to the internet
    - Python, Java, R are transaction processing
    - Allows user to interact with the program while the program is under execution, like reading input from the terminal

- Memory - 
    - Main - RAM - size of RAM comes with the version of Mainframe
    - Secondary Memory - 
        1. DASD - Direct Access Storage Device - 3390 device, similar to hard-disk
        2. TAPE - Audio Tapes/Video Cassettes, VHS, plastic film used for backup
            - If you want bank statements for more than a couple years, need a few days to get that data
                - needs authorization, authentication, etc.
        - Partitioned into volumes
        - Volume is a partition of a DASD
            - like C drive, D drive, etc.
        - When creating data sets, need to specify which volume it would be allocated in

- Data Set Allocation Parameters
    1. DSN - Dataset Name - not as easy as you think
        - HLQ.Q2.Q3.Q4.Q5.Q6.Q7
        - HQL - High Level Qualifier -> user MF id/project id (ex: ARI001)
            - If you give an HLQ with somebody else's id, breaching security
        - 7 qualifiers max
            - this rule is overruled in some versions of mainframe
        - 2 minimum
        - Rory Eiffe (like first name and last name are qualifiers)
        - Qualifiers can 
            - have 8 chars max
            - first character must be alphabetical
            - after first, can be alphanumeric
        - Every qualifier is separated by period
        - Total number of chars: 44
        - Make sure to give meaningful name to files
        - By convention, make 2nd qualifier be your name
        - ex: OZAGS1.ALWYN.OUTPUT.DATA
            - Let the name tell the story
        - Can also include whether it is PS vs PDS
            - Remember, these are not extensions
    2. SPACE UNITS - 
        - Bytes, MB, KB, TRACKS, CYL
        - How many MBs make a track?
        - How many tracks make a cylinder
    3. Primary Quantity - 
        - Is is the initial amount of space units that you're trying to request from the system
        - ex: 3 tracks, 3 MB, 3000 cylinders
        - We go by an initial guess
    4. Secondary Quantity - 
        - Represents the number of space units that chat that can be allocated when the previously allocated space units are fully utilized
        - 2 x 15 times
        - SPACE ABEND
    5. Directory Blocks: folder (PDS, LIBRARY, PDSE)
        - It represents the max numbers of members inside the folder
            - (N X 6 ) - 1
    6. Volume/Serial (optional)
    7. Logical RECord Length (LRECL)
        - number of characters for each line
        - Min = 1, Max num = 32 KB per line
        - Job/COBOL program - 80
        - Dataset - depending on the num of chars per record
    8. BLKSIZE = multiples of LRECL
        - Unit/min amount of data transfer between primary, secondary, and the main memory
            - Helps to improve efficiency
            - Comparison to withdrawing money, you always take more than you need because it's likely you'll need it
                - Same with data, 80% chance when retrieving data that you will also need the next record
    9. RECFM - RECord ForMat
        - F - FIXED
        - FB - FIXED BLOCK
        - FBA - FIXED BLOCK ADDRESSING
        - V - VARYING Record Length
        - VB - VARYING BLOCK SIZE
        - VBA - VARYING BLOCK SIZE ADDRESSING
        - U - UNFORMATTED/UNKNOWN -> used to load modules, not human readable formats
    10. DSORG/DS-TYPE - PS or PDS
        - Empty - PS
        - PDS, PDSE, LIBRARY
    11. EXPIRY - give an expiration date
        - at the end of that particular date, system will delete this file
        - DAYS 200 -
        - Can also give a date
    - ISPF panel, 
        - choose option 3-> Utilities
        - 2 -> Data Set
        - A -> Allocate
        - Fill in parameters
        - Enter name and hit enter
    - Now, to see the data set
        - 3 -> Utilities
        - 4 -> DSList
        - Enter first 2 qualifiers and hit enter, should see the data set
    - To delete file
        - Move cursor to command section on the same line of file
        - Type d and hit enter
    - File Commands
        - D - Delete
        - R - Rename
        - E - Open in edit mode - can make edits and save them
        - V - Open in view mode - can make changes to data but can't actually save them
        - B - Browse Mode - read-only
        - I - Information about the file
        - S - Short Info

        PDS
        - C - Copy a Member
        - Z - Compress

- TSO Edit Commands
    - I - Insert
    - D - Delete
    - 
    
- Edit Records


