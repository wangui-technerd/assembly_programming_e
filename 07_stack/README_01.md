# 01.asm: 64-bit push and pop
This program pushes two numbers onto the 64-bit stack and pops them off in reverse order (last in, first out).

At startup, `RSP` points to the top of the process stack. The program loads `100` (`0x64`) into `RAX` and `200` (`0xC8`) into `RBX`.

Pushong:
   - `push rax` subtracts 8 from `RSP` (the x86_64 stack grows downward and each slot is 8 bytes) and writes `100` to `[RSP]`.
   - `push rbx` subtracts another 8, so `RSP` is now 16 bytes below its starting value, and writes `200` to `[RSP]`.
   - At the first `nop`, `200` is at `[RSP]` and `100` is at `[RSP + 8]`.

Popping:
   - `pop rcx` copies the 8 bytes at `[RSP]` (`200`) into `RCX`, then adds 8 to `RSP`.
   - `pop rdx` copies the next 8 bytes (`100`) into `RDX`, then adds 8 to `RSP`. `RSP` is back at its starting address.

The program loads `60` into `RAX`, clears `RDI` to `0`, and calls `syscall` to exit.

## Observations
- `./01` prints nothing because there is no `sys_write` call, and `echo $?` returns `0`.
- In GDB (`starti`, `layout regs`, `si`), `x/2gx $rsp` right after both pushes shows `0x00000000000000c8` (200) on top and `0x0000000000000064` (100) underneath.
- After both pops, `RCX` holds `200` and `RDX` holds `100`. The values came off in the reverse of the order they went on.