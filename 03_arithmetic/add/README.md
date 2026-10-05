# ADD: flags after `add`

## Program 1: `add1.asm` (8-bit)

`add al, [num2]`: AL = 120 (0x78) + 10 (0x0A) = 0x82 (130 unsigned, -126 signed)

| Flag | State | Why |
|---|---|---|
| CF | Cleared | 130 fits in 8 bits unsigned (max 255), so no carry out of bit 7 |
| ZF | Cleared | 0x82 is not zero |
| SF | Set | bit 7 of `10000010` is 1 |
| OF | Set | two positives (120 + 10) gave a negative; 130 exceeds the signed 8-bit max of 127 |
| PF | Set | low byte `10000010` has two 1 bits (even) |
| AF | Set | low nibbles 8 + 0xA = 0x12, which carries into bit 4 |

## Program 2: `add2.asm` (16-bit)

`add ax, [num2]`: AX = 32000 (0x7D00) + 500 (0x01F4) = 0x7EF4 (32500)

| Flag | State | Why |
|---|---|---|
| CF | Cleared | 32500 fits in 16 bits unsigned (max 65535) |
| ZF | Cleared | 0x7EF4 is not zero |
| SF | Cleared | bit 15 of `0111 1110 1111 0100` is 0 |
| OF | Cleared | positive + positive = positive, within the signed 16-bit max of 32767 |
| PF | Cleared | low byte `11110100` has five 1 bits (odd) |
| AF | Cleared | low nibbles 0 + 4 = 4, no carry into bit 4 |