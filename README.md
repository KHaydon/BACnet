# BACnet
BACnet info
This is the respository for documents/code and ideas for BACnet implementation of illum products


How to add a new service

Following steps give brief about adding a new service to BACnet. Before going into details some pre-requisites are as follows.
Pre-requisite #1: You must have good understanding of section 12, 13, 20 and 21 of the BACnet specification. It is must.
Pre-requisite #2: You should know architecture of source code.
Steps 

1) Copy source code of similar service. For writing unconfirmed service, copy existing unconfirmed service source code. The source code for a given service ideally consists of 4 files, for example
a. utm.c – containing encoding & decoding of APDU
b. utm.h – data structures and prototype declaration
c. h_utm.c – receive handler code. This is a callback function, which
decodes APDU. Further processing of IDU-ESMS specific should be
done here.
d. s_utm.c – send API for initiator, calls functions to encode APDU.

2) Then use the detailed description of the service entities given in Clause 21 of the BACnet standard. The trick is reading ASN.1 and figuring out the meaning of when to use context or application tags and when to use opening and closing context tags.

3) If the entity is listed with brackets [], then it is context tagged. There are separate functions for encoding application tags and separate functions for encoding of context tags. It is quite obvious that if encoding is done using context tags then decoding should be done using context tags.

4) If there are no brackets, it is application tagged.

5) If there is a complex data type (like ABSTRACT SYNTAX), then it uses opening and closing context tags around it.

6) Everything ends up being encoded with primitive data types using functions from bacdcode.c. This file contains encoding and decoding functions for all types of data e.g. unsigned, string, enumerated etc.

7) Header file should contain structure definition as per Clause 21. This clause also defines types of variables within that structure.

8) After writing the send and receive API, please register (or add) receive handler using “apdu_set_confirmed_handler” or “apdu_set_unconfirmed_handler” or other error handlers

9) Then start unit testing your code.

