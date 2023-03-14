# Lesson 01: The minimal working program.

For our first assembly program, we will create the equivalent of the following C code:

```c
int main(void) {
  return 0;
}
```

The corresponding assembly code would then be:

```assembly
    .assume ADL = 1
    .org $40000

    ; Jump over the header code
    JP main

    ; Define the standard MOS header
    .align 64
    .db "MOS"
    .db 00h
    .db 01h

main:
    LD hl, 0
    ret
```
