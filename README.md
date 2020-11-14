# GRiD Convertible (a.k.a. AST PenExec) touchscreen driver reverse-engineering logs

**This is work-in-progress**

## Interrupts

### int 15h

#### AH = 0xe4

Touchscreen & LCD display related functions

##### AL = 0x28 - get / set video mode
* in:
  - DX - video mode (0xFF - to get current video mode)
    + bit 0 - display mode (1/0 - normal / reverse)
* out:
  - DX - current video mode (only if DX was 0xFF ?)

##### AL = 0x60 - ??? used in initialization
* in: -
* out: -

##### AL = 0x61 - ??? used in initialization
* in:
  - BX - ??? (PENDRAW.EXE sets it to 0)
  - CX - ??? (PENDRAW.EXE sets it to 0)

##### AL = 0x62 - ??? used in finalization
* in: -
* out: -

##### AL = 0x64 - get raw touch coordinates
* in: -
* out:
  - AX - stylus button pressed (0 = no switch (?), 1 = tip switch, 2 = barrel switch (?))
  - BX - raw X touch coordinate
  - CX - raw Y touch coordinate
  - DX - flag
    + bit 8 - ???

##### AL = 0x66 - get touchscreen parameters/calibration (?)
* in: -
* out:
  - AX - minimum raw X touch coordinate
  - BX - minimum raw Y touch coordinate
  - CX - maximum raw X touch coordinate
  - DX - maximum raw Y touch coordinate

##### AL = 0x69 - ??? used to "acknowledge" touch event
* in: -
* out: -

##### AL = 0x6C - ??? used in initialization
* in:
  - DX - ??? (PENDRAW.EXE sets it to 2)
  - CX - ??? (PENDRAW.EXE sets it to 0x78)
* out: -

##### AL = 0x7A - ???
* in:
  - CX - ??? (in "Near draw" mode, PENDRAW.EXE sets it to 0x8A, otherwise to 0xCC)
  - DX - ??? (PENDRAW.EXE sets it to 0)
out: -

##### AL = 0xC3 - Get BIOS language
* in:
  - DL - 0xFF default value
* out:
  - DL - current BIOS language
     0 US   1 CF   2 LA   3 NL   4 BE
     5 FR   6 SP   7 IT   8 SF   9 SG
    10 UK  11 DK  12 SV  13 NO  14 GR
    15 PO  16 SU  17 X1  18 X2  19 X3
    20 X4  21 X5  22 X6  23 X7  24 X8
    25 X9

## Device checks

PENDRAW.EXE checks whether it runs on the valid proper device. For this, it checks following values at the following addresses:
* F000:DFDC - 0x2D2D ("--")
* F000:DFFE - 0x24 ("$")

AST PenExec BIOS from 03/02/1993 contains string "--0000 <<-- ppaarrtt  nnuummbbeerr$" at the address f000:dfdc
