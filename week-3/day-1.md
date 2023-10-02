## Paragraphs
- Paragraph starts in margin A
    - 1000-INIT-PARA
    - 3000-PROC-PARA
    - 4000-TERM-PARA

## Perform Statements
- Out of the line Perform statement
    - When a perform statement performs/calls a paragraph that is/not in the sequence of the statements. The perform statement jumps to the paragraph and executes the statements in the paragraph and after doing so it comes back to the next from where it went.

## Move
- Move corresponding grp1 to grp2
    - It will move the values from source members to the destination members ONLY IF THEIR NAME MATCHES, irrespective of their sequence
    - If trying to move group to another with no matching member names, it will have no effect
- Move grp1 to grp2
    - It moves values in the sequence

## Copy Statements
- A group of reusable statements can be written in any PDS member. 
    - They can be COPY'd into any division of your COBOL program
- Note: We need to mention the PDS in which the copy member is kept, in the compile JCL
    - COMPILE.SYSLIB DD 
    - The member is called a COPY BOOK
    - The PDS is called COPY LIBRARY
    - Can put the skeleton in a COPY BOOK and copy it every time

## Control Statements

