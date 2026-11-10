---
title: "x86 어셈블리 기초 1: 명령어와 피연산자"
date: 2026-09-02 16:00:00 +0900
categories: [블로그/기술문서]
tags: [system-hacking, assembly, x86, dreamhack]
---

## 어셈블리어의 기본 구조

`opcode operand1, operand2`

예: `mov eax, 3` (eax에 3을 대입해라), `push rbp`

---

## 명령어 종류

- Data transfer: `mov`, `lea`
- Arithmetic (산술 연산): `inc`, `dec`, `add`, `sub`
- Logical: `and`, `or`, `xor`, `not`
- Comparison: `cmp`, `test`
- Branch: `jmp`, `je`, `jg`
- Stack: `push`, `pop`
- Procedure (프로시저): `call`, `ret`, `leave`
- System call: `syscall`

---

## 피연산자 (Operand)

- Immediate Value
- Register
- Memory: 메모리 피연산자는 `[]`로 감싸서 표현한다. 앞에 크기 지정자(`BYTE`, `WORD`, `DWORD`, `QWORD`)가 올 수 있다.

```nasm
QWORD PTR [0x8048000]   ; 0x8048000의 데이터를 8바이트만큼 참조
WORD PTR [rax]          ; rax가 가리키는 주소에서 데이터를 2바이트만큼 참조
```

피연산자 표기법 일부

| 피연산자 | 의미 |
|---|---|
| r8 | 8비트 범용 레지스터 (ah, al, dh, dl, ...) |
| r16 | 16비트 범용 레지스터 (ax, bx, cx, ...) |
| r32 | 32비트 범용 레지스터 (eax, ebx, ecx, ...) |
| r64 | 64비트 범용 레지스터 (rax, rbx, rcx, ...) |
| imm8 | 8비트 부호 있는 정수 |
| imm16 | 16비트 부호 있는 정수 |
| imm32 | 32비트 부호 있는 정수 |
| imm64 | 64비트 부호 있는 정수 |
| r/m8 | 8비트 범용 레지스터 혹은 메모리 |
| r/m16 | 16비트 범용 레지스터 혹은 메모리 |
| r/m32 | 32비트 범용 레지스터 혹은 메모리 |
| r/m64 | 64비트 범용 레지스터 혹은 메모리 |

---

## 산술·논리 연산

`add`: 덧셈

```nasm
mov rax, 5
mov rbx, 10
add rax, rbx   ; --> rax = 15
```

`sub`: 뺄셈

```nasm
mov rax, 50
sub rax, 10    ; --> rax = 40
```

`mul` (부호 없음) 곱셈: 암시적으로 RAX 레지스터를 첫 피연산자로 사용한다. 예를 들어 EAX와 ESI를 곱하면 32비트 x 32비트라서 결과가 64비트까지 나올 수 있다. 그래서 32비트 레지스터 하나를 더 써서 상위 32비트와 하위 32비트를 나눠 보관한다.

> **mul 연산 레지스터 분할**
>
> - **8비트 x 8비트**: AL 값과 피연산자를 곱한다. 결과는 최대 16비트라서 AX에 보관한다.
> - **16비트 x 16비트**: AX 값과 피연산자를 곱한다. 상위 16비트는 DX, 하위 16비트는 AX에 보관한다.
> - **32비트 x 32비트**: EAX 값과 피연산자를 곱한다. 상위 32비트는 EDX, 하위 32비트는 EAX에 보관한다.
> - **64비트 x 64비트**: RAX 값과 피연산자를 곱한다. 상위 64비트는 RDX, 하위 64비트는 RAX에 보관한다.
{: .prompt-tip }

`imul` (부호 있음) 곱셈: 피연산자가 1개면 mul과 비슷하게 동작한다. 2개면 피연산자1에 피연산자2를 곱한 결과를 피연산자1에 대입한다. 3개면 피연산자2와 피연산자3을 곱한 값을 피연산자1에 대입한다.

`div` (부호 없음), `idiv` (부호 있음) 나눗셈

```nasm
div ebx   ; EDX:EAX 안에 들어 있는 값을 ebx로 나눠라
div rbx   ; RDX:RAX 안에 들어 있는 값을 rbx로 나눠라
```

`and`, `or`, `xor`, `not`

`inc`: 레지스터/메모리에 저장된 값을 1 증가
`dec`: 레지스터/메모리에 저장된 값을 1 감소

---

## 데이터 이동

`mov`: 피연산자2의 값을 피연산자1에 대입
`lea`: 피연산자2의 주소값을 피연산자1에 그대로 대입

---

## 비교

`cmp`: 두 피연산자가 같으면 ZF가 1 (op2 - op1이 음수면 SF가 1, overflow 시 OF가 1)

`test`: 두 피연산자를 and 연산한 뒤 플래그 레지스터만 갱신한다. and 연산이라서 둘 중 하나라도 0이면 ZF가 1로 설정된다.

---

## 분기 (Branch)

프로그램의 실행 흐름을 바꾸는 명령어. 분기 명령어를 쓰면 명령어 포인터(instruction pointer)가 바뀐다. `jmp <label>`, `je <label>` 같은 문법으로 동작한다.

> **어셈블리어의 레이블**
>
> 특정 주소에 이름을 붙여서 쓸 수 있게 하는 기호. 코드가 커질 때 레이블로 코드를 나눌 수 있어서 분기문이나 함수 선언에 많이 쓴다.
{: .prompt-tip }

`jmp`: 특정 주소로 rip를 바꾸는 명령어

`je` (`jz`): 직전에 비교한 두 피연산자가 같으면 점프

```nasm
mov rax, 10
mov rbx, 10
cmp rax, rbx
je label   ; ZF가 1이기 때문에 label로 점프
```

`jg` (`jge`): 직전에 비교한 두 피연산자 중 앞쪽이 더 크면(크거나 같으면) 점프. jg는 (ZF == 0) && (SF == OF)일 때, jge는 SF == OF일 때 동작한다.

`jl` (`jle`): 직전에 비교한 두 피연산자 중 앞쪽이 더 작으면(작거나 같으면) 점프. jl은 SF != OF일 때, jle는 ZF == 1 \|\| (SF != OF)일 때 동작한다.

`jne` (`jnz`): 직전에 비교한 두 피연산자가 다르면 점프 (ZF == 0)

---

## 반복문

**카운터 기반 반복문** (for와 비슷)

```nasm
mov rcx, 10          ; 반복 횟수 설정
loop_start:
    ; (반복할 코드)
    loop loop_start  ; RCX를 감소시키며 0이 아닐 때까지 반복
```

**조건 기반 반복문** (while과 비슷)

```nasm
check_condition:
    cmp rax, 50          ; 특정 조건 확인
    jge loop_exit        ; 조건이 만족되면 종료
    ; (반복할 코드)
    jmp check_condition  ; 다시 비교 연산으로 이동
loop_exit:
```

**후조건 기반 반복문** (do while과 비슷)

```nasm
do_loop:
    ; (반복할 코드 실행)
    cmp rax, 10   ; 조건 검사
    jl do_loop    ; 조건이 만족되면 다시 실행
```
