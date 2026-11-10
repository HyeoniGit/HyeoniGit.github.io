---
title: "x86-64 아키텍처와 레지스터"
date: 2026-09-02 16:30:00 +0900
categories: [블로그/기술문서]
tags: [system-hacking, assembly, x86-64, register, dreamhack]
---

## n비트 아키텍처

**n**은 CPU가 한 번에 처리할 수 있는 데이터의 크기(WORD SIZE)를 뜻한다.

64-bit architecture means the CPU can process 64 bits of data at a time.

---

## 범용 레지스터 (General-Purpose Registers)

x86-64 architecture has 16 general-purpose registers, each 64 bits wide. 주로 데이터 연산에 쓰인다.

`rax`, `rbx`, `rcx`, `rdx`, `rsi`, `rdi`, `rbp`, `rsp`, `r8`, `r9`, `r10`, `r11`, `r12`, `r13`, `r14`, `r15`

---

## 세그먼트 레지스터 (Segment Registers)

x86-64 architecture has 6 segment registers, used to hold the base addresses of different segments in memory.

`cs`, `ds`, `es`, `fs`, `gs`, `ss`

---

## 명령어 포인터 레지스터 (Instruction Pointer Register)

x86-64 architecture has an instruction pointer register, which points to the next instruction to be executed.

`rip`

---

## 플래그 레지스터 (Flags Register)

x86-64 architecture has a flags register, which holds the status of the CPU after it executes an instruction.

`rflags`

> rflags는 64비트 크기지만, 실제로 쓰이는 건 20여 개의 비트뿐이다. (CF, ZF, SF, OF ...)
{: .prompt-tip }

---

## 레지스터 호환 (Register Compatibility)

x86-64 architecture is backward-compatible with IA-32 (x86, Intel 32-bit).

| Mode | Registers |
|---|---|
| 64-bit | `rax`, `rbx`, `rcx`, `rdx`, ... |
| 32-bit | `eax`, `ebx`, `ecx`, `edx`, ... |
| 16-bit | `ax`, `bx`, `cx`, `dx`, ... |

`ax`, `bx`, `cx`, `dx`는 각각 8비트 레지스터 두 개로 나눌 수 있다.

- **하위(low) 8비트**: `al`, `bl`, `cl`, `dl`
- **상위(high) 8비트**: `ah`, `bh`, `ch`, `dh`
