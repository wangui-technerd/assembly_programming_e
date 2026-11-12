# 02.asm

This program builds a stack frame inside `work`, stores two local variables in it, reads them back into registers, and tears the frame down before returning.

`call work` pushes the 8-byte return address (the address of `mov rax, 60`) onto the stack, lowering `RSP` by 8, and jumps to `work`.


   - `push rbp` saves the caller's base pointer (`RSP` drops by 8).
   - `mov rbp, rsp` sets `RBP` to the current stack pointer, giving the local variables a fixed reference point.
   - `sub rsp, 16` moves `RSP` down 16 bytes, reserving room for two 8-byte (`qword`) locals.


   - `mov qword [rbp - 8], 100` writes `100` (`0x64`) into the first slot.
   - `mov qword [rbp - 16], 200` writes `200` (`0xC8`) into the second slot.
   - `mov rax, [rbp - 8]` and `mov rbx, [rbp - 16]` load both values back into `RAX` and `RBX`.


   - `mov rsp, rbp` moves `RSP` back up 16 bytes, releasing the local variable space.
   - `pop rbp` restores the caller's `RBP` (`RSP` rises by 8).
   - `ret` pops the return address into `RIP` (`RSP` rises by 8) and returns to `_start`, which exits with status `0`.


## Observations
- The binary exits silently with status `0`.
- In GDB, `x/2gx $rbp-16` after the `mov` instructions shows `0x00000000000000c8` (200) at `[rbp - 16]` and `0x0000000000000064` (100) at `[rbp - 8]`.
- `mov rsp, rbp` has to run before `pop rbp`. Without it, `pop rbp` would load a local variable into `RBP` and `ret` would jump to a local value as if it were the return address.