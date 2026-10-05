# SUB: flags after `sub`

For `sub`, CF means a borrow: the first operand is smaller than the second as unsigned numbers.

## Program 1: `sub1.asm` (8-bit)

`sub al, [num2]`: AL = 50 (0x32) - 80 (0x50) = 0xE2 (226 unsigned, -30 signed)

| Flag | State | Why |
|---|---|---|
| CF | Set | 50 < 80 unsigned, so a borrow was needed |
| ZF | Cleared | 0xE2 is not zero |
| SF | Set | bit 7 of `11100010` is 1 |
| OF | Cleared | -30 fits in signed 8-bit (-128 to 127) |
| PF | Set | low byte `11100010` has four 1 bits (even) |
| AF | Cleared | low nibbles 2 - 0, no borrow from bit 4 |

## Program 2: `sub2.asm` (16-bit)

`sub ax, [num2]`: AX = 1000 (0x03E8) - 2000 (0x07D0) = 0xFC18 (64536 unsigned, -1000 signed)

| Flag | State | Why |
|---|---|---|
| CF | Set | 1000 < 2000 unsigned, so a borrow occurred |
| ZF | Cleared | 0xFC18 is not zero |
| SF | Set | bit 15 of `1111 1100 0001 1000` is 1 |
| OF | Cleared | -1000 fits in signed 16-bit (-32768 to 32767) |
| PF | Set | low byte `00011000` has two 1 bits (even) |
| AF | Cleared | low nibbles 8 - 0, no borrow from bit 4 |