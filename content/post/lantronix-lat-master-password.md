---
title: "Reverse Engineering the Lantronix ETS16P LAT Master Password"
date: 2023-09-25T06:32:30+02:00
---

I'm going to take my VAX 4000/105A to an upcoming retrocomputing
convention as part of my X.25 networking exhibit.  I would also like
to provide a couple of serial ports for other exhibitors to connect
their machines too, and for that, I have a Lantronix ETS16P terminal
server.  It is a relatively recent (late 1990ies) device with 16
serial ports on RJ45 connectors and support for TCP/IP and LAT.  LAT
is a proprietary DEC protocol for local terminal access, and it is
what I want to use for the VAX.

Unfortunately, the LAT support of the ETS16P is disabled by default.
To enable it, one needs to provide a license code, which back then was
an extra cost item.  Lantronix has ended support for the device in the
year 2000, but they still provide a last firmware update on
[their FTP server](ftp://ftp.lantronix.com/pub/ets16p/v3.6.4/).

I downloaded the update file to see whether I could figure out either
how the license code was generated so that I could generate one for
myself, or patch the firmware to completely disable the license
checks.  The firmware comes in the form of a file ETSP.SYS, and the
first thing to check is whether it is in some commonly known file
format:

```
mecipt-1 1_% file ETSP.SYS
ETSP.SYS: data
```

It is not, so I took a look at the data itself:

```
mecipt-1 2_% hexdump -C ETSP.SYS|head -20
00000000  16 67 4f f2 bb 7a d5 7d  00 00 00 00 ff ff 04 24  |.gO..z.}.......$|
00000010  04 24 00 00 01 01 00 00  40 40 56 00 01 00 15 00  |.$......@@V.....|
00000020  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
000000f0  01 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
00000100  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
*
00000200  01 50 00 03 00 00 00 00  00 00 00 00 00 00 00 00  |.P..............|
00000210  00 1c 02 0f 01 0b 00 00  00 08 97 3a 00 00 69 10  |...........:..i.|
00000220  00 00 2b 54 00 02 00 18  00 02 00 18 00 21 00 00  |..+T.........!..|
00000230  2e 74 65 78 74 00 00 00  00 02 00 18 00 02 00 18  |.text...........|
00000240  00 08 97 3a 00 00 00 a8  00 00 00 00 00 00 00 00  |...:............|
00000250  00 00 00 00 00 00 00 20  2e 64 61 74 61 00 00 00  |....... .data...|
00000260  00 21 00 00 00 21 00 00  00 00 69 10 00 08 97 e2  |.!...!....i.....|
00000270  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 40  |...............@|
00000280  2e 62 73 73 00 00 00 00  00 21 69 10 00 21 69 10  |.bss.....!i..!i.|
00000290  00 00 2b 54 00 00 00 00  00 00 00 00 00 00 00 00  |..+T............|
```

This looked promising, as there seemed to be something looking like a
normal object file header starting at an offset of 512 bytes.  Next I
checked see whether `file` recognizes the header:

```
mecipt-1 3_% dd if=ETSP.SYS of=etsp.o bs=512 skip=1
1153+0 records in
1153+0 records out
590336 bytes transferred in 0.008175 secs (72212355 bytes/sec)
mecipt-1 4_% file etsp.o
etsp.o: mc68k COFF object (demand paged)
```

Now I knew that the terminal server uses a Motorola 68000 CPU, which I
am familiar with and which is also well supported by
[Ghidra](https://github.com/NationalSecurityAgency/ghidra), the
reverse engineering tool that I intended to use in order to analyze
the binary.

Next, I checked whether any integrity checks are made by the boot
loader of the ETS16P before it accepts a firmware file and execute it.
There could be checksum of some sort which I had to update after
having made a code modification to the ETSP.SYS file.  I changed the
system prompt in the binary file using Emacs and tried to boot the
changed file, and to my relief I found it to just work.

Ghidra does not support the COFF file format, however, so before I
could really have a closer look, I needed to know into which memory
locations the individual sections of the COFF file are loaded into the
terminal server.  This is required by Ghidra so that it can properly
follow address references that are made in the code it analyzes.


The
[GNU binary utilities suite](https://www.gnu.org/software/binutils/)
(binutils) is a set of tools to work with object files and
executables.  The particular tool I needed is
[objdump](https://man7.org/linux/man-pages/man1/objdump.1.html), which
can display file headers, amongst other things.  The 68000 is ancient,
thus support for the architecture has been removed from the binutils
years ago.  The last version that would work is binutils-2.16.1.  With
it compiled (`./configure --target m68k-coff`), I could retrieve the
information I needed:

```
netzhansa 1_% m68k-coff-objdump -x etsp.o

etsp.coff:     file format coff-m68k
etsp.coff
architecture: m68k:68020, flags 0x00000102:
EXEC_P, D_PAGED
start address 0x0000000000020018

Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .text         0008973a  0000000000020018  0000000000020018  000000a8  2**2
                  CONTENTS, ALLOC, LOAD, CODE
  1 .data         00006910  0000000000210000  0000000000210000  000897e2  2**2
                  CONTENTS, ALLOC, LOAD, DATA
  2 .bss          00002b54  0000000000216910  0000000000216910  00000000  2**2
                  ALLOC
SYMBOL TABLE:
no symbols
```

For each of the segments in the file, `objdump` shows us the its size
(`Size`), the memory address into which the segment is loaded (`LMA`)
and the offset of the segment in the file (`File off`).  The `.text`
segment contains the program code and read-only data like strings.
`.data` contains preinitialized data items and `.bss` is an area with
data initialized to 0.

Equipped with this information, it was time to start Ghidra, open a
new project and import the binary file selecting "68000" in the
"Language and Compiler Specification" dialog.  In the Code Browser, I
defined the mapping from the object file to memory locations using the
"Memory Map" tool.  First, the automatically created "ram" section
needed to be removed, then I could define "text", "data" and "bss"
sections according to the `objdump` output.

With the memory map defined, I could run the "Auto Analyze" process of
Ghidra to make it disassemble and decompile the file.  It then was a
matter of using the string browser to search for "License" in order to
find the message "Incorrect LAT license string entered" and also the
decompiled C code from where the string was referenced:

```
undefined4 FUN_0003145e(undefined4 param_1,char **param_2)

{
  bool bVar1;
  int iVar2;
  undefined1 *puVar3;
  bool bVar4;
  
  bVar4 = **param_2 != 'S';
  bVar1 = false;
  puVar3 = &DAT_0021862c;
  if (!bVar4) {
    puVar3 = DAT_002102d0;
  }
  if (*param_2[3] == 'L') {
    if (bVar4) {
      iVar2 = FUN_000a7f24(param_2[4],s_26tn3wr_00031430);
      if (iVar2 == 0) {
        *(ushort *)(puVar3 + 0x7a) = *(ushort *)(puVar3 + 0x7a) | 0x80;
      }
      else {
        FUN_0003d3ee(s_Incorrect_LAT_license_string_ent_00031438);
        bVar1 = true;
      }
    }
```

I spent a couple of hours in Ghidra to turn this into something more
legible:

```
int set_or_define_license(int argc,char **argv)

{
  int res;
  undefined1 *status_base_pointer;
  bool is_define;
  bool failed;
  
  is_define = **argv != 'S';
  failed = false;
  status_base_pointer = &PROTO_PARAMS;
  if (!is_define) {
    status_base_pointer = GLOBALS_4;
  }
  if (*argv[3] == 'L') {
    if (is_define) {
      res = strcasecmp(argv[4],s_26tn3wr_00031430);
      if (res == 0) {
                    /* enable license */
        *(ushort *)(status_base_pointer + 0x7a) = *(ushort *)(status_base_pointer + 0x7a) | 0x80;
      }
      else {
        print_error(s_Incorrect_LAT_license_string_ent_00031438);
        failed = true;
      }
    }
```

As it seemed, the license check only consisted of comparing the
license entered against one statically compiled-in string.  If that
string was entered, the LAT license is enabled, otherwise the error
message is printed.  So I typed `SET PROTOCOL LAT LICENSE 26tn3wr`
into the terminal server, it accepted the command and after a reboot,
I could connect to the VAX using LAT.

My suspicion is that someone at Lantronix decided to basically replace
the previous license checking code with this statically compiled-in
version when the last version of the firmware was released and support
for the device was stopped.  I think this is a very applaudable
approach to deal with the end of support life of a device like this.
Sure, I still needed to reverse engineer my way to the password, but
the result makes me feel good about publishing it.  The ETS16P is
obsolete anyway, so I don't fear to get sued for this.

Working with Ghidra on the firmware felt like solving pieces of a
giant puzzle.  Even after I had figured out how the license checking
works, I spent more time naming functions that I recognized as being
implementations of C standard library functions and guessing how the
operating system kernel in the device works.  Ghidra is very usable,
but I feel I only scratched the surface of what it can do.
