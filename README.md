QWIKSCAN

QWIKSCAN is a low level, high performance PDS scanning utility that will perform character-by-character comparisons of PDS members for up to 234 bytes of user data

```
                   QWIKSCAN                                             
            Maximised PDS Scanning                                      
```

# Contents

- Installing QUIKSCAN
  
- Structured Testing of QUIKSCAN
  
  - Testing
    
- QUIKSCAN Modes of Operation a Normal Locate
  
  - Super Locate
    
  - Mask Matching Fields  
    

# Introduction

QWIKSCAN is written in S/370 Assembler H and executes within an ISPF environment. QWIKSCAN requires ISPF Release 2 Version 0 or above to process, it also requires a terminal type of 3192-2 (or similar) or above.

Basically QWIKSCAN is a low level, high performance PDS scanning utility that will perform character-by-character comparisons of PDS members for up to 234 bytes of user data. User data is defined as:

- Up to 78 bytes of 'if' data
  
- Up to 78 bytes of 'and' data
  
- Up to 78 bytes of 'or' data
  

In addition to this the user may enhance the performance of QWIKSCAN by means of giving it delimiting 'stop after' instructions in either it's regular mode of operation or it's super-locate mode.

Whilst QWIKSCAN is running the users terminal is updated after the first 50 and every subsequent 50 PDS members are processed. The updated panel will tell the user how many members have been processed so far and how many successful matches have been located.

Assuming that at least one successful match has been made, a temporary ISPF table is updated with the member name, ISPF statistics, etc. and when the search completes this table will be presented to the user. They may then select any or all displayed members (multiple row selects are honoured.)

If a member is selected for processing, QWIKSCAN will initiate an ISPF BROWSE session with the nominated member(s). Prior to displaying the member, QWIKSCAN will pre-format the control line of the BROWSE panel with the command required to find the user 'if' character string. So, all the user has to do to locate the data they are interested in, is to press ENTER and use the RFIND key to display further occurrences, if any.

As a rough guide to performance, QWIKSCAN requires about 0.9 seconds, elapsed time, to search a PDS member fixed at 80 bytes, containing approximately 80 lines of data for a 4 byte search string (and not finding it.) Having said this the processing time is ultimately dependent upon machine type, IPS settings, allocated SU's, etc. It is possible, as explained later, to instruct QWIKSCAN to operate far quicker than this.
