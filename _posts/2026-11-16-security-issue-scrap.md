---
title: "보안 이슈 스크랩"
date: 2026-11-16 09:00:00 +0900
categories: [블로그/기술문서]
tags: [security-news, scrap, knights-frontier]
pin: true
---

매일 X(트위터)에 올라오는 최신 보안 이슈를 모아 두는 기록.
Knights Frontier 3기 활동 기간 동안 꾸준히 채운다. 각 항목은 링크 + 한 줄 요약.

> 월별로 나눠서 쌓는다. 아래로 갈수록 최신.
{: .prompt-info }

## 2026-11

| 날짜 | 주제 | 요약 | 링크 |
|:---:|---|---|:---:|
| 11.16 | 메타 벌금 (이탈리아) | 페이스북이 안드로이드 사용자 사진 갤러리에 명시적 동의 없이 접근(기본 활성화, 약 2천만 명 영향)한 혐의로 이탈리아가 8억 500만 유로 벌금 부과, 60일 내 시정 명령 | [🔗](https://x.com/TheHackersNews/status/2121928494527685015?a=-nNhxU1_Ccll5WOOnVo&s=32) |
| 11.17 | Optimizely 스크립트 하이재킹 (OPTIMJACK) | A/B테스트 도구 Optimizely 취약점을 악용해 CNN·BBC·Zoom 등 178개 조직 사이트에 악성 스크립트 주입 — 카드 정보·암호화폐 지갑 주소 탈취, iOS 익스플로잇 킷 로드 (공급망 공격) | [🔗](https://thehackernews.com/2026/11/hijacked-optimizely-scripts-served-card.html) |
| 11.19 | PHP 보안 업데이트 (치명적 취약점) | PHP 보안팀이 모든 지원 브랜치의 취약점을 수정하는 업데이트를 11/19 배포 예정, 최고 등급 critical. 세션 업로드 진행률 기능의 RCE(CVE-2026-115560) 포함이나 비기본 설정에서만 악용됨. 버전 점검·패치 촉구 | [🔗](https://cybersecuritynews.com/php-critical-vulnerabilities/) |
| 11.20 | phpAfterShell (PHP RCE 상세) | PHP 세션 파일 조작을 원격 코드 실행으로 바꾸는 취약점(CVSS 9.2) 상세 공개. 10월 PHP2Shell 패치가 근본 원인을 못 고쳐 재등장, 기본 파일 기반 세션 핸들러에 영향. 8.2.36/8.3.37/8.4.28/8.5.13/8.6.0으로 업데이트 필요, 실제 악용 사례는 아직 없음 | [🔗](https://x.com/TheHackersNews/status/2123463167741456864) |
| 11.21 | Meta Muse AI 에이전트 장악 (연구 시연) | 악성 웹페이지 한 번으로 Meta의 AI 에이전트 Muse를 공격 — 프롬프트 인젝션으로 대화·메모리 탈취·변조 후, 내장 브라우저+리눅스 커널 결함을 엮어 클라우드 머신 전체 장악. 실사용자 공격 아닌 연구실 시연, 공개 전 패치됨. AI 에이전트가 외부 웹을 읽고 행동할 때의 위험 사례 | [🔗](https://thehackernews.com/2026/11/a-single-malicious-webpage-let.html) |
| 11.22 | KEPCO(한국전력) 대규모 개인정보 유출 | 해커가 KEPCO의 두 시스템에 무단 접근해 고객 서비스 이용자 650,487명 + 조달 시스템 이용자 869,761명(총 약 152만 명) 정보 유출. 이름·주민등록번호·은행 계좌·신용카드·전기요금 청구 기록·직원 연락처 포함. 11/20 발견 후 외부 접근 차단, 국정원 등과 조사 중. 지난 한 달 새 두 번째 사고(이전엔 직원·파트너 35,528명 영향) | [🔗](https://x.com/DarkWebInformer) |
| 11.23 | N-able Mail Assure 치명적 취약점 (CVE-2026-134624) | MSP용 메일 필터링 서비스 Mail Assure/SpamExperts의 SSRF 취약점(CVSS 9.3). 침해 사고 조사 중 발견 — 고권한 API 사용자가 서버로 하여금 내부 호스트·포트에 접속하게 만들 수 있음(코드 실행은 아님). LDAP 동기화의 연결 확인 기능에 목적지 제한이 없던 게 원인. 빌드 20261121.1943에서 패치(호스티드는 자동 적용), 실제 악용은 침해된 서버 1대 외 정황 없음 | [🔗](https://thehackernews.com/2026/11/n-able-discloses-critical-mail-assure.html) |
| 11.24 | VMware vCenter 인증 우회 (CVE-2026-123890) | vCenter의 Directory Service에 인증 우회 취약점(CVSS 9.8). 네트워크 접근만으로 인증을 우회해 시스템 장악 가능. 패치보다 2개월 이상 앞서 **제로데이로 악용**됨 — 중국 연계 첩보 조직(Armory Castle)이 9월 초 이미 침투한 망 안에서 vCenter 관리자 권한 획득(Mandiant 보고). Broadcom이 전 지원 브랜치 패치, 우회책 없음 | [🔗](https://thehackernews.com/2026/11/critical-vmware-vcenter-flaw-cvss-98.html) |
| 11.25 | Drupal 설치 과정 RCE (CVE-2026-132165) | Drupal 코어 설치 기능 결함(CVSS 9.8). 비인증 공격자가 설치 과정("안전하게 실패하지 못함")을 악용해 사이트 장악·코드 실행 가능. 패치 공개됐으나 공개 직후 **실제 악용 확인**돼 CISA KEV 등록 | [🔗](https://thehackernews.com/2026/11/drupal-patches-critical-installer-flaw.html) |
| 11.26 | Hex.pm 레지스트리 RCE (CVE-2026-129514, CVSS 10.0) | Elixir/Erlang 패키지 레지스트리 Hex.pm **서버 코드**의 역직렬화 RCE(2016년부터 존재). 공격자가 서비스 계정 권한으로 OS 명령 실행 → 운영 서버 6대에 익스플로잇 흔적, **실제 악용 확정**(UTC 18:16~18:29 익스플로잇 요청 69건). 약 15분 만에 패치, 전 사용자 강제 로그아웃·토큰 교체. 단 **패키지 변조 흔적은 없음**, 데이터(이메일·비번 해시·2FA 시크릿·비공개 패키지) 유출 여부는 배제 못 함(확인 필요) | [🔗](https://thehackernews.com/2026/11/hackers-exploited-cvss-100-hexpm-rce.html) |
| 11.27 | 스위스, AI 기반 공격의 핵심 인프라 침해 확인 | 스위스 NCSC가 AI 에이전트를 활용한 사이버공격 5건을 확인, 그중 4건에서 실제 침해 발생. 핵심 인프라 운영사 3곳을 겨냥해 2곳이 뚫렸으나 큰 서비스 중단 보고는 없음. 기술적 세부는 공개되지 않았고, 이전 공식 발표와 공격 건수가 달라진 점은 설명 없음(확인 필요). AI 에이전트가 공격에 실전 투입된 사례로 주목 | [🔗](https://thehackernews.com/2026/11/switzerland-confirms-ai-assisted.html) |
| 11.28 | 1Password 침해 (GitHub 계정 경유) | 직원 **GitHub 계정** 탈취로 시작 — 공격자가 소스 코드에 접근하고 백엔드 서비스에 악성 코드 푸시, 일부 고객 **계정 메타데이터**(이름·이메일·계정ID·공개키·SRP 파라미터·초대/복구 토큰)와 비공개 레포 소스 코드 유출. 과장된 "1Password Hacked" 제목과 달리 **볼트 내용(비밀번호·시크릿·복구키)은 유출 안 됨**(1Password 주장) — 볼트는 사용자만 가진 키로 암호화, 유출된 토큰도 SSO나 이메일 코드 없이는 단독 사용 불가. 영향 계정은 소수·개별 통지. 대응: GitHub 계정 차단·토큰 폐기·자격증명 교체, 초대/복구 코드 6자리→10자로 연장 | [🔗](https://thehackernews.com/2026/11/1password-hacked-customer-data-and.html) |
