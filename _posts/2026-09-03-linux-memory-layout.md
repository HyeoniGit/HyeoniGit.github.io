---
title: "리눅스 프로세스 메모리 구조"
date: 2026-09-03 13:55:00 +0900
categories: [블로그/기술문서]
tags: [system-hacking, linux, memory, dreamhack]
---

## 메모리 구조

In Linux, a process's memory layout is divided into 5 segments:

*Code segment, Data segment, BSS segment, Heap segment, Stack segment*

---

## 코드 세그먼트 (Code Segment)

The code segment (also called the **text segment**) is where the executable machine code is located.

Since this area is executed as instructions, it is granted **r** (read) and **x** (execute) permissions.

---

## 데이터 세그먼트 (Data Segment)

The data segment is where **initialized global and static variables** are stored.

This area is split into two parts:

- **data segment**: writable
- **rodata segment** (read-only data): not writable

```c
char *str_ptr = "readonly";
// str_ptr (the pointer itself) is stored in the data segment,
// but the string "readonly" is stored in the rodata segment.
```

---

## BSS 세그먼트 (BSS Segment)

The BSS segment is where **uninitialized global and static variables** are stored.

When the program is loaded, the OS zero-initializes this area. It is granted **r** and **w** permissions.

---

## 스택 세그먼트 (Stack Segment)

The stack segment stores local variables and function call information. It is granted **r** and **w** permissions.

### Stack frame

The stack frame is the basic unit of the stack segment. It stores:

- **parameters**
- **return address**
- **local variables**

A stack frame is created when a function is called, and destroyed when the function returns.

It's generally impossible to predict how much stack space a program will need in advance, so the OS reserves a small initial amount and expands it as needed once the stack fills up.

The stack is often described as **growing downward**: it starts at a high address and expands toward lower addresses.

### Managing the stack segment

Stack frame management happens in two phases:

- **Function prologue**: sets up (creates) the stack frame
- **Function epilogue**: tears down (destroys) the stack frame

---

## 힙 세그먼트 (Heap Segment)

The heap segment is where dynamically allocated memory is stored.

- Allocated via system calls such as `brk()` or `mmap()`.
- This area **grows upward**.

If the heap and stack grew in the same direction, the heap could eventually collide with the stack once its allocated space is exhausted. To avoid this, the heap and stack are placed to grow in **opposite directions**.
