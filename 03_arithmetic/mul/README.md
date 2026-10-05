# MUL: flags after `mul`

After `mul`, only CF and OF are defined: both are set if the upper half of the product (AH for 8-bit, EDX for 32-bit) is non-zero, and cleared otherwise. SF, ZF, AF and PF are undefined, so the values GDB shows for them are not a result of the multiplication.

## Program 1: `mul1.asm` (8-bit)

`mul byte [num2]`: AL = 25 × 10 = AX 0x00FA (250), AH = 0

| Flag | State | Why |
|---|---|---|
| CF | Cleared | AH = 0, so 250 fits in 8 bits |
| OF | Cleared | same condition as CF |
| SF, ZF, AF, PF | Undefined (observed: cleared) | not defined after `mul` |

## Program 2: `mul3.asm` (32-bit)

`mul dword [num2]`: EAX = 100000 × 300000 = 30,000,000,000 = 0x6FC23AC00, so EDX = 0x6, EAX = 0xFC23AC00

| Flag | State | Why |
|---|---|---|
| CF | Set | EDX = 6 is non-zero, so the product does not fit in 32 bits |
| OF | Set | same condition as CF |
| SF, ZF, AF, PF | Undefined (observed: cleared) | not defined after `mul` |