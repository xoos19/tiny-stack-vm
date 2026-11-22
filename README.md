# tiny-vm

A tiny stack virtual machine for make.fun's `tiny-stack-vm-01` bounty
(ISA version 1). One program, `main.ts` (Node 22), runs all 40 instances
of a set: each `/input/<id>/program.bin` on its `input.bin`, writing
`/output/<id>/stdout.bin` and `/output/<id>/exit.txt`.

The VM is a faithful interpreter of the ISA: 32-bit wrapping integers
(`Math.imul` for exact low-32 multiplication), a 256-value stack, a
64-deep call stack, 256 words of memory, a 1,000,000-step limit, a
65,536-byte output limit, and all ten traps with their own exit codes —
including the hard cases: `DIV`/`MOD` by zero and `-2147483648 / -1`,
unsigned `DIVU`/`MODU`/`LTU`, shifts masked to 5 bits, `SHR` vs `SHRU`,
`PUSH8` sign extension, `EXIT`'s low byte, and traps keeping the output
written before them.

Local check on the practice set:

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=../kit/practice/input MAKEFUN_OUTPUT_DIR=out node main.ts
```

or the kit's own grader:

```sh
node ../kit/cli/makefun.mjs grade-local --kit ../kit --bot . --driver process
```
