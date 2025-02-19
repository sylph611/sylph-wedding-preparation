# 결혼 준비 사이트 개발 개요

## 프로젝트 개요
사용자는 결혼 준비를 효율적으로 관리하기 위해 프론트엔드와 백엔드가 분리된 웹사이트를 개발 중이다. 

- **프론트엔드**: React
- **백엔드**: Java Spring Framework + Spring Boot
- **데이터베이스**: MySQL

## 핵심 기능
### 1. 회원 관리
- 회원가입, 로그인, 비밀번호 관리
- 회원별 일정 및 예산 데이터 관리

### 2. 일정 관리
- 일정 추가/수정/삭제
- 이력서 스타일의 타임라인 뷰 (가로/세로 전환 가능)
- 일정별 상태 관리 (예정, 진행 중, 완료)

### 3. 예산 관리
- 예산 항목별 관리 (`budget_items` 테이블)
- 항목별 세부 내역 관리 (`budget_details` 테이블)
- 일정과 예산 연동 가능
- 여러 개의 견적 관리 (`budget_quotes` 테이블)

### 4. 알림 기능
- **이메일 알림**: 일정 마감, 새로운 일정 추가 등
- **슬랙 알림**: 사용자별 API 키 저장, 개인 DM 또는 특정 채널로 알림 전송 가능
- **알림 설정**: `notification_settings`, `notification_details` 테이블로 구성

## 데이터베이스 설계
테이블 목록:
1. **`members`**: 회원 정보 저장
    - `id` (PK)
    - `name`
    - `email`
    - `password`
    - `created_at`, `updated_at`

2. **`schedules`**: 일정 정보 저장
    - `id` (PK)
    - `member_id` (FK)
    - `title`
    - `date`
    - `description`
    - `status` (ENUM: `PLANNED`, `IN_PROGRESS`, `COMPLETED`)
    - `created_at`, `updated_at`

3. **`notification_settings`**: 사용자별 알림 설정 저장
    - `id` (PK)
    - `member_id` (FK)
    - `created_at`, `updated_at`

4. **`notification_details`**: 알림 유형 및 설정 저장
    - `id` (PK)
    - `notification_setting_id` (FK)
    - `notification_type` (ENUM: `EMAIL`, `SLACK`, `SMS`)
    - `enabled`
    - `value`
    - `created_at`, `updated_at`

5. **`budget_items`**: 예산 항목 정보 저장
    - `id` (PK)
    - `member_id` (FK)
    - `name`
    - `total_amount`
    - `payment_status` (ENUM: `PENDING`, `PAID`)
    - `linked_schedule_id` (FK, Nullable)
    - `created_at`, `updated_at`

6. **`budget_details`**: 예산 항목의 세부 내역 저장
    - `id` (PK)
    - `budget_item_id` (FK)
    - `description`
    - `amount`
    - `payment_status` (ENUM: `PENDING`, `PAID`)
    - `created_at`, `updated_at`

7. **`budget_quotes`**: 예산 항목별 여러 개의 견적 저장
    - `id` (PK)
    - `budget_item_id` (FK)
    - `provider`
    - `quote_amount`
    - `description`
    - `quote_date`
    - `created_at`, `updated_at`

## 추가 사항
- 모든 테이블에 `created_at`, `updated_at` 컬럼 추가
- `ENUM` 값은 대문자로 변환 (`PLANNED`, `IN_PROGRESS`, `COMPLETED`, `PENDING`, `PAID`)

이 문서는 프로젝트의 데이터베이스 설계 및 핵심 기능을 정리한 내용이다. 추가적으로 변경할 사항이 있으면 반영하여 업데이트할 수 있다.
