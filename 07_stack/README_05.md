# 05.asm
This program pushes two 64-bit values onto the stack and pops them back in reverse order so both registers end with their original values.

- `_start` puts `0x1122334455667788` into `RAX` and `0x1234` into `RBX`.

- `push rax` subtracts 8 from `RSP` and writes `0x1122334455667788` to `[RSP]`.
- `push rbx` subtracts 8 more and writes `0x0000000000001234` to `[RSP]`.
- `pop rbx` reads the top 8 bytes (`0x1234`) into `RBX` and adds 8 to `RSP`.
- `pop rax` reads the next 8 bytes (`0x1122334455667788`) into `RAX` and adds 8 to `RSP`.
- The program calls `sys_exit(0)` through `syscall`.


## Observations
- `./05` prints nothing and returns exit code `0`.
- In GDB, `x/2gx $rsp` at the `nop` shows `0x0000000000001234` at `[RSP]` and `0x1122334455667788` at `[RSP + 8]`.
- `RBX` was pushed last, so it has to be popped first. Popping in that order puts each value back in its own register.