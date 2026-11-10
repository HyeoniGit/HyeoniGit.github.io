---
title: "GDB 명령어 정리"
date: 2026-09-03 13:57:00 +0900
categories: [블로그/기술문서]
tags: [system-hacking, gdb, pwndbg, debugging, dreamhack]
---

## Basic Usage

### `file`

Loads a binary file into GDB so it can be debugged.

### `run` (`r`)

Starts execution of the loaded binary.

### `break` / `continue` (`b` / `c`)

- `break` sets a breakpoint at a specific line or function.
- `continue` resumes execution after a breakpoint is hit.

### `entry` / `start`

- `entry` begins analysis from the binary's entry point.
- `start` runs the program and stops at the beginning of `main`. If no `main` function exists, it behaves like `entry`.

> **Entry point**
>
> The entry point is the address where the program begins execution. Find it with `readelf -h binary_file`. You can also see it in the DISASM pane after running `entry`.
{: .prompt-tip }

### `ni` / `si`

Both step to the next line of execution.

- `ni` (next instruction) steps **over** function calls.
- `si` (step instruction) steps **into** function calls.

### `finish`

Runs until the current function returns, then stops back in the caller. Useful for quickly exiting a function.

---

## Frequently Used Commands

### `info`

**`info registers` (`i r`)**
Displays the current values of the CPU registers.

> **Using register values**
>
> Prefix a register name with `$` to reference its value, e.g. `$eax` for the EAX register.
{: .prompt-tip }

**`info breakpoints` (`i b`)**
Lists all breakpoints currently set.

### `disassemble` (`disass`)

Displays the assembly code of the current function.

### `x` (examine memory)

Examines and displays memory contents at a given address, with a specified length and format.

```text
x/10gx $rsp
```

→ Displays 10 quadwords (giant words, 8 bytes each) in hex format, starting from the address in `$rsp`.

### `telescope`

Displays memory contents starting from a given address, recursively dereferencing pointers until a final value is reached.
(주소를 따라 값이 나올 때까지 계속 dereference하다가, 값이 나오면 출력)

### `vmmap`

Displays the process's virtual memory layout. If files are memory-mapped, shows their names and address ranges.

> **File mapping**
>
> A technique where a file's contents are mapped directly into the process's memory space.
{: .prompt-tip }

### `backtrace` (`bt`)

Displays the call stack.

> **Call stack**
>
> A data structure tracking the order of function calls during execution. Each call pushes a new frame; each return pops one off.
{: .prompt-tip }

### Dump memory

Saves the memory contents of a specified address range to a file.

Typical workflow to dump a program's code section:

```text
entry → vmmap → find the code section's address range → dump memory <file_name> <start_address> <end_address>
```

### `context` (`ctx`)

Displays the main runtime context, split into 4 panes:

1. **Registers**: current CPU register values
2. **DISASM**: assembly code starting from `rip`
3. **Stack**: stack memory contents starting from `rsp`
4. **BACKTRACE**: the current call stack

### `set`

Modifies the value of a register or memory address.

```text
set $rax = 0                          # RAX 값을 0으로 설정
set $rsp = $rbp                       # RSP 값을 RBP 값으로 설정
set *(unsigned long*)0x401000 = 0     # 0x401000부터 8바이트를 0으로 설정
```
