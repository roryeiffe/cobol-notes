## Topics Today
- Character Manipulations
- Usage Clause
- Condition Names
- Edited Picture Clause
- Renames
- Redefines
- Intrinsic Functions
- Sort Function - obsolete

### String/Character Manipulation
1. String - Multiple input variables (Char data type) can be concatenated into a single destination variable
    - WS05-FNAME
    - WS05-MNAME
    - WS05-LNAME
    - WS05-NAME -> 
2. Unstring - From a single source variable, the data can be split into multiple destination variables
3. Inspect - allows you to check or count the number of characters in a variable
    - Find and replace a character in a variable
#### Delimited by
- Delimiter - define the limit/bounds of the string we are concatenating
- By Size - split based on the size of the variable
- By Spaces - will split until a space occurs
- By "frogs" - will split when that corresponding string appears

#### With Pointer
- determines the starting character position for insertion into the destination string
- Before starting the string function, helps to locate the position
- After the string function the value in the pointer will be automatically updated to the next empty position
- The pointer - 1 represents the number of characters that were deposited in the string function
- But, if you perform all the operations in a single STRING function, you don't need the "WITH POINTER" clause

### Usage Clause
- COMP variables
- A special clause that can be mentioned while declaring variables. It can be used only with numeric data. It compresses the data stores in lesser bytes. However, using them in arithmetic operations doesn't have any adverse affect on the data
- Comp -> pic (9(01) - 9(04)) - 2 bytes
-         pic (9(05) - 9(08)) - 4 bytes
-         pic (9(09) - 9(18)) - 8 bytes
- Syntax:
    - WS05-VAR PIC 9(04) VALUE 123 USAGE IS COMP

- Comp-1 -> No PIC clause must be given, single precision FLOATING POINT
- Comp-2 -> No PIC clause must be given, double precision FLOATING POINT
- Ex:
    - 05 WS05-TAX COMP-2.
- Comp-3
    - Bytes used: N/2 + 1
    - Ex: 05 WS05-VAR3 PIC 9(07) COMP-3
- 1.0000098 x 10**-23
- Comp-4 - Figure out what this is for HW
- Comp-5 - Figure out what this is for HW

### Condition Names
- Definition: It can be considered as a user-defined reserved word in COBOL
- 88 Level Number, Margin B
- Must immediately follow the variable for which it is defined
- 05 WS05-AGE PIC 9(03).
- EVALUATE TRUE
- WHEN WS05-AGE 0 THRU 2
    - DISPLAY 'INFANT'
- WHEN WS05-AGE 3 THRU 12
    - DISPLAY 'TODDLER'
- WHEN WS05-AGE 13 THRU 19
    - DISPLAY 'TEEN'
- WHEN WS05-AGE 20 THRU 59
    - DISPLAY 'ADULT'
- WHEN WS05-AGE 60 THRU 99
    - DISPLAY 'SR.CTZN'
- WHEN OTHER
    - DISPLAY 'ANGEL'
- END-EVALUATE

- Condition Names:
- 05 WS05-AGE PIC 9(03).
    - 88 INFANT     VALUE 0 THRU 2
    - 88 TODDLER    VALUE 3 THRU 12
    - 88 TEEN       VALUE 13 THRU 19
    - 88 ADULT      VALUE 20 THRU 59
    - 88 SR-CTZN     VALUE 60 THRU 99
- 05 WS05-GENDER PIC A(01).
    - 88 MALE VALUE 'M'
    - 88 FEMALE VALUE 'F'
    - 88 NONBIN VALUE 'N'

- 05-WS05-FST-TI001 PIC 9(02).
    - 88 TI001-EOF              VALUE 10
    - 88 TI001-SUCC             VALUE 00
    - 88 TI001-ATT-MIS-MATCH    VALUE 39

- OPEN INPUT TI001-PS
    - EVALUATE TRUE
    - WHEN TI001-SUCC
        - DISPLAY 'TI001 OPENED'
    - WHEN OTHER
        - DISPLAY 'OPEN FAILED'
        - PERFORM 9000-TERM-PARA
    - END-EVALUATE
### COBOL INTRINSIC FUNCTIONS
- https://www.mainframestechhelp.com/tutorials/cobol/intrinsic-functions.htm

### Edited Picture Clause
- It would be nice to have things like commas, decimals, stars, and other characters in our numbers
- Also leading zero suppression
    - Imagine a bill like 000000010.99
- Use move statement to transfer data to edit item
- Z - zero suppression
- B - blank space
- asterisk - stars
- / - put a slash in the edited data, used for dates
- 0 - put a zero, will not be replaced
- CR/DB - credit/debit, will only appear if value is negative
- $


## Assignment Assistance
- Applications Program - Cooking Data for the output file
- Read from a PS -> Validations -> Calculations