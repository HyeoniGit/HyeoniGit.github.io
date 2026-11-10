---
title: "함수 호출 규약: cdecl, stdcall, fastcall, SysV"
date: 2026-09-03 13:50:00 +0900
categories: [블로그/기술문서]
tags: [system-hacking, assembly, calling-convention, dreamhack]
---

**함수 호출 규약**이란 함수의 호출과 반환에 대한 약속이다.

한 함수가 다른 함수를 호출하고, 호출된 함수가 반환하면 다시 원래 함수로 돌아와서 기존 실행 흐름을 이어 간다. 함수를 호출할 때는 반환된 이후를 위해 **호출자**의 상태와 반환 주소를 저장해야 한다. 또 **피호출자**가 요구하는 인자를 전달해 줘야 하고, 피호출자의 실행이 끝나면 반환 값을 전달받아야 한다.

## 호출 규약 종류

- **x86**: cdecl, stdcall, fastcall, thiscall ...
- **x86-64**: System V AMD64 ABI, MS ABI

---

## x86 호출 규약

### cdecl

- 리눅스의 gcc가 바이너리를 컴파일할 때 일반적으로 쓰는 호출 규약
- 스택만으로 인자 전달
- 스택을 호출자가 정리
- 마지막 인자부터 첫 번째 인자 순으로 거꾸로 스택에 push

### stdcall

- 윈도우 API에서 기본으로 쓰는 호출 규약
- cdecl의 변형
- 스택 정리를 호출자가 아닌 **피호출자**가 수행

### fastcall

- 인자를 레지스터로 전달해서 속도를 높임
- 처음 두 인자는 각각 ecx, edx로 전달하고, 나머지 인자는 마지막부터 스택으로 전달

### thiscall

- C++ 클래스 멤버 함수를 위한 호출 규약
- this 포인터를 ecx 레지스터로 전달 (`[ecx + offset]`으로 접근 가능)

---

## x86-64 호출 규약

### SYSV ABI (System V Application Binary Interface)

- 인자 6개를 rdi, rsi, rdx, rcx, r8, r9에 순서대로 저장해서 전달
- 인자가 더 필요하면 스택 사용
- 인자 전달에 쓰인 스택은 **호출자**가 정리
- 반환 값은 rax로 전달

<details>
<summary>함수 호출 시 일어나는 일 (내가 이해한 대로 정리)</summary>

<p>rbp(base pointer register)에 rsp(stack pointer register)를 대입한다. return할 때 돌아가기 위한 것이고, 함수 내부에서 rbp는 바꾸지 않는다.</p>
<p>이후 함수를 실행하고, 반환 값을 rax에 저장한다.</p>
<p>마지막으로 rsp에 rbp를 대입하고 반환한다 (leave, ret).</p>
</details>

---

## 개인적으로 생각해 본 것

피호출자가 스택을 정리하는 게 훨씬 간결하고 직관적으로 보이는데, 왜 cdecl에서는 호출자가 스택을 정리할까?

→ printf처럼 인자 개수가 달라질 수 있는 함수는 피호출자가 스택을 얼마나 정리해야 할지 알기 어렵다. 그래서 인자를 넣은 호출자가 정리한다.
