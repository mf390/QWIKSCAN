# QWIKSCAN

***Maximised PDS Scanning***

QWIKSCAN is a low level, high performance PDS scanning utility that will perform character-by-character comparisons of PDS members for up to 234 bytes of user data

# Contents

- Installing QUIKSCAN

- Structured Testing of QUIKSCAN
  
  - Testing

- QUIKSCAN Modes of Operation a Normal Locate
  
  - Super Locate
  
  - Mask Matching Fields  

# Introduction

QWIKSCAN is written in S/370 Assembler H and executes within an ISPF environment. QWIKSCAN requires ISPF Release 2 Version 0 or above to process, it also requires a terminal type of 3192-2  (or similar) or above.

Basically QWIKSCAN is a low level, high performance PDS scanning utility that will perform character-by-character  comparisons of PDS members for up to 234 bytes of user data.  User data is defined as:

- Up to 78 bytes of 'if' data

- Up to 78 bytes of 'and' data

- Up to 78 bytes of 'or' data

In addition to this the user may enhance the performance of QWIKSCAN by means of giving it delimiting 'stop after'  instructions in either it's regular mode of operation or it's  super-locate mode.

Whilst QWIKSCAN is running the users terminal is updated after the first 50 and every subsequent 50 PDS members are processed. The updated panel will tell the user how many members have been processed so far and how many successful matches have been  located.

Assuming that at least one successful match has been made, a temporary ISPF table is updated with the member name, ISPF statistics, etc. and when the search completes this table will be presented to the user. They may then select any or all displayed members (multiple row selects are honoured.)

If a member is selected for processing, QWIKSCAN will initiate an ISPF BROWSE session with the nominated member(s). Prior to displaying the member, QWIKSCAN will pre-format the control line of the BROWSE panel with the command required to find the user 'if' character string. So, all the user has to do to locate the data they are interested in, is to press ENTER and use the RFIND key to display further occurrences, if any.

As a rough guide to performance, QWIKSCAN requires about 0.9 seconds, elapsed time, to search a PDS member fixed at 80 bytes, containing approximately 80 lines of data for a 4 byte search string (and not finding it.) Having said this the processing time is ultimately dependent upon machine type, IPS settings, allocated SU's, etc. It is possible, as explained later, to instruct QWIKSCAN to operate far quicker than this.

# Installing QWIKSCAN

The following PDS members should be obtained and copied to their respective datasets (in the case of load libraries, the source must first be assembled):

| Name     | Library            |
| -------- | ------------------ |
| QWIKSCN1 | ISPF Panel Dataset |
| QWIKSCN2 | ISPF Panel Dataset |
| QWIKSCN3 | ISPF Panel Dataset |

| Name    | Library                      |
| ------- | ---------------------------- |
| QWIKS00 | ISPF         Message Dataset |
| QWIKS01 | ISPF Message Dataset         |

| Name     | Library           |
| -------- | ----------------- |
| QWIKSCAN | ISPF Load Dataset |
| QWKF     | ISPF Load Dataset |
| QWIK     | ISPF Load Dataset |

| Name | Library         |
| ---- | --------------- |
| QWIK | SYSEXEC Dataset |

| Name    | Library     |
| ------- | ----------- |
| QWIKASM | JCL Dataset |

Once this has been done you are now ready to perform a series  
of structured tests of QWIKSCAN.

# Structured Testing of QWIKSCAN

Now that you have installed the various modules to their correct locations it is possible to determine if QWIKSCAN is functioning properly. Follow the series of tests described below, assuming that everything is working correctly you may now use QWIKSCAN however you wish. For the purpose of these test you must know the name of the ISPF Message Library, this contains 2 QWIKSCAN members and will be the target of the tests.

## Testing

1. Exit and re-initialise ISPF. This will force the inclusion of all new ISPF objects
2. From ISPF option 6 enter QWIKSCAN
3. The primary QWIKSCAN panel will be displayed
4. Enter the following values in the fields:
   1. The PDS name which is your ISPF Message Library 
   2. ALARM which is the string to search for
5. Now press ENTER
6. The dialog will respond with the message: Enter GO to start
7. Enter GO in the command field
8. After a second or two, the second QWIKSCAN will be displayed showing 4 table entries. The ISPF LMSG will display:
   1. 'Scanned 4 members, matched 4 members'
9. Enter an 'S' in each of the 'SEL' fields and press ENTER
10. QWIKSCAN will initiate a browse for each of the selected members
11. The command 'F ALARM' will be formatted in the command line
12. Press PF3 until you are back at the first QWIKSCAN screen
13. The values you entered will still be displayed on the panel
14. In the STATS mask field 'CREATED' enter '**/' and enter GO on the command line
15. After a second or two the 'IF' data field will start to blink and the message: 'No data located' will be displayed

Assuming that all of these tests met with the expected results it is probable that QWIKSCAN is working okay.

# QWIKSCAN Modes of Operation

QWIKSCAN operates in two modes, normal locate and super locate. The difference between the two modes is that normal locate uses internal delimiting logic whilst super locate uses ISPF statistics to enhance the performance of the program.

In order to use super locate the user is expected to know more than just what data string is to be located.

## Normal Locate

Normal locate isn't an inefficient method of scanning a PDS. It does imply though that a 'full scan' is performed. There are 6 fields used in normal locate, these are as follows:

**TARGET PDS NAME** The name of the PDS that is to be scanned. Maximum of 42 characters. No quotes  

**IF DATA STRING** Up to 78 bytes of character data (upper case) that defines the primary search argument. The program will halt processing the current PDS member when it finds the first occurrence of this string. Generally the longer the string the more efficient the program will be. Imbedded and multiple blanks are allowed. There are no requirements for the string to be suffixed or prefixed with special characters within the target PDS  

**AND DATA STRING** Same rules as 'IF'. If used the program will check that the 'AND' data does not exist on the same record as the 'IF' data and not a member level  

**NOT DATA STRING** Same rules as 'IF'. If used the program will check that the 'NOT' data does not exist on the same record as the 'IF' and/or 'AND' data

**MAX MEMBER**S 4 byte numeric field that tells the program to stop processing the PDS after n members  have been scanned  

**MAX RECORD**S 4 byte numeric filed that tells the program to stop processing the current member after n records have been processed

### Super Locate

Super locate is used in conjunction with normal locate to enhance the programs ability to scan a PDS. The implication of using super locate is that not all members will be selected for scanning. The reason for this is that user super locate values are masked against ISPF PDS member statistics. If the mask fails then the member will not be selected for further processing, i.e. it will not be opened and read.

In addition to providing the equal-to mask values it is possible to code two not-equal-to mask values for the created  and updated fields. This option should be used when ISPF statistics do not exist for some members and those are the members that you are interested in.

For instance, assume that you are interested in PDS members that have been created by an application as opposed to a user, no statistics will exist for them. To tell QWIKSCAN that these are the members to scan, simply code: '**/**/**', in either (or both) the two date fields, you may also code part of this string as it is a mask that will be applied to the actual data.

One important point to understand when using super locate mask matching is that because you are applying a mask it is not necessary to conform to any syntactical standards. Another point to note is that QWIKSCAN will rearrange the dates into the European standard of dd/mm/yy, so any date masking should reflect this. For instance should you only be interested in looking at members updated in July 1999, you would code '/07/99' in the updated field. You could also code '/07/' but this wouldn't guarantee that you only got 1999 data.

### Mask Matching Fields

USER Any fully qualified user name, partially qualified 
to the left, right or middle allowed. For instance if the target was Userid USER1 the following would pick it up: 

    USER  

    US

    USER1 

If you are searching your own PDS, always use this field  

**CREATE**: The  date  the  member  was  created  in  the  format dd/mm/yy.  If  statistics do not exist for  a  member the  value `**/**/**` will be inserted by QWIKSCAN. For example, to pick up members created in July 1999  any of the following will work:

`

    **/                                                               
    /**/                                                              
    *                                                                 
    */**/*    `

**UPDATE**: As for CREATE but probably the better field to use  
**MEMBER**: The member name mask, its efficiency is enhanced by the proper naming standards

- SORRY...NO WARRANTY BUT QUESTIONS OR INQUIRIES CAN BE DIRECTED TO: SEBASTIAN§WELTON.DE

