# 04.asm

This program is assembled with `BITS 32`, so it uses 32-bit registers, 4-byte stack slots and the `int 0x80` Linux interrupt to exit.

- `call work` subtracts 4 from `ESP`, pushes the 4-byte return address, and jumps to `work`.

- `push ebp` saves the old `EBP` (`ESP` drops by 4).
- `mov ebp, esp` anchors `EBP` at the current top of the stack.
- `sub esp, 8` makes room for two 4-byte (`dword`) locals.

- `mov dword [ebp - 4], 100` stores `100` (`0x64`) in the first slot.
- `mov dword [ebp - 8], 200` stores `200` (`0xC8`) in the second slot.
- `mov eax, [ebp - 4]` and `mov ebx, [ebp - 8]` load both values into `EAX` and `EBX`.

- `mov esp, ebp` releases the 8 bytes of locals (`ESP` rises by 8).
- `pop ebp` restores `EBP` (`ESP` rises by 4).
- `ret` pops the 4-byte return address into `EIP` (`ESP` rises by 4).
- `_start` sets `EAX = 1` and `EBX = 0`, then `int 0x80` exits.


## Observations
- As a 32-bit binary, it needs `-f elf32` to assemble and `-m elf_i386` to link.
- In GDB, `x/2wx $ebp-8` shows `0x000000c8` (200) at `[ebp - 8]` and `0x00000064` (100) at `[ebp - 4]`.
- Every push, pop, and variable offset moves in 4-byte steps, whereas the 64-bit programs move in 8.