## COBOL - Common Business Oriented Language
- Cannot submit a COBOL program
- COBOL created around 1958
- CODASYL - conference in the Pentagon building
- Funded by the American government
- Dr. Grace Hopper
- COBOL can run in any environment
    - IBM wanted to use COBOL
    - Needs to be converted to object code for the mainframe environment to understand
- Application Programming Language
    - Can include logic
    - Declare variables
    - ex: Add a to b giving c
    - Divide a by b giving d remainder e
    - Open, Read, Display
    - Loops/Iterations
1. Write the COBOL program
2. Submit the compile job on behalf of the COBOL program


### Program Structure
1. IDENTIFICATION DIVISION (mandatory)  
- Identifies the program
- 
- PROGRAM-ID  (mandatory)
- AUTHOR
- INSTALLATION
- DATE-WRITTEN
- DATE-COMPILED
- SECURITY
- REMARKS
2. ENVIRONMENT DIVISION (optional)
- Files and other external sources are linked here
- CONFIGURATION SECTION
    - SOURCE COMPUTER - zOS
    - OBJECT COMPUTER - 390
- INPUT-OUPUT SECTION
- FILE CONTROL -> where the files are linked
3. DATA DIVISION (optional)
- All variables are declared
- FILE SECTION
    - FD - FILE'S FIELD DESCRIPTION (FILE LAYOUT/FILE VARIABLES)
- WORKING-STORAGE SECTION
    - ALL THE COMMONLY USED VARIABLES ARE DECLARED HERE
- LOCAL-STORAGE SECTION
    - REPORTS
- LINKAGE SECTION
    - SUB PROGRAMS AND TO WHICH THE VALUES CAN BE RECEIVED FROM THE USER DYNAMICALLY
4. PROCEDURE DIVISION (mandatory)
- Has no predefined sections or paragraphs
    - Executable statements are written/verbs
    - Last statement of the procedure division must be 'STOP RUN.'

### Column Description of COBOL Program:
- 1-6 - SEQNUM
- 7 - INDICATOR -> executable sentence
    - '*' -> comment
    - '-' -> continuation of string from previous sentence
- 8-11 AREA/MARGIN A
    DIVISIONS, SECTIONS, PARAGRAPHS, FD,SD,01,77,ENTRIES
- 12-72 - AREA/MARGIN B
    - 02-49,66,88 LEVEL NUMBERS, ALL EXECUTABLE SENTENCE IN PROCEDURE DIVISION (VERBS)
- 73-80 - IDENTIFIER - USER'S DISCRETION

### Data Types:
1. Char -> A
2. Numeric -> 9
3. Alphanumeric -> X
#### DECLARE A VARIABLE
- SYNTAX: Level-number space name-var spaces picture-clause datatype(size) value-clause
- Level Number -> 01,77,02-49,66,88
- VAR-NAME(MAX 36) -, ALPHANUMERIC,-,FIRST CHAR MUST BE ALPHABETICAL
    - (let the name tell the story)
- PICTURE CLAUSE (PIC) (MANDATORY)
- DATATYPE(SIZE)
    - AAAAA
    - A(6) -> left-justified with auto spaces on the suffix
    - 9(4) -> right-justified with auto zeroes on the prefix
    - X(10) -> left-justified with auto spaces on the suffix
- VALUE (OPTIONAL) -> THE USER CAN ASSIGN THE VARIABLE WITH SOME INITIAL VALUES