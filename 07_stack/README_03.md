# 03.asm

This program shows how `call` and `ret` use the stack to remember where execution resumes after a subroutine finishes.

- `_start` loads `10` (`0x0A`) into `RAX`. `call calculate` subtracts 8 from `RSP`, writes the address of `after_call` to `[RSP]` and jumps to `calculate`.

- In `calculate`, `add rax, 5` changes `RAX` from `10` to `15` (`0x0F`). `RSP` stays the same because nothing is pushed or popped. `ret` reads the 8-byte return address at `[RSP]` into `RIP` and adds 8 to `RSP`.

- Execution continues at `after_call` with `RAX` holding `15`, then the program exits with status `0`.

``

## Observations
- `./03` produces no screen output and exits with code `0`.
- In GDB, `call calculate` lowers `RSP` by 8 bytes. Inside `calculate`, `x/1gx $rsp` prints the address of `after_call`.
- When `ret` runs, `RSP` rises by 8 bytes and `RIP` jumps to `after_call`.