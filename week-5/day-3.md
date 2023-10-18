## CICS
### Exception Handling
- RESP PIC S9(8) COMP
- PUSH and POP
    - activating and deactivating the conditions
- HANDLE AID (Attention Identifiers)
    - Function Keys - Attention Identifiers
    - Example:
        - EXEC CICS HANDLE AID
            - PF1(PARA1)
            - PF2(PARA2) PF3(CPARA)
        - END-EXEC
### Basic Mapping Support:


## Maps
- Designing a screen is called a map
- Protected Field - cannot change/edit it
- Unprotected Field - can edit it
- We can design a screen and send to user
- User can enter data which can be received by the program

### BMS - Basic Mapping Support
- Map set name will be user id
- When your program has maps:
    - CEMT SET PROGRAM(ARI001*) NE
- DON'T CHANGE LOADLIB OR PROCLIB, JUST COPYLIB
    - COPYLIB - HLQ.NAME.REVAT.CICS.COPYLIB
        - 80,FB,PDS

### Map Example
- Revature Data Center
- Enter your Id: _______ Skipper
- Enter your Name: _______ Stopper
- Thank you -name- for coming (hidden message)
#### Open a Member
- ARI001   DFHMSD TYPE=MAP,LANGUAGE=COBOL,STORAGE=AUTO,TIOAPFX=YES,        X (72ND COLUMN)
    - MODE=INOUT
- EMPDTLS DFHMDI SIZE=(10,80),LINE=10,COLUMN=1,CTRL=(FREEKB,FRSET),        X
    - DSATTS=(COLOR,HIGHLIGHT),MAPATTS(COLOR,HIGHLIGHT)
-         DFHMDF POS=(1,30), INITIAL="REVATURE DATA CENTER",LENGTH=20,     X
    -             COLOR=RED,ATTRB=(PROT,ASKIP,BRT)
-         DFHMDF POS=(2,30), INITIAL="********************",LENGTH=20,     X
    -             COLOR=BLUE,ATTRB=(PROT,ASKIP,BRT)
-         DFHMDF POS=(4,5), INITIAL="ENTER YOUR ID",LENGTH=14,             X
    -             COLOR=WHITE,ATTRB=(PROT,ASKIP,NORMAL)
-         DFHMDF POS=(4,21), INITIAL="----",LENGTH=4,                      X
    -             COLOR=WHITE,ATTRB=(IC,UNPROT,fset,NORMAL)
-         DFHMDF POS=(4,26),LENGTH=1,ATTRB=(PROT,ASKIP,NORMAL)
- OZAGS1   DFHMSD TYPE=FINAL
- END
