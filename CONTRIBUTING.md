# 기여 가이드 (Contributing Guide)

이 리포지토리는 [uber-go/guide](https://github.com/uber-go/guide)의 한국어 번역입니다.
기여를 환영합니다! 아래 가이드를 따라 주세요.

---

## 번역 규칙

### 1. 헤딩 형식
모든 섹션 헤딩은 **한국어 번역 + 영문 원문** 형식을 사용합니다.

```markdown
### 한국어 번역 (English Original)
```

예시:
```markdown
### 에러를 한 번만 처리하라 (Handle Errors Once)
### 컨테이너 용량 지정을 선호하라 (Prefer Specifying Container Capacity)
```

### 2. 코드 블록
- **코드 자체는 원문과 100% 동일하게 유지**합니다. 절대 수정하지 마세요.
- **코드 내 주석은 한국어로 번역**합니다.

```go
// Good: 한국어 주석
func isActive(now, start, stop time.Time) bool {
  return (start.Before(now) || start.Equal(now)) && now.Before(stop)
}
```

### 3. 기술 용어
아래 용어는 번역하지 않고 원어를 사용합니다:

| 번역 금지 용어 |
|---|
| goroutine, mutex, channel, struct, interface |
| slice, map, pointer, receiver, embedding |
| package, import, defer, panic |
| Bad / Good (표 헤더) |

### 4. 표 형식
Bad/Good 비교 표는 원문의 HTML 테이블 구조를 그대로 유지합니다.

```html
<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>
...
</td><td>
...
</td></tr>
</tbody></table>
```

---

## PR 작성 방법

### 브랜치 명명 규칙
```
fix/issue-{번호}-{간단한설명}
feat/issue-{번호}-{간단한설명}
style/issue-{번호}-{간단한설명}
```

### 커밋 메시지 형식
```
fix(docs): 변경 내용 요약 (closes #이슈번호)
feat(docs): 변경 내용 요약 (closes #이슈번호)
style(docs): 변경 내용 요약 (closes #이슈번호)
```

### PR 체크리스트
PR을 올리기 전에 아래를 확인해 주세요:

- [ ] 원문([uber-go/guide](https://github.com/uber-go/guide/blob/master/style.md))과 번역 내용 대조
- [ ] 코드 블록이 원문과 100% 동일한지 확인
- [ ] 헤딩 형식이 `### 한국어 (English)` 형식인지 확인
- [ ] 관련 이슈 번호가 PR body에 포함되었는지 확인

---

## 원본 업데이트 추적

원본 리포의 변경사항은 [CHANGELOG.md](https://github.com/uber-go/guide/blob/master/CHANGELOG.md)에서 확인할 수 있습니다.

번역 리포의 현재 반영 기준은 이 리포의 [CHANGELOG.md](./CHANGELOG.md)를 참고하세요.

---

## 이슈 작성

번역 오류, 누락, 개선 제안은 이슈로 등록해 주세요.

이슈 제목 형식:
```
[번역] 섹션명: 문제 설명
[오류] 섹션명: 코드/내용 오류 설명
[내용 누락] 섹션명: 누락된 내용 설명
```
