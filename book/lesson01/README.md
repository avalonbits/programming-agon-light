# Lesson 01: The minimal working program.

For our first assembly program, we will create the equivalent of the following C code:

```c
int main(void) {
  return 0;
}
```

The corresponding assembly code would then be:

```assembly
    ; Tell the assembler we want eZ80 24-bit address mode.
    .assume ADL = 1

    ; User memory starts at $040000
    .org $40000

    ; Jump over the header code
    JP main

    ; Define the standard MOS header, which must start at byte 64.
    .align 64

    .db "MOS"   ; This tells MOS it is load a MOS program.
    .db 00h     ; The version of the MOS header, currently 0.
    .db 01h     ; CPU run mode. 0 means z80 and 1 means eZ80.

main:
    LD hl, 0
    ret
```

Pretty short, tough not as short as the C version. Let's understand  what is happening
on each line/block of code.

## The eZ80 run mode and user memory.

The Agon Light uses an [eZ80F92 microcontroller](http://www.zilog.com/docs/ez80acclaim/ps0153.pdf)
as its CPU. It is a modern version of the venerable Z80 used in
[so many 8-bit computers of the 1980's](https://en.wikipedia.org/wiki/Category:Z80-based_home_computers)
(ZX Spectrum, MSX, Amstrad CPC, among others), along with a few home consoles
(Sega SG-1000, Sega Master System; the Gaemboy used a variant of the Z80).

The main improvements of the eZ80 over the Z80 are:
- A instruction pipeline, allowing the a similar clocked eZ80 to perfrom 2-4x more
  instructions per cycle.
- A 24-bit flat address, allowing the CPU to address up to 16MiB of memory without
  the need for banking.
- Full compatibility mode with Z80, with code running in a 64k segment and switching
  into eZ80 mode to perform banking.

When writting assembly programs for the Agon Light, you have to choose which mode
you want your program to run in. This is done using the `.assume ADL` directive,
with `0` being Z80 mode (limited to 64k segment, 8/16 bit registers) and `1` being
in eZ80 mode (24-bit address space, 8/24 bit registers).

You also need to specify where the program should be loaded in memory. MOS
specifies that user memory starts at addres 0x40000, so the first two lines of our
assembly programs defines the run mode and the memory loading address.

```assembly
    ; Tell the assembler we want eZ80 24-bit address mode.
    .assume ADL = 1

    ; User memory starts at $040000
    .org $40000
```

