# PaymentSimulator-Kotlin

# 결제 시스템 시뮬레이터 프로젝트 계획

<!-- 프로젝트의 전체 개요와 목적. 토스 Data Analyst 포지션 포트폴리오와의 연계를 강조 -->
이 문서는 Kotlin과 Spring Boot로 구현되는 결제 시스템 시뮬레이터 프로젝트의 계획을 정의합니다. 이 시스템은 가상 결제 흐름을 시뮬레이션하고, `Fintech-Data-Analytics-Portfolio`의 데이터 분석 프로젝트(결제 서비스 사용자 행동 분석, A/B 테스트, 이탈 예측)에 활용될 거래 데이터를 생성합니다. 프로젝트는 토스 Data Analyst 포지션 지원을 위한 포트폴리오로 사용되며, 핀테크 환경의 안정성과 확장성을 반영합니다.

## 1. 프로젝트 계획
<!-- 프로젝트의 범위, 일정, 목표를 정의 -->
- **목적**: 가상 결제 시스템을 구현해 거래 데이터를 생성하고, 데이터 분석 프로젝트의 데이터 소스로 활용.
- **범위**: 사용자 관리, 결제 처리, 데이터 생성 API 구현. 실제 PG(결제 게이트웨이) 연동은 제외.
- **일정**: 4주 (2025년 5월 10일 기준, 6월 7일 완료).
  - 주 1: 환경 설정 및 설계.
  - 주 2: 사용자 관리 및 결제 API 구현.
  - 주 3: 데이터 생성 시뮬레이터 및 테스트.
  - 주 4: 문서화 및 최적화.
- **성과물**: RESTful API, PostgreSQL 데이터베이스, Swagger 문서, GitHub 리포지토리(`Fintech-Data-Analytics-Portfolio/payment-simulator`).

## 2. 주요 기능
<!-- 시스템의 핵심 기능. PRD 기반으로 간결히 정리 -->
- **사용자 관리**:
  - 사용자 등록: `POST /api/users` (속성: user_id, email, created_at).
  - 사용자 조회: `GET /api/users/{user_id}`.
  - 출력: JSON 형식 사용자 데이터.
- **결제 처리**:
  - 결제 요청: `POST /api/payments` (속성: user_id, amount, currency, payment_method).
  - 결제 승인: `PATCH /api/payments/{payment_id}/approve`.
  - 결제 취소: `PATCH /api/payments/{payment_id}/cancel`.
  - 출력: JSON 형식 결제 상태.
- **데이터 생성**:
  - 테스트 데이터 생성: `POST /api/simulate/transactions` (지정된 사용자 수와 거래 건수로 데이터 생성).
  - 예: 100명 사용자, 각 1~10건 거래.
- **데이터 저장**:
  - PostgreSQL에 `users`와 `payments` 테이블 저장.
  - 결제 상태 관리: REQUESTED, APPROVED, CANCELED, FAILED.

## 3. 기술 스택
<!-- 구현에 사용할 기술. 토스의 기술 환경(Kotlin, Spring Boot)과 호환성 고려 -->
- **언어**: Kotlin
- **프레임워크**: Spring Boot (REST API, Spring Data JPA)
- **데이터베이스**: PostgreSQL
- **빌드 도구**: Gradle
- **테스트**: JUnit, Testcontainers
- **문서화**: Swagger (OpenAPI)
- **도구**: Docker, Git, IntelliJ IDEA
- **로깅/모니터링**: Spring Boot Actuator

## 4. 구현 계획
<!-- 주차별 개발 단계. 실습 중심으로 실행 가능하도록 설계 -->
- **주 1: 환경 설정 및 설계**
  - IntelliJ IDEA, PostgreSQL, Docker 설치.
  - Spring Boot 프로젝트 생성, Gradle 설정.
  - 데이터베이스 스키마 설계 (`users`, `payments` 테이블).
  - API 명세 작성 (Swagger 초기 설정).
  - GitHub에 `payment-simulator` 폴더 초기 커밋.
- **주 2: 사용자 관리 및 결제 API 구현**
  - 사용자 엔터티와 리포지토리 구현 (Spring Data JPA).
  - `POST /api/users`, `GET /api/users/{user_id}` API 개발.
  - 결제 엔터티와 상태 관리 로직 구현.
  - `POST /api/payments`, `PATCH /api/payments/{payment_id}/approve`, `PATCH /api/payments/{payment_id}/cancel` API 개발.
  - 트랜잭션 처리 (`@Transactional`) 추가.
- **주 3: 데이터 생성 및 테스트**
  - `POST /api/simulate/transactions` API 구현, 랜덤 데이터 생성 로직 추가.
  - JUnit으로 단위 테스트 (컨트롤러, 서비스 레이어).
  - Testcontainers로 통합 테스트 (PostgreSQL 환경).
  - 1,000건 거래 데이터 생성 및 저장 검증.
- **주 4: 문서화 및 최적화**
  - Swagger UI로 API 문서화 완료.
  - 성능 테스트 (JMeter로 초당 100건 요청 검증).
  - 코드 리팩토링 및 로깅 추가 (Actuator).
  - `payment-simulator` README 작성 (설치, 실행, API 사용법).
  - GitHub PR 생성 및 병합.

## 5. 성능 목표
<!-- 시스템의 성능 기준. 핀테크 환경의 안정성과 효율성 반영 -->
- **처리량**: 초당 100건의 결제 요청 처리.
- **응답 시간**: 평균 API 응답 시간 200ms 이내.
- **데이터베이스**: 10,000건 거래 데이터 저장 시 쿼리 응답 시간 50ms 이내.
- **확장성**: 마이크로서비스 구조를 고려한 모듈화 (별도 모듈로 분리 가능).
- **가용성**: 로컬 환경에서 99.9% 가동률.

## 6. 테스트 계획
<!-- 시스템의 신뢰성을 보장하기 위한 테스트 전략 -->
- **단위 테스트**:
  - 대상: 컨트롤러, 서비스, 리포지토리 레이어.
  - 도구: JUnit, Mockito.
  - 목표: 80% 이상 코드 커버리지.
  - 예: 결제 상태 전환 로직 테스트.
- **통합 테스트**:
  - 대상: API 엔드포인트 및 데이터베이스 연동.
  - 도구: Testcontainers (PostgreSQL 컨테이너).
  - 예: 결제 요청부터 승인까지의 전체 흐름 검증.
- **성능 테스트**:
  - 도구: JMeter.
  - 시나리오: 100명의 동시 사용자가 1,000건 결제 요청.
  - 목표: 초당 100건 처리, 응답 시간 200ms 이내.
- **보안 테스트**:
  - JWT 인증 구현 및 토큰 유효성 검증.
  - SQL 인젝션 방지 (파라미터화된 쿼리 사용).
- **수동 테스트**:
  - Swagger UI로 API 호출 테스트.
  - 데이터 생성 API로 1,000건 데이터 생성 및 분석 프로젝트 연계 확인.

## 참고
<!-- 프로젝트 실행에 필요한 추가 정보 -->
- **리포지토리**: `Fintech-Data-Analytics-Portfolio/payment-simulator` (https://github.com/[사용자명]/Fintech-Data-Analytics-Portfolio).
- **PRD**: 결제 시스템 시뮬레이터 PRD (`docs/pr
