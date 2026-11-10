---
title: "x86 어셈블리 기초 2: 함수 호출과 시스템 콜"
date: 2026-09-05 16:40:00 +0900
categories: [블로그/기술문서]
tags: [system-hacking, assembly, x86, syscall, dreamhack]
---

## 함수와 프로시저

### 스택

> **스택이란**
>
> LIFO 방식으로 동작하는 구조. 가장 나중에 넣은 데이터가 가장 먼저 꺼내진다.
{: .prompt-tip }

높은 메모리 주소에서 낮은 메모리 주소로 자란다. 즉 **데이터를 추가할 때마다 메모리 주소가 감소**하고, **데이터를 제거하면 다시 증가**한다.

rsp(Stack pointer) 레지스터는 스택의 최상단을 가리킨다. push를 실행하면 rsp가 감소하면서 스택에 값이 저장되고, pop을 실행하면 rsp가 증가하면서 스택에 저장된 값을 꺼낸다.

`push`: 스택에 값을 저장하는 명령어

```nasm
push val    ; rsp -= 8, [rsp] = val
```

`pop`: 스택에서 값을 꺼내는 명령어

```nasm
pop reg     ; reg = [rsp], rsp += 8
```

---

### 함수 호출과 반환

`call`: 함수 호출 명령어. 실행하면 call 다음 명령어의 주소를 스택에 저장하고, rip를 함수로 옮긴다.

```nasm
call addr   ; addr에 위치한 프로시저 호출
            ; = push return_address
            ;   jmp addr
```

`leave`: 프로시저가 반환되기 전에 스택 프레임을 정리하는 명령어

```nasm
leave       ; = mov rsp, rbp
            ;   pop rbp
```

> **스택 프레임이란**
>
> 서로 다른 함수가 사용하는 스택 영역을 명확히 구분하기 위한 단위
{: .prompt-tip }

`ret`: rip 레지스터 값을 return address로 바꾼다.

```nasm
ret         ; = pop rip
```

### 함수 선언

1. 레이블 정의 (예: `add:`)
2. 스택 프레임이 필요하면 함수 프롤로그로 스택 프레임 구성 (예: `push ebp`)
3. 함수 본문 작성 (예: `mov eax, [ebp+8]`)
4. 함수 에필로그로 스택 프레임 해제, ret으로 종료 (예: `leave`, `ret`)

**x86**에서는 아래처럼 cdecl 함수 호출 규약으로 함수를 선언한다.

```nasm
add:
    push ebp
    mov ebp, esp
    mov eax, [ebp+8]    ; 스택에서 읽음
    add eax, [ebp+12]   ; 스택에서 읽음
    leave
    ret
```

**x64**에서는 아래처럼 SYSV 함수 호출 규약으로 함수를 선언한다.

```nasm
add1:
    push rbp
    mov rbp, rsp
    mov rax, rdi        ; 레지스터에서 바로
    add rax, rsi        ; 레지스터에서 바로
    pop rbp
    ret
```

### 함수 호출 과정

**x86**에서는 인자를 스택에 push한 뒤 call로 함수를 호출한다. 함수가 끝난 뒤에는 호출자 쪽에서 스택을 정리해야 한다.

```nasm
_start:
    push dword 20
    push dword 10
    call add
    add esp, 8
    mov ebx, eax
    mov eax, 1
    int 0x80
```

**x64**에서는 대부분의 정수 인자가 레지스터로 전달된다. 인자가 2개라면 첫 번째 인자는 rdi, 두 번째 인자는 rsi에 들어간다.

```nasm
_start:
    mov rdi, 10
    mov rsi, 20
    call add1
    mov rdi, rax
    mov rax, 60
    syscall
```

---

## 시스템 콜

### 사용자 모드와 커널 모드

현대 운영체제는 크게 **사용자 모드**와 **커널 모드**로 나뉘어 동작한다.

**커널 모드**는 운영체제가 전체 시스템을 제어하기 위해 시스템 소프트웨어에 부여하는 권한이다. 모든 메모리 영역에 접근할 수 있고 하드웨어에 직접 접근할 수 있다.

**사용자 모드**는 운영체제가 사용자에게 부여하는 권한이다. 접근할 수 있는 메모리 영역과 권한이 매우 한정되어 있고, 하드웨어에 직접 접근할 수 없다.

**시스템 콜**은 사용자 모드에서 커널 모드의 시스템 소프트웨어에게 어떤 동작을 요청할 때 쓴다. 사용자 모드에서는 파일 시스템에 직접 접근할 수 없으므로 커널이 도와줘야 하는데, 이 도움을 요청하는 것이 시스템 콜이다.
예: `read()`, `write()` 같은 함수는 내부적으로 read, write 시스템 콜을 호출해 커널에게 요청한다.

### 시스템 콜 사용법

**x86**에서는 `int 0x80` 명령어로 시스템 콜을 호출한다. eax에 시스템 콜 번호를 넣고, ebx → ecx → edx → esi → edi → ebp 순서로 인자를 넣는다.

```nasm
section .data
    filename db "hahah.txt", 0
    buffer times 100 db 0

section .text
    global _start

_start:
    mov eax, 5          ; sys_open
    mov ebx, filename   ; filename
    mov ecx, 0          ; read only
    int 0x80            ; call kernel
```

**x64**에서는 `syscall` 명령어로 시스템 콜을 호출한다. rax에 시스템 콜 번호를 넣고, rdi → rsi → rdx → r10 → r8 → r9 순서로 인자를 넣는다.

```nasm
section .data
    filename db "hahah.txt", 0
    buffer times 100 db 0

section .text
    global _start

_start:
    mov rax, 2            ; sys_open
    lea rdi, [filename]   ; filename
    mov rsi, 0            ; read only
    xor rdx, rdx          ; mode = 0
    syscall               ; call kernel
```

`lea rdi, [filename]`처럼 `[]` 안에 넣는 이유: x64에서 filename의 절대 주소를 그대로 받으면 값이 매우 커질 수 있어서, lea로 상대 주소(참조)를 받는다.

다른 예제

```text
[Register]
rax = 0x1
rdi = 0x1
rsi = 0x401000
rdx = 0xb       // 11

[Memory]
0x401000 | "Hello Wo"
0x401008 | "rld"

[Code]
syscall
```
