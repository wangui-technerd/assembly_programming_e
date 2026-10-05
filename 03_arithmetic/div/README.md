# DIV: flags after `div`

After `div`, all six flags (CF, OF, SF, ZF, AF, PF) are undefined, so whatever GDB shows is not a result of the division. The quotient and remainder show that the division worked.

## Program 1: `div1.asm` (8-bit)

`div bl`: AX = 100 ÷ BL = 7, so AL = 14 (quotient), AH = 2 (remainder). Check: 14 × 7 + 2 = 100.

| Flag | State | Why |
|---|---|---|
| CF, OF, SF, ZF, PF | Undefined (observed: cleared) | not defined after `div` |
| AF | Undefined (observed: set) | not defined after `div`; a hardware leftover, not a carry from the division |

## Program 2: `div3.asm` (32-bit)

`div ebx`: EDX:EAX = 300000000 ÷ EBX = 1000, so EAX = 300000 (quotient), EDX = 0 (remainder). Check: 300000 × 1000 + 0 = 300000000.

| Flag | State | Why |
|---|---|---|
| CF, OF, SF, ZF, PF | Undefined (observed: cleared) | not defined after `div` |
| AF | Undefined (observed: set) | not defined after `div`; a hardware leftover, not a carry from the division |