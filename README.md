# 안녕하세요, 백엔드 개발자 이예린입니다 👋

> **AI의 답을 믿기 전에, 기준부터 세우는** Java/Spring 백엔드 개발자입니다.
> 주로 **인증 · 권한**을 맡았고, 맡은 기능은 **API부터 화면까지 직접 연결**합니다. AI로 만든 결과는 그대로 쓰지 않고 **판정 기준을 세워 검증**합니다.
> SK쉴더스 루키즈 지능형 애플리케이션 개발 트랙 5기 (2026.02 ~ 2026.07) · 최종 프로젝트 **대상** 수상

- 🧰 **Backend** Java · Spring Boot · Spring Security · JPA · MySQL · MariaDB · Redis · Flyway
- 🖥 **Frontend** React · TypeScript
- 🛠 **Test · Tools** JUnit 5 · Mockito · Postman · Docker · Git · Python · Playwright

---

## 🛠 Projects

### ✈️ ONDE (온데) - 항공 · 숙소 · 렌터카 통합 여행 예약 서비스
[![repo](https://img.shields.io/badge/GitHub-UR--VULN%2FOnde__Backend-181717?logo=github)](https://github.com/UR-VULN/Onde_Backend)
`Spring Boot 3` `Java 17` `Spring Security` `JWT` `OAuth2` `Redis` `MariaDB` `Flyway` `React` `TypeScript`

2026.05 ~ 2026.07 · 부트캠프 최종 프로젝트(대상) · Spring Boot 멀티 모듈(core / api / admin) 구조. **팀장 겸 PM**으로 일정을 조율하고 **인증 · 인가**를 담당했습니다.

- **JWT 인증** 설계·구현: 회원가입 · 로그인 · 토큰 재발급 API, Access 30분 / Refresh 14일, Refresh Token은 **Redis**(`@RedisHash` + TTL)에 저장
- **Google / Kakao OAuth2 소셜 로그인**: 토큰은 HttpOnly · Secure 쿠키로 전달, 이메일 제공 동의를 받지 못한 카카오 계정은 `GUEST`로 임시 가입
- `USER` · `SELLER` · 관리자 3종(`SUPER_ADMIN`, `USER_ADMIN`, `SELLER_ADMIN`)에 임시 가입 `GUEST`, 차단 `BLACKLIST`까지 **역할 7종** 기반 접근 제어, 기본 차단 + 공개 경로 명시 방식의 Security 설정
- **500 에러를 401 · 403으로 분리**: 역할 이름이 Enum과 `@PreAuthorize`에서 달라 관리자 API가 500을 내던 문제를 Enum 하나로 통일(15개 파일)하고, 로그인 실패 401 · 차단 · 권한 거부 403 핸들러를 MockMvc 테스트로 고정 (권한 규칙 39개 중 28개 작성)
- **프론트 API 호출 61개 재대조**: 백엔드 응답 형식(ApiResponse) 변경으로 로그인 · 토큰 재발급이 깨지자, AI로 뽑은 대조표를 백엔드 코드와 다시 맞춰 어댑터를 고치고 이름이 바뀐 필드 10곳은 새 이름 · 예전 이름을 모두 읽게 처리, 빌드와 재대조로 누락 0건 확인
- 국세청 **사업자 진위확인 API** 연동, 로컬 스텁 서버로 응답 분기 테스트
- 수동 진단 후 이행 점검 때 **Burp Suite**로 재현하며 백엔드 코드를 바로 수정: 로그인 5회 실패 잠금, 실패 메시지 통일, 쿠키 SameSite=Lax, XSS 이스케이프, CSRF 토큰 쿠키 ([사용자](https://github.com/UR-VULN/Onde_Backend/tree/audit/user-remediation/yr) · [관리자](https://github.com/UR-VULN/Onde_Backend/tree/audit/admin-remediation/yr) 이행 점검 브랜치)
- 인증 기능은 화면까지 직접 연결: [Onde_Frontend](https://github.com/UR-VULN/Onde_Frontend)(React 19 · TypeScript)에 사용자 · 판매자 프로필 화면, 관리자 역할별 메뉴 제어를 구현하고 Nginx 리버스 프록시로 소셜 로그인 리다이렉트 처리

### 🔎 ARGUS - WEB/API 취약점 자동 진단 · 증적 · 결과서(PDF) 생성 도구
[![repo](https://img.shields.io/badge/GitHub-UR--ARGUS%2FARGUS__Merge-181717?logo=github)](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev)
`Python` `Playwright` `OWASP ZAP` `Docker`

2026.05 ~ 2026.07 · 부트캠프 최종 프로젝트(대상) · DAST. SK쉴더스 WEB/API 개발 보안 가이드(2022)의 28개 항목을 자동 진단하고 증적 스크린샷과 결과서(HTML → PDF)를 만드는 도구로, ONDE를 진단 대상으로 사용했습니다. **SSRF / File Inclusion 진단 엔진**과 **"요청 및 응답 값 내 주요정보 포함 여부" 항목의 증적 · 결과서 엔진**을 맡았습니다. (코드는 `dev` 브랜치)

**SSRF / File Inclusion 진단 엔진** · [코드](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/diagnosis/modules/1-4)
- OpenAPI(Swagger) · URL 목록 입력을 정규화하고, 파라미터 이름 · 스키마로 SSRF / LFI / RFI 의심 파라미터를 정적 탐색
- baseline 응답 비교, 반복 타이밍 측정, 저장 후 재조회로 판정하고 OWASP ZAP Active Scan 결과를 병합, 결과마다 `HIGH / MEDIUM / LOW` 신뢰도와 근거를 기록
- **판정 기준**: 내부 파일 · 서버 흔적이 실제로 보이거나 외부 콜백이 돌아온 경우만 확정(HIGH), 응답 차이 · 지연뿐이면 신뢰도를 낮추고, 비교용 요청까지 느리면 판정 보류
- 다중 역할 계정 로그인 · 401 시 토큰 갱신 후 재시도, 역할별 결과 중복 제거, `unittest` 테스트 38개
- 엔진 코드 초안은 AI로 작성하고, 판정 기준 · 신뢰도 등급 · 오탐 검증은 직접 맡음
- 기한 안에 외부 콜백(OOB) 검증을 갖추지 못해 신뢰할 결과가 나오지 않자, 같은 기준을 28개 항목 전체에 적용하자고 팀에 제안 → **8개 항목(SSRF / File Inclusion 포함)을 비워 두지 않고 '수동 진단'으로 표시**

**요청 및 응답 값 내 주요정보 포함 증적 · 결과서** · [증적](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/screenshot/modules/5-2) · [결과서](https://github.com/UR-ARGUS/ARGUS_Merge/tree/dev/backend/report/modules/5-2)
- 브라우저 화면과 요청/응답 값을 함께 담는 **증적 스크린샷 엔진**, 재진단 시 이전 스크린샷 자동 정리
- 노출을 엔드포인트 × 계정 단위로 묶어 한 장에 마스킹 안 된 값을 함께 강조하고, 인증 헤더 · 쿠키는 가려서 저장
- 진단 결과를 공통 형식의 HTML 결과서로 병합해 Playwright로 PDF 자동 렌더링

### 💊 PharmLink (팜링크) - 중소 제약 유통업체 ERP
[![backend](https://img.shields.io/badge/GitHub-MJ--PharmLink%2FBackend-181717?logo=github)](https://github.com/MJ-PharmLink/Backend/tree/dev) 
`Spring Boot 4` `Java 21` `Spring Security` `JWT` `JPA` `MySQL 8` `Flyway` `JUnit 5` `Mockito` `Postman` `React` `TypeScript`

2026.09 ~ 진행 중 · 캡스톤 디자인. 중소 제약 유통업체의 거래처 · 주문 · 재고 · 매입 · 매출을 관리하는 ERP입니다 (백엔드 3명). **공통 기반과 인증 · 사용자 · 거래처 · 회사 정보 API**를 맡았습니다. (현재 코드는 `dev` 브랜치)

- **JWT vs 세션**: 팀원과 의견이 갈려 기획서 요구사항(API로만 연결 · 역할 3종 · 역할 변경 · 비활성화 즉시 반영)을 기준으로 비교한 뒤 JWT를 선택하고, 세션의 장점인 즉시 반영은 조건으로 보완 (access 1시간, refresh는 SHA-256 해시로 사용자당 1건만 저장해 로그아웃 · 비밀번호 · 역할 변경 · 비활성화 때 즉시 폐기 → 역할 변경은 최대 1시간 안에 반영, 조건은 API 명세서에 기록)
- **역할별 접근 제어**: 관리자 · 영업 · 창고 3개 역할의 허용 범위를 권한 규칙 4개로 정리해 팀 공통으로 제공 (로그인이 필요한 API 27개 모두 같은 규칙 사용, 그중 14개 직접 작성), 로그인 · 재발급 외 전부 토큰 필수, 미인증 401 · 권한 없음 403
- 사용자 관리: 비활성 계정은 비밀번호가 맞을 때만 403으로 알리고 틀리면 같은 401로 응답해 계정 상태를 숨김, 관리자 본인 강등 · 비활성화는 422로 차단
- 공통 기반: DB 설계서의 16개 테이블을 Flyway 마이그레이션으로 작성하고 `ddl-auto=validate`로 엔티티와 스키마가 어긋나면 서버가 뜨지 않게 고정, 공통 응답 · ErrorCode · 전역 예외 처리, 저장은 KST · 응답은 UTC로 통일
- 거래처 관리: 진행 중 거래가 있으면 비활성화를 409로 차단, 거래 이력은 주문 · 매출 · 매입을 네이티브 SQL(`UNION ALL`)로 조회
- JUnit 5 · Mockito 단위 테스트 10개, Postman 컬렉션 4개(요청 129개 · 검증 270건, 그중 401 · 403 응답 검증 28건)
- 담당 기능은 화면까지 직접 연결: [MJ-PharmLink/Frontend](https://github.com/MJ-PharmLink/Frontend/tree/dev)(React 18 · TypeScript)에서 팀원이 만든 사용자 · 거래처 · 회사 정보 화면을 샘플 데이터에서 실제 API로 바꾸고, 새로고침 시 `/me`로 로그인 상태 복원, 로그아웃 시 서버 토큰 폐기, refresh 토큰까지 만료되면 로그인 화면으로 이동하게 처리, 백엔드에는 프론트 개발 서버 CORS 허용 설정 추가

---

