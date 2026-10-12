# 안녕하세요, 백엔드 개발자 이예린입니다 👋

> 취약한 서비스를 직접 만들고(ONDE), 직접 진단하고(ZeroDay Lab), 진단을 자동화(ARGUS)해 본
> **보안을 이해하는 Java/Spring 백엔드 개발자**입니다.
> SK쉴더스 루키즈 지능형 애플리케이션 개발 트랙 5기 (2026.02 ~ 2026.07) · 최종 프로젝트 **대상** 수상

- 🧰 Java · Spring Boot · Spring Security · JPA · MariaDB · Redis · Docker · Python

---

## 🛠 Projects

### ✈️ ONDE (온데) - 통합 여행 예약 플랫폼 백엔드
[![repo](https://img.shields.io/badge/GitHub-UR--VULN%2FOnde__Backend-181717?logo=github)](https://github.com/UR-VULN/Onde_Backend)
`Spring Boot 3` `Java 17` `Spring Security` `JWT` `OAuth2` `Redis` `MariaDB` `Flyway`

Spring Boot 멀티 모듈(core / api / admin) 구조의 여행 예약 플랫폼. 팀 프로젝트에서 **인증 · 인가 모듈을 담당**했습니다.

- **JWT 인증 모듈** 설계·구현: 회원가입 · 로그인 · 토큰 재발급 API, Access 30분 / Refresh 14일
- **Refresh Token을 Redis**(`@RedisHash` + TTL)에 저장해 만료 · 재발급 · 중복 로그인 관리
- **Google / Kakao OAuth2 소셜 로그인** 연동, 이메일 미제공 계정을 `GUEST` → 이메일 인증 → `USER`로 승격하는 **단계적 온보딩 플로우**
- 사용자 · 판매자 · 관리자 **5단계 권한**(`USER`, `SELLER`, `SUPER_ADMIN`, `USER_ADMIN`, `SELLER_ADMIN`) 기반 접근 제어, 기본 차단 + 공개 경로 명시 방식의 Security 설정
- 구현 후 **Burp Suite**로 직접 점검

### 🧪 ZeroDay Lab - 웹 취약점 학습 · 워게임 플랫폼
[![repo](https://img.shields.io/badge/GitHub-ZeroDayLab--capstone%2FVulnerability--Analysis-181717?logo=github)](https://github.com/ZeroDayLab-capstone/Vulnerability-Analysis)
`PHP` `Python (FastAPI)` `Docker` `Docker Compose`

취약점을 "개념 → 원인 → 공격 → 방어" 순서로 학습하고 격리된 컨테이너에서 바로 실습하는 워게임 플랫폼. **보안 학습 콘텐츠 · 가이드, 워게임 문제 기획 및 개발 담당**했습니다.

- **SQL Injection · Command Injection · CSRF 3종, 총 9개 실습 환경**을 직접 설계하고 Docker로 구축
- 1 문제 = 1 컨테이너(세트) 격리, FLAG를 환경 변수 / 읽기 전용 파일로 주입하는 구조에 맞춰 문제 제작
- 각 문제의 학습 가이드(개념 · 원인 · 공격 · 방어 코드 비교) 작성
- 문제 선택 시 컨테이너를 생성하고 TTL로 회수하는 플랫폼 API는 팀의 [back-end](https://github.com/ZeroDayLab-capstone/back-end/tree/feat/API%2C-SQLite) · [front-end](https://github.com/ZeroDayLab-capstone/front-end) 저장소 참고

### 🔎 ARGUS - 웹 취약점 자동 진단 · 증적 · 결과서 생성
[![repo](https://img.shields.io/badge/GitHub-UR--ARGUS%2FARGUS__Merge-181717?logo=github)](https://github.com/UR-ARGUS/ARGUS_Merge)
`Python` `FastAPI` `Docker` `Selenium`

SK쉴더스 WEB/API 개발 보안 가이드(2022) 항목을 자동 진단하고 증적 스크린샷과 결과서(HTML → PDF)를 만들어 주는 도구. **"SSRF / File Inclusion 진단 엔진"과 "요청 및 응답 값 내 주요정보 포함 여부" 항목의 증적 · 결과서 엔진을 담당**했습니다. (코드는 `dev` 브랜치)

**1-4 SSRF / File Inclusion 진단 엔진** · [코드](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/diagnosis/modules/1-4)
- OpenAPI(Swagger) · URL 목록 입력을 정규화하고, 파라미터 이름 · 스키마로 SSRF / LFI / RFI 의심 파라미터를 정적 탐색
- baseline 응답 비교, 반복 타이밍 측정, 저장 후 재조회로 판정하고 OWASP ZAP Active Scan 결과를 병합, 결과마다 `HIGH / MEDIUM / LOW` 신뢰도와 근거를 기록
- 다중 역할 계정 로그인 · 401 시 토큰 갱신 후 재시도, 역할별 결과 중복 제거, `unittest` 테스트 6종
- 응답 차이와 지연만으로는 정상 기능과 공격 성공을 구분하기 어렵고 외부 콜백(OOB) 검증을 기간 안에 갖추기 어려워, 검증 끝에 **팀 논의로 자동 진단에서 수동 진단 항목으로 전환**

**5-2 증적 · 결과서** · [증적](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/screenshot/modules/5-2) · [결과서](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/report/modules/5-2)
- 브라우저 화면과 요청/응답 값을 함께 담는 **증적 스크린샷 엔진**, 재진단 시 이전 스크린샷 자동 정리
- 진단 결과를 공통 형식의 HTML 결과서로 병합해 PDF 자동 렌더링

---

## 📄 더 보기
프로젝트 상세 내용과 역할은 포트폴리오에 정리되어 있습니다.
