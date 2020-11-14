# GRiD Convertible (a.k.a. AST PenExec) touchscreen driver reverse-engineering logs

**This is work-in-progress**

## Interrupts

### int 15h

Touchscreen & LCD display related functions

#### AH = 0xe4

##### AL = 0x28 - get / set video mode
* in:
  - DX - video mode (0xFF - to get current video mode)
    + bit 0 - display mode (1/0 - normal / reverse)
* out:
  - DX - current video mode if in(DX) was 0xFF

##### AL = 0x60 - ???
* in: -
* out: -

##### AL = 0x61 - ???
* in:
    BX - ??? (PENDRAW.EXE sets it to 0)
    CX - ??? (PENDRAW.EXE sets it to 0)

##### AL = 0x62 - ???
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
  - AX - raw x touch coordinate
  - BX - raw y touch coordinate
  - CX - x coordinate divider
  - DX - y coordinate divider

##### AL = 0x69 - ???
* in: -
* out: -

##### AL = 0x6C - ???
* in:
  - DX - ??? (PENDRAW.EXE sets it to 2)
  - CX - ??? (PENDRAW.EXE sets it to 0x78)
* out: -

##### AL = 0x7A - ???
* in:
  - CX - ??? (in "Near draw" mode, PENDRAW.EXE sets it to 0x8A, otherwise to 0xCC)
  - DX - ??? (PENDRAW.EXE sets it to 0)
out: -

## Device checks

PENDRAW.EXE checks whether it runs on the valid proper device. For this, it checks following values at the following addresses:
* F000:DFDC - 0x2D2D ("--")
* F000:DFFE - 0x24 ("$")

AST PenExec BIOS from 03/02/1993 contains string "--0000 <<-- ppaarrtt  nnuummbbeerr$" at the address f000:dfdc
