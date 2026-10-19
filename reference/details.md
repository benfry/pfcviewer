# File Format of the PFC

The PFC starts with the letters `AOLVM100`, other files have other starting strings: e.g. the new files for the address book in AOL 6.0 (`*.ABX` and `*.ABI`) have an other format - like the `main.idx` - one file for the data (ABX), an other for the pointers (ABI = index). The ABX file starts with `AOLDB-25`, the ABI with `AOLINDEX`, but that is a feature for the future of my program. In AOL 7.0 the `*.ABX` moved to `*.ABY`, which has the same contents as the `*.ABX`, but with a simple key on the data parts to not see plain text when opening with an editor.

The four bytes at address `0x10` of the PFC are a pointer (little endian) to an index-table of longs (4 bytes each) that holds the absolute addresses of every line and folder you can see or those in closed folders, when you open your PFC. These lines (envelopes) all have a fixed length and are holding pointers to manage the order of the lines and to point to the data, you see, when you open the line - translated to absolute addresses via the index-table. Every object in this file starts with the letters `RS` for synchronisation and two binary Bytes of zero each for all except the index-table pointed to by address `0x10` - it has `01 00`.

Every object has its length in the next four bytes - after the four bytes of synchronisation - counting from the Byte after the length (for folders and envelopes it is `0x7E`).

General block as structure:

```c
char sync[4]; // 52 53 00 00
long length;
```

The index table then has a long, which may be working as flags. The next long is index 0 and holds a value of zero, because pointers with zero in the envelopes and folders are empty pointers. The next long (index 1) points to the root folder of this PFC. From there you can go via the pointers to the next- and child- folders to the description lines of mails. And from there you get a pointer to the contents of the mail, posting, ..., which then is made of subrecords, which are limited by the words `AOLH` at the beginning and `AOLF` at the end - except favorites, they have only the URL in the data field.

Index table as structure:

```c
char sync[4]; // 52 53 01 00
long length;
long count;   // count of elements in the folowing table (= length / 4 - 1) 
long table[];
```

After the length value of every envelope there are three binary numbers (16 bit wide), which tell the sort of it (folder, mail, newsgroup, favorite, ...), the `icon_ID` as long (found in `main.idx`) and some flags as long.

| Offset | Notes |
| --- | --- 
| 0x08 | a
| 0x0A | b
| 0x0C | c
| 0x0E | `Icon_ID` (zero for folders)
| 0x12 | 32 bit magic pattern, which is also stored in the first record of the data part of the mail, else zero
| 0x16 | flags

**Folders**

If the LSB of b is set it is a folder. If bit 8 is set it is a system folder, which cannot be deleted. The other bits tell, which system folder it is.
If the LSB of c is set, the folder is open, if it's cleared it's closed. The other bits of c also tell, which system folder it is.

| Values of a | | 
| --- | --- |
| 2 | Favorite
| 5 | File, prepared for download
| 6 | File, completely downloaded
| 7, 8 | Mail, received
| 12 | Mail, written
| 14, 15 | Posting, received
| 17 | Address book, single
| 18 | Address book, group
| 20 | Posting, sent
| 21 | Make new posting in this group
| 23 | Signature
| 24 | Buddy away
| 25 | Buddy preferences (since ver 7.0)
| 26 | Buddy icons (since ver 7.0)

| Flag bits | | 
| --- | --- |
| 0 | Seen
| 1 | Mail with attachment
| 2 | Self written
| 3 | Mail has picture

Then you can see the text of the line at offset 0x1A - padded with zero to get a length of 0x50 (= 80 Bytes) and split by TABs. The next two longs at offset 0x6A and 0x6E I didn't figure out (flags ?), but the following pointers are clear:
0x72: *data
0x76: *next
0x7A: *prev
0x7E: *parent
0x82: *child

With the child pointer you get one indent to the right and down in the graphical view, with the *parent you get one indent to the left and up. With the prev and next pointers you get up and down with the same indent (*prev of first and *next of last have zero). The data pointer tells you, where you can find the contents of the mail, posting, ... - all translated via the index-table. The data pointer of the root folder points to a field, which holds the filename of the actual cache file (the file with the envelopes only). Data pointers of main newsgroup and pinboard folders are pointing to a record with the newsgroup name for sorting with automatic AOL.

---

Free space in the PFC (if elements were deleted):

The number at address `0x22` (unsigned short) tells the amount of elements in the following list - the list of deleted elements (if it is zero, no cleanup can be performed):

```c
struct {
unsigned long pointer; // Start of deleted area (deleted folder, envelope, mail, ...)
unsined long len; // Length of deleted area (deleted folder, envelope, mail, ...)
} deleted_elements[]
```

…starts at address `0x24`.

---

The contents of a data record:

The text of a mail is one record also starting with `RS\0\0` and split into several subrecords starting with `AOLH` and ending with `AOLF`.

A data record starts (after the `RS\0\0` and the length) with `AOLH` followed by the length of this sub-block (including the starting `AOLH` and the final `AOLF`) as an unsigned long. The first subrecord holds the title, date/time, from:, to:, cc:, ...

These items are split and named by binary structures:

`RS..[length]AOLH[length2] [item1][type1][{length3}data1] [item2][type2][{length4}data2] ... AOLF`

```
short item;
byte type;
data (depends on type);
type 1: 1 bytes of data;
type 2: 2 bytes of data;
type 3: 2 bytes of data;
type 4: 4 bytes of data; 
type 5, 7, 8, 9: unsigned long length_of_following_data
```

Items:
2:  Number of subrecords in this block except this first one
3:  Holds the same magic pattern as the envelope
5:  Date/time:
6:  From:
7:  To:
8:  Cc:
9:  Bcc:
10: Subject:
15: Mail from outside AOL = 1; inside AOL = 0
16: Reply to: (??)
17: Recipient of this mail (??)
18: Mail from outside AOL = 1; inside AOL = 0
19: Count of To recipients
20: Count of Cc recipients
22: 1 for embedded picture
24: Date/time of the mail as 32 bits in seconds starting Jan 1st 1980 (for sorting in version 6.0 and higher)

The following subrecords only have the following items:

AOLH[length][item1=12][type1=2][type_of_data][item2=13][type2=7][length2][data2]AOLF

12: Type of data
13: The data itself

For the type of data:
0:   ASCII text (may be with HTML tags)
1:   Attachment (length, name)
3:   Header of posting
4:   Original From: ..., To: ..., ... (before 7.0)
5:   Header of the mail (Ver 7.0 and above)
256: Packed contents (Ver 7.0 and higher, packed with zlib)
257: Data for versions before 7.0 ("You need a higher version to read this")
260: End of data for versions before 7.0
270: Hyperlink (HTML tags for it)
272: AOL HTML tags
273: Picture (Embed + Binary Data)
275: Files for an attachment (where the files, that made the zip are on the hd)
276: AOL HTML (HeaderData)
281: Filenames of embedded binary data (Ver 7.0 and above)

---

Favorites are not made of subrecords - they only have the url in the data record.

char sync[4]; // 52 53 00 00
long length;
char url[];

---

Signatures, Buddy away:

Item 1: Description
Item 2: Contents

---

Downloaded files:

The items are:
3:  Date/Time of download (base is 1970)
5:  Holds the same magic pattern as the envelope
6:  Date/Time of the original file (as big endian, base = 1970)
7:  Length of file
8:  Length downloaded
9:  Unzipped files
10: Package
11: Subject
15: Filename
16: Destination folder
17: Stored as (if the name is duplicate in the folder)

---

Address book (before ver 6.0):

The items are (for single):
1: First name
2: Surname
3: Mail Address
4: Remarks

The items are (for group):
1: Name of the group
2: Mail Addresses

---

Written by Franz v. Gordon (email: fvongordon@aol.com) 17. Apr 2002
