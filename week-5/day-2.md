## CICS

### Logging On
- Instead of TSO, type CICSOZA
    - Enter same username and password
- CEMT
    - CICS Execute Master Terminal

### Batch vs Online
- Online systems have data entered as needed, so it is less predictable, programs and files can be shared and transactions can be run at any time.

### History
- Introduced by IBM in 1968
- Transaction Server
- CICS = Customer Information Control System
- VTAM, BTAM, TCAM are technologies used to interact with remote terminals

### What does CICS do?
- telecommunication
- multitasking

### CICS System Components
- Data-Communication Functions
- Data Handling Functions
- Application Program Services
- System Services
- Monitoring Functions

### System Services
- Program Control
- Storage Control
- Task Control

### Data Handling Functions
- Interface with data access methods
- Interface with database access methods
- Maintain data integrity

### Application Program Services
- Command Level Translator
- Interface with COBOL, PL/I, Assembler Programs
- Execution Diagnostic Facility
- Command Interpreter
- Screen Definition Facility

### Concepts of CICS
- Multitasking
- Multithreading
- Quasi-Reentrancy
    - Playing a game, alt-tab and the game pauses

### Terms
- Application - collection of programs
- Transaction - collection of logically related programs
- Task - single execution of some transaction
- ex: withdrawing money is a transaction made of the following tasks:
    - insert card
    - enter pin
    - choose language
    - choose account
    - withdraw/deposit

### CICS Nucleus
- Control Tables
- Control Programs

### CICS Control Programs and Tables
- Terminal Management
    - store info on terminal in Terminal Management table
    - TCP - Terminal Control Program
    - TCT - Terminal Control Table
    - TIOA - Terminal I/O Area
- Program Management
    - PCP - Program Control Program
    - PPT - Processing Program Table
- Storage Management
- Task Management
    - Task Control Program (KCP)
    - Program Control Table (PCT)
    - Task Control Area (TCA)
    - Execute Interface Block (EIB)
- File Management

### BMS
- BMS - Basic Mapping Support
    - can create your own screens

### Native CICS Commands
- CESN - CICS Execute Sign On
- CEDA - CICS Execute Definition and Administration
- CEMT - CICS Execute Master Terminal
- CEDF - CICS Execute Command Interpreter
- Read more [here](https://www.ibmmainframer.com/cics-tutorial/list-of-cics-transaction/)

### Commands in CICS
- EXEC CICS
    - option (argument)
    - option (argument)
- END-EXEC.

#### SEND COMMAND
- EXEC CICS SEND
    - FROM (dataname)
    - LENGTH(length of dataname)
- END-EXEC.

#### RECEIVE COMMAND
- EXEC CICS RECEIVE
    - INTO (ws-input)
    - LENGTH (length of ws-input)
- END-EXEC.

#### SEND PAGE COMMAND
- EXEC CICS SEND TEXT
    - FROM (dataname)
    - ACCUM
- END-EXEC.
- ...
- EXEC CICS SEND
    - PAGE
- END-EXEC.


### CEDA
#### CEDA DEFINE
- CEDA DEFINE (PROG)RAM
- CEDA DEFINE (TRANS)ACTION
- CEDA DEFINE FILE
- CEDA DEFINE (MA)PSET
- CEDA DEFINE DB2ENTRY
- CEDA DEFINE TD
    - TYPE: INTRA
#### FIELDS:
- Program name   : USERIDP
- TRANSID              : FIRST 2 CHARS AND LAST 2 CHARS( ARI001 à AR01)
- FILE NAME          : USERIDF
- MAPNAME          :USERID
- DB2 ENTRY         : USERIDP
- TDQ                      : TD and last 2 char of your userid
####
- CEDA DISPLAY
- Enter group name ALWYNGP

### TSO
- Create a PDS HLQ.NAME.REVAT.CICS.PDS
- 10,10,10,80,800,PDS
- Copy members from OZAGS1.ALWYN.REVAT.CICS.PDS
    - CICSCOMP
    - PGM1
- Open up CICSCOMP and change line 11 value to be your PDS
- Don't change line 12!
- Also change line 13 DSN to be your PDS with member name
- Line 15, NAME useridP(R)
    - ex: NAME ARI001P(R)
- sAVE AND SUBMIT PROGRAM

### CICS
- CEMT SET PROG (ARI001P) NE
    - Should get normal
    - Press F3 once
    - Use eraser on Toolbar
    - Enter Trans ID and hit enter
        - ex: AR15
    - Should see output
    