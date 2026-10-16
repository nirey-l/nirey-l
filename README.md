# 안녕하세요, 백엔드 개발자 이예린입니다 👋

> **데이터의 접근 권한과 정합성**을 이해하는 **Java/Spring 백엔드 개발자**입니다.
> 맡은 기능은 **API부터 화면까지 직접 연결**하고, AI와 함께 만든 결과는 **기준을 세워 검증**합니다. 직접 만든 서비스를 보안 관점으로 진단해 본 경험도 있습니다.
> SK쉴더스 루키즈 지능형 애플리케이션 개발 트랙 5기 (2026.02 ~ 2026.07) · 최종 프로젝트 **대상** 수상

- 🧰 **Backend** Java · Spring Boot · Spring Security · JPA · MySQL · MariaDB · Redis · Flyway
- 🖥 **Frontend** React · TypeScript
- 🛠 **Test · Tools** JUnit 5 · Mockito · Postman · Docker · Git · Python(FastAPI)

---

## 🛠 Projects

### ✈️ ONDE (온데) - 통합 여행 예약 플랫폼 백엔드
[![repo](https://img.shields.io/badge/GitHub-UR--VULN%2FOnde__Backend-181717?logo=github)](https://github.com/UR-VULN/Onde_Backend)
`Spring Boot 3` `Java 17` `Spring Security` `JWT` `OAuth2` `Redis` `MariaDB` `Flyway` `React` `TypeScript`

Spring Boot 멀티 모듈(core / api / admin) 구조의 여행 예약 플랫폼. 팀 프로젝트에서 **인증 · 인가 모듈을 담당**했습니다.

- **JWT 인증 모듈** 설계·구현: 회원가입 · 로그인 · 토큰 재발급 API, Access 30분 / Refresh 14일
- **Refresh Token을 Redis**(`@RedisHash` + TTL)에 저장해 만료 · 재발급 · 중복 로그인 관리
- **Google / Kakao OAuth2 소셜 로그인** 연동, 이메일 미제공 계정은 `GUEST`로 임시 가입 → 이메일 인증번호(Redis 3분) 검증 후 `USER` 토큰을 발급하는 **단계적 온보딩 플로우**
- `USER` · `SELLER` · 관리자 3종(`SUPER_ADMIN`, `USER_ADMIN`, `SELLER_ADMIN`)에 임시 가입 `GUEST`, 차단 `BLACKLIST`까지 **역할 7종** 기반 접근 제어, 기본 차단 + 공개 경로 명시 방식의 Security 설정
- 로그인 실패 · 권한 거부가 500으로 나가던 응답을 **401 · 403으로 정리**하고 MockMvc 테스트로 고정
- 수동 진단 후 이행 점검 때 **Burp Suite**로 재현하며 백엔드 코드를 바로 수정: 로그인 5회 실패 잠금, 실패 메시지 통일, 쿠키 SameSite=Lax, XSS 이스케이프, CSRF 쿠키 필터 ([사용자](https://github.com/UR-VULN/Onde_Backend/tree/audit/user-remediation/yr) · [관리자](https://github.com/UR-VULN/Onde_Backend/tree/audit/admin-remediation/yr) 이행 점검 브랜치)
- 국세청 **사업자 진위확인 API** 연동, 로컬 스텁 서버로 응답 분기 테스트
- 인증 기능을 화면까지 직접 연결: [Onde_Frontend](https://github.com/UR-VULN/Onde_Frontend)(React 19 · TypeScript)에 **소셜 로그인 신규 가입자 이메일 인증 페이지**, 사용자·판매자 프로필 화면, 관리자 역할별 메뉴 제어를 구현하고 Nginx 리버스 프록시로 소셜 로그인 리다이렉트 처리
- 프론트 API 호출 61개를 백엔드 계약과 대조해 응답 형식(ApiResponse)·식별자 불일치를 정리, 누락 0건 확인

### 🔎 ARGUS - 웹 취약점 자동 진단 · 증적 · 결과서 생성
[![repo](https://img.shields.io/badge/GitHub-UR--ARGUS%2FARGUS__Merge-181717?logo=github)](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev)
`Python` `FastAPI` `Docker` `OWASP ZAP` `Playwright`

SK쉴더스 WEB/API 개발 보안 가이드(2022) 항목을 자동 진단하고 증적 스크린샷과 결과서(HTML → PDF)를 만들어 주는 도구. ONDE를 진단 대상으로 사용했습니다. **SSRF / File Inclusion(1-4) 진단 엔진**과 **"요청 및 응답 값 내 주요정보 포함 여부"(5-2) 항목의 증적 · 결과서 엔진을 담당**했습니다. (코드는 `dev` 브랜치)

**1-4 SSRF / File Inclusion 진단 엔진** · [코드](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/diagnosis/modules/1-4)
- OpenAPI(Swagger) · URL 목록 입력을 정규화하고, 파라미터 이름 · 스키마로 SSRF / LFI / RFI 의심 파라미터를 정적 탐색
- baseline 응답 비교, 반복 타이밍 측정, 저장 후 재조회로 판정하고 OWASP ZAP Active Scan 결과를 병합, 결과마다 `HIGH / MEDIUM / LOW` 신뢰도와 근거를 기록
- 서로 다른 페이로드가 거의 같은 응답을 내면 신뢰도를 낮추고, 대조 URL도 타임아웃이면 판정을 보류하는 오탐 방지 규칙
- 다중 역할 계정 로그인 · 401 시 토큰 갱신 후 재시도, 역할별 결과 중복 제거, `unittest` 테스트 38개
- 응답 차이와 지연만으로는 정상 기능과 공격 성공을 구분하기 어렵고 외부 콜백(OOB) 검증을 기간 안에 갖추기 어려워, 검증 끝에 **팀 논의로 자동 진단에서 수동 진단 항목으로 전환**

**5-2 증적 · 결과서** · [증적](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/screenshot/modules/5-2) · [결과서](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/report/modules/5-2)
- 브라우저 화면과 요청/응답 값을 함께 담는 **증적 스크린샷 엔진**, 재진단 시 이전 스크린샷 자동 정리
- 노출을 엔드포인트 × 계정 단위로 묶어 한 장에 마스킹 안 된 값을 함께 강조하고, 인증 헤더 · 쿠키는 가려서 저장
- 진단 결과를 공통 형식의 HTML 결과서로 병합해 Playwright로 PDF 자동 렌더링

### 💊 PharmLink (팜링크) - 의약품 도매 ERP 백엔드
[![backend](https://img.shields.io/badge/GitHub-MJ--PharmLink%2FBackend-181717?logo=github)](https://github.com/MJ-PharmLink/Backend/tree/dev) [![frontend](https://img.shields.io/badge/GitHub-MJ--PharmLink%2FFrontend-181717?logo=github)](https://github.com/MJ-PharmLink/Frontend/tree/dev)
`Spring Boot 4` `Java 21` `Spring Security` `JWT` `JPA` `MySQL 8` `Flyway` `JUnit 5` `Mockito` `Postman` `React` `TypeScript`

의약품 도매업체의 거래처 · 주문 · 재고 · 매입 · 매출을 관리하는 ERP (캡스톤 디자인, 백엔드 3명, 진행 중). **백엔드 공통 기반과 인증 · 사용자 · 거래처 · 회사 정보 API를 담당**하고, 담당 기능은 프론트 화면 연동까지 했습니다. (코드는 `dev` 브랜치)

- DB 설계서의 **16개 테이블을 Flyway 마이그레이션**으로 먼저 작성하고 `ddl-auto=validate`로 엔티티와 스키마가 어긋나면 서버가 뜨지 않게 고정
- 공통 응답 · ErrorCode · 전역 예외 처리 · 권한 상수를 만들어 팀원 API가 같은 형식을 쓰도록 정리, 저장은 KST · 응답은 UTC로 통일
- **JWT 인증**: refresh token은 SHA-256 해시로 사용자당 1건만 저장하고 로그아웃 · 비밀번호 · 역할 변경 · 비활성화 시 즉시 폐기, 재발급은 존재 · 폐기 · 만료 · 계정 활성 4단계 검사
- 사용자 관리: 로그인 실패는 계정 존재 여부와 관계없이 같은 401, 관리자 본인 강등 · 비활성화는 422로 차단
- 거래처 관리: 진행 중 거래가 있으면 비활성화를 409로 차단, 거래 이력은 주문 · 매출 · 매입을 네이티브 SQL(`UNION ALL`)로 조회
- JUnit 5 · Mockito 단위 테스트 10개, Postman 컬렉션 4개(요청 129개 · 검증 270건)
- 프론트 코드는 팀원이 만들었고, 제 API를 쓰는 **사용자 · 거래처 · 회사 화면을 실제 API에 연결**: 새로고침 시 `/me`로 세션 복원, 로그아웃 시 서버 토큰 폐기, 세션 만료 처리

---

## 📄 더 보기
프로젝트 상세 내용과 역할은 포트폴리오에 정리되어 있습니다.
