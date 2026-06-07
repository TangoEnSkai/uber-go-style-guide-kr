# CHANGELOG

## 2026/06/07

원문 기준: [2023-05-09](https://github.com/uber-go/guide/blob/master/CHANGELOG.md#2023-05-09)까지 반영

### 코드 오류 수정
- Error Wrapping: `%s` → `%w` 코드 오류 수정 ([#60](https://github.com/TangoEnSkai/uber-go-style-guide-kr/pull/60))
- Don't Panic: deprecated `ioutil.TempFile` → `os.CreateTemp`, 예시 코드 원문 복원 ([#61](https://github.com/TangoEnSkai/uber-go-style-guide-kr/pull/61))

### 누락 내용 복구 및 업데이트
- Embedding in Structs: 임베딩 금지 조건 12가지 및 코드 예시 복구 ([#62](https://github.com/TangoEnSkai/uber-go-style-guide-kr/pull/62))
- Reduce Scope of Variables: 상수 스코프 예시 추가 ([#63](https://github.com/TangoEnSkai/uber-go-style-guide-kr/pull/63))
- Functional Options: struct 기반 Option 패턴으로 전면 업데이트 ([#64](https://github.com/TangoEnSkai/uber-go-style-guide-kr/pull/64))

### 신규 번역 (원문 2019-10-21 ~ 2023-05-09)
- Errors 섹션 재편 + Error Naming, Handle Errors Once
- Exit in Main / Exit Once
- Use field tags in marshaled structs
- Don't fire-and-forget goroutines
- Prefer Specifying Container Capacity (Map/Slice 힌트)
- Style: Avoid overly long lines, Be Consistent, Initializing Maps
- Style: Initializing Structs 서브섹션 재편
- Patterns: Test Tables 서브섹션 (Avoid Unnecessary Complexity, Parallel Tests)
- Linting 섹션 전체

### 스타일 통일
- 구형 번역 헤딩 6개에 영문 병기 추가 (`### 한국어 (English)` 형식)

## 2022/06/07

- Introduction 부분 내용 업데이트 및 재번역

## 2019/10/17

- 초벌 번역 완료
- [uber-go/guide](https://github.com/uber-go/guide)의 README.md에 링크 추가 [PR #47](https://github.com/uber-go/guide/pull/47)
- Public에 공개됨

---

## 2019/10/16

- 우버의 [uber-go/guide](https://github.com/uber-go/guide)에 한국어 번역 관련 대화
- 한국어 번역 repo를 따로 개설해서 번역본은 본래의 repo와 별개로 관리 (중국어 버전 PR도 이런 방식을 따르고 있음)

---
