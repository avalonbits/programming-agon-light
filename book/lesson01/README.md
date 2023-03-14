# Lesson 01: The minimal working program.

For our first assembly program, we will create the equivalent of the following C code:

```c
int main(void) {
  return 0;
}
```

The corresponding assembly code would then be:

```assembly
    ; Tell the assembler we want ez80 24-bit address mode.
    .assume ADL = 1

    ; User memory starts at $040000
    .org $40000

    ; Jump over the header code
    JP main

    ; Define the standard MOS header, which must start at byte 64.
    .align 64

    .db "MOS"   ; This tells MOS it is load a MOS program.
    .db 00h     ; The version of the MOS header, currently 0.
    .db 01h     ; CPU run mode. 0 means z80 and 1 means ez80.

main:
    LD hl, 0
    ret
```
