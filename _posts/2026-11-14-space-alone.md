---
title: "Space Alone 풀이 모음"
date: 2026-11-14 18:00:00 +0900
categories: [CTF/Wargame]
tags: [space-alone, system-hacking, pwnable, hspace]
pin: true
---

HSPACE의 [Space Alone](https://github.com/hspace-io/Space_Alone) 풀이 기록.
LOB(The Lord of BOF)를 Ubuntu 22.04 환경에 맞게 다시 만든 시스템 해킹 입문 문제집으로, 10개 챕터로 되어 있다.

## 진행 상황

| 챕터 | 다룬 기법 | 상태 | 풀이 |
|:---:|---|:---:|---|
| 1 | | BOF (인접 변수 덮어쓰기) |완료| [풀이](/posts/space-alone-ch1/) |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |
| 7 | | | |
| 8 | | | |
| 9 | | | |
| 10 | | | |

> 각 챕터의 기법은 풀고 난 뒤에 적는다.
{: .prompt-info }

### 자주 쓰는 명령어

| 명령어 | 설명 |
|---|---|
| `cmd` | 사용 가능한 커스텀 명령어 목록 |
| `story` | 현재 스토리 출력 |
| `chap` | 현재 챕터 확인 |
| `status` | 진행 상황, 비밀번호 출력 |
| `next` | 다음 챕터로 이동 |

### 규칙

- 주어진 `chall` 계정 안에서만 푼다. root로 푸는 건 금지.
- 다음 단계로 넘어가는 바이너리는 setuid가 걸려 있어 바로 디버깅할 수 없다. 복사본을 만들어서 디버깅한다.
