# 06.asm
This program runs the same save-and-restore test as `05.asm` in 32-bit mode (`BITS 32`), with 4-byte (`dword`) values and `ESP`.

- `_start` loads `0x11223344` into `EAX` and `0x1234` into `EBX`.

- `push eax` subtracts 4 from `ESP` and writes `0x11223344` to `[ESP]`.
- `push ebx` subtracts 4 more and writes `0x00001234` to `[ESP]`.

- `pop ebx` reads `0x00001234` from `[ESP]` into `EBX` and adds 4 to `ESP`.
- `pop eax` reads `0x11223344` from `[ESP]` into `EAX` and adds 4 to `ESP`.

- The program sets `EAX = 1` and `EBX = 0`, then `int 0x80` exits.

## Observations
- `./06` runs silently and returns exit status `0`.
- In GDB, `x/2wx $esp` at the `nop` shows `0x00001234` at `[ESP]` and `0x11223344` at `[ESP + 4]`.
- Compared with `05.asm`, the only difference is slot width: `ESP` moves 4 bytes per push or pop here, and `RSP` moves 8 in 64-bit mode.