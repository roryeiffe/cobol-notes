## Arrays
- Whichever variable has the search clause is the array
- Linear collection of data: internal table
- 01 WS01-VARS.
    - 05 WS05-TEMP PIC 9(02)V9(02) OCCURS 24 TIMES.
    - 05 WS05-SUB PIC 9(02) VALUE 2
- MOVE 17.98 TO WS05-TEMP(2).
- DISPLAY WS05-TEMP(2).
- MOVE 34.56 TO WS05-TEMP(WS05-SUB)
- INITIALIZE WS05-TEMP -> empty the array elements
### Weekly Weather Report
- Every week has 7 days, and every day has 24 hours
- This is a 2 dimensional array:
- 25 WEEKLY-TEMP-REPORT OCCURS 7 TIMES.
    - 30 TEMP PIC 9(02)V9(02) OCCURS 24 TIMES.
- 6-DIMENSIONAL ARRAY:
- 01-WS01-VARS
    - 07 CENTURY-TEMP-REPORT OCCURS 10 TIMES.
        - 08 DECADE-TEMP-REPORT OCCURS 10 TIMES.
            - 10 YEARLY-TEMP-REPORT OCCURS 12 TIMES.
                - 20 MONTHLY-TEMP-REPORT OCCURS 5 TIMES.
                    - 25 WEEKLY-TEMP-REPORT OCCURS 7 TIMES.
                        - 30 TEMP PIC 9(02)V9(02) OCCURS 24 TIMES.
- MOVE 19.78 TO TEMP(10,5,6,2,7,24) -> Move this value to the 10th decade, 5th year, 6th month, 2nd week of the month, 7th day of the week, and 24th hour of the day
### Rules for an array:
1. 01 level number CANNOT BE THE ARRAY. It cannot have OCCURS clause
2. An array must have a OCCURS clause
3. Array can be a group item OR a member item
4. An array element can be referred by a subscript variable 
5. An array element can be refered by the index variable if the array is INDEXED
6. The elements are from 1 to the size of the array
    - Ex: Array occurs 10 times, then the elements can be referred from 1 to 10
### SIMPLE ARRAY
- 01 WS01-VARS
    - 05 WS05-SUBSCRIPT PIC 9(03) VALUE 0.
    - 05 WS05-STUD OCCURS 10 TIMES
        - 10 STUD-ID        PIC 9(04).
        - 10 STUD-NAME      PIC A(05).
        - 10 STUD-MARKS     PIC 9(03).
### INDEXED ARRAY
- Whatever name mentioned in 'INDEXED BY' acts a subscript
- the indexed variable allows only the below operationS on that:
    - SET ALWYN TO X
    - SET ALWYN UP BY X
    - SET ALWYN DOWN BY X
- 01 WS01-VARS
    - 05 WS05-STUD OCCURS 10 TIMES INDEXED BY ALWYN.
        - 10 STUD-ID        PIC 9(04).
        - 10 STUD-NAME      PIC A(05).
        - 10 STUD-MARKS     PIC 9(03).
### Searching in Arrays:
1. Search Function (Table must be Indexed)
    1. Search
        - SEARCH ARRAY-NAME
        - AT END
            - IMPERATIVE STATEMENT
        - WHEN STUD-NAME(ALWYN) = 'TOMMY'
            - DISPLAY WS05-STUD(ALWYN)
        - END-SEARCH
    2. Search All
        - Note: this method applies binary search technique
            - But, the values in the array must be in sorted order, based on the key on which the search is done
        - SEARCH ALL ARRAY-NAME
        - AT END
            - IMPERATIVE STATEMENT
        - WHEN STUD-NAME(ALWYN) = 'TOMMY'
            - DISPLAY WS05-STUD(ALWYN)
        - END-SEARCH
    - Will only display one match if multiple matches exist
    - Reposition the index variable to the first value before starting the search
    - Note: The search function stops the search once a match is found. The duplicate records (if any) will not be searched
2. Manual Search
    - Subscript search
    - PERFORM VARYING WS05-SUBSCRIPT FROM 1 BY 1 UNTIL WS05-SUBSCRIPT > 5
        - EVALUATE TRUE
        - WHEN STUDENT-NAME(WS05-SUBSCRIPT) = 'JERRY'
            - MOVE 1 TO WS05-FOUND
            - DISPLAY 'RECORD FOUND!'
            - DISPLAY STUD-ID(WS-5-SUBSCRIPT)
            - DISPLAY STUD-NAME(WS-5-SUBSCRIPT)
            - DISPLAY STUD-PHONE(WS-5-SUBSCRIPT)
            - DISPLAY STUD-MARKS(WS-5-SUBSCRIPT)
        - WHEN OTHER
            - DISPLAY 'JERRY NOT FOUND'
        - END-EVALUATE
    - END-PERFORM.
    - IF WS05-FOUND = 0
        - DISPLAY 'JERRY NOT FOUND'
    - END-IF
        