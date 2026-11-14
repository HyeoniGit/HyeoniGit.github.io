---
title: "Space Alone Chapter N 풀이"
date: 2026-11-14 18:00:00 +0900
categories: [CTF/Wargame]
tags: [space-alone, system-hacking, pwnable]
---

<!-- 복사해서 _posts/날짜-space-alone-chN.md 로 저장. N과 날짜를 바꾸고, 날짜는 지금보다 과거로 -->

[Space Alone 풀이 모음](/posts/space-alone/)

## 문제

<!-- story 요약 1~2줄, 주어진 파일(ls 결과) -->

```bash
ls -l
```

## 보호 기법

```bash
checksec ./파일이름
```

<!-- 결과 붙여넣기 -->

## 소스 코드 분석

```c
// 소스 코드 중 핵심 부분만
```

<!-- 취약한 부분과 이유 -->

## 디버깅

<!-- GDB로 확인한 것: 버퍼 위치, 리턴 주소까지 거리 등 -->

## 익스플로잇

```python
from pwn import *

p = process("./파일이름")
# p = remote(...)

p.interactive()
```

## 막혔던 부분

<!-- 어디서 막혔고 어떻게 풀었는지 -->

## 배운 점

-
