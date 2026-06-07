# uber-go-style-guide-kr

- **Translated in Korean**  

  - First translation done with original doc on 17th of Oct, 2019 from [uber-go/guide](https://github.com/uber-go/guide)
  - Please feel free to fork and PR if you find any updates, issues or improvement.

- **한국어 번역본**
  - 초벌 번역은 [uber-go/guide](https://github.com/uber-go/guide)의 2019년 10월 17일 의 style.md 파일을 기반으로 완성되었음.
  - 기술 용어에 대한 과도한 한국어 번역은 지양하였으며, 특정 용어에 대한 한국어 번역을 했을 때에는 괄호로 원문의 단어를 살려두어 최대한 원문의 의도를 왜곡하지 않는 방향에서 번역 함.

---

<!--

Editing this document:

- Discuss all changes in GitHub issues first.
- Update the table of contents as new sections are added or removed.
- Use tables for side-by-side code samples. See below.

Code Samples:

Use 2 spaces to indent. Horizontal real estate is important in side-by-side
samples.

For side-by-side code samples, use the following snippet.

~~~
<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
BAD CODE GOES HERE
```

</td><td>

```go
GOOD CODE GOES HERE
```

</td></tr>
</tbody></table>
~~~

(You need the empty lines between the <td> and code samples for it to be
treated as Markdown.)

If you need to add labels or descriptions below the code samples, add another
row before the </tbody></table> line.

~~~
<tr>
<td>DESCRIBE BAD CODE</td>
<td>DESCRIBE GOOD CODE</td>
</tr>
~~~

-->

# Uber의 Go언어 스타일 가이드 (Uber's Go Style Guide)

- [uber-go-style-guide-kr](#uber-go-style-guide-kr)
- [Uber의 Go언어 스타일 가이드 (Uber's Go Style Guide)](#uber의-go언어-스타일-가이드-ubers-go-style-guide)
  - [소개 (Introduction)](#소개-introduction)
  - [가이드라인 (Guidelines)](#가이드라인-guidelines)
    - [인터페이스에 대한 포인터 (Pointers to Interfaces)](#인터페이스에-대한-포인터-pointers-to-interfaces)
    - [인터페이스 컴플라이언스 검증 (Verify Interface Compliance)](#인터페이스-컴플라이언스-검증-verify-interface-compliance)
    - [리시버(Receivers)와 인터페이스(Interfaces)](#리시버receivers와-인터페이스interfaces)
    - [제로 값 뮤텍스(Zero-value Mutexes)는 유효하다 (Zero-value Mutexes are Valid)](#제로-값-뮤텍스zero-value-mutexes는-유효하다-zero-value-mutexes-are-valid)
    - [바운더리에서 슬라이스 및 맵 복사 (Copy Slices and Maps at Boundaries)](#바운더리에서-슬라이스-및-맵-복사-copy-slices-and-maps-at-boundaries)
      - [슬라이스와 맵 수신](#슬라이스와-맵-수신)
      - [슬라이스와 맵 반환](#슬라이스와-맵-반환)
    - [Clean Up 하기 위한 Defer (Defer to Clean Up)](#clean-up-하기-위한-defer-defer-to-clean-up)
    - [채널의 크기(Channel Size)는 하나(One) 혹은 제로(None) (Channel Size is One or None)](#채널의-크기channel-size는-하나one-혹은-제로none-channel-size-is-one-or-none)
    - [Enums은 1에서부터 시작하라 (Start Enums at One)](#enums은-1에서부터-시작하라-start-enums-at-one)
    - [시간을 처리하려면 `"time"`을 사용하라 (Use `"time"` to handle time)](#시간을-처리하려면-time을-사용하라-use-time-to-handle-time)
      - [시간의 순간(instants of time)을 나타내기 위해서는 `time.Time` 를 사용하라](#시간의-순간instants-of-time을-나타내기-위해서는-timetime-를-사용하라)
      - [시간의 기간(periods of time)을 나타내기 위해 `time.Duration` 을 사용하라](#시간의-기간periods-of-time을-나타내기-위해-timeduration-을-사용하라)
      - [`time.Time` 과 `time.Duration`을 외부 시스템과 사용하기](#timetime-과-timeduration을-외부-시스템과-사용하기)
    - [에러 (Errors)](#에러-errors)
      - [에러 형(Error Types)](#에러-형error-types)
      - [오류 래핑(Error Wrapping)](#오류-래핑error-wrapping)
      - [에러 이름 짓기 (Error Naming)](#에러-이름-짓기-error-naming)
      - [에러를 한 번만 처리하라 (Handle Errors Once)](#에러를-한-번만-처리하라-handle-errors-once)
    - [타입의 어설션 실패 다루기 (Handle Type Assertion Failures)](#타입의-어설션-실패-다루기-handle-type-assertion-failures)
    - [패닉을 피할 것 (Don't Panic)](#패닉을-피할-것-dont-panic)
    - [go.uber.org/atomic의 사용](#gouberorgatomic의-사용)
    - [변경 가능한(mutable) 전역변수 피하기 (Avoid Mutable Globals)](#변경-가능한mutable-전역변수-피하기-avoid-mutable-globals)
    - [공개 구조체(public struct)에서 내장 타입들(Embedding Types) 사용하지 않기 (Avoid Embedding Types in Public Structs)](#공개-구조체public-struct에서-내장-타입들embedding-types-사용하지-않기-avoid-embedding-types-in-public-structs)
    - [내장된(built-in) 이름 사용을 피하라 (Avoid Using Built-In Names)](#내장된built-in-이름-사용을-피하라-avoid-using-built-in-names)
    - [`init()` 사용을 피하라 (Avoid `init()`)](#init-사용을-피하라-avoid-init)
    - [Main에서 종료하기 (Exit in Main)](#main에서-종료하기-exit-in-main)
      - [한 번만 종료하라 (Exit Once)](#한-번만-종료하라-exit-once)
    - [직렬화되는 구조체에 필드 태그를 사용하라 (Use field tags in marshaled structs)](#직렬화되는-구조체에-필드-태그를-사용하라-use-field-tags-in-marshaled-structs)
    - [Goroutine을 Fire-and-Forget 방식으로 실행하지 마라 (Don't fire-and-forget goroutines)](#goroutine을-fire-and-forget-방식으로-실행하지-마라-dont-fire-and-forget-goroutines)
      - [Goroutine이 종료될 때까지 대기하라 (Wait for goroutines to exit)](#goroutine이-종료될-때까지-대기하라-wait-for-goroutines-to-exit)
      - [`init()`에서 goroutine을 사용하지 마라 (No goroutines in `init()`)](#init에서-goroutine을-사용하지-마라-no-goroutines-in-init)
  - [성능(Performance)](#성능performance)
    - [`fmt` 보다 `strconv` 선호](#fmt-보다-strconv-선호)
    - [string-to-byte 변환을 피해라 (Avoid repeated string-to-byte conversions)](#string-to-byte-변환을-피해라-avoid-repeated-string-to-byte-conversions)
    - [컨테이너 용량 지정을 선호하라 (Prefer Specifying Container Capacity)](#컨테이너-용량-지정을-선호하라-prefer-specifying-container-capacity)
      - [Map 용량 힌트 지정 (Specifying Map Capacity Hints)](#map-용량-힌트-지정-specifying-map-capacity-hints)
      - [슬라이스 용량 지정 (Specifying Slice Capacity)](#슬라이스-용량-지정-specifying-slice-capacity)
  - [스타일 (Style)](#스타일-style)
    - [지나치게 긴 줄을 피하라 (Avoid overly long lines)](#지나치게-긴-줄을-피하라-avoid-overly-long-lines)
    - [일관성을 유지하라 (Be Consistent)](#일관성을-유지하라-be-consistent)
    - [그룹 유사 선언 (Group Similar Declarations)](#그룹-유사-선언-group-similar-declarations)
    - [Import 그룹 정리/배치 (Import Group Ordering)](#import-그룹-정리배치-import-group-ordering)
    - [패키지 이름 (Package Names)](#패키지-이름-package-names)
    - [함수 이름 (Function Names)](#함수-이름-function-names)
    - [Import 별칭 (Import Aliasing)](#import-별칭-import-aliasing)
    - [함수 그룹화와 정렬/배치 (Function Grouping and Ordering)](#함수-그룹화와-정렬배치-function-grouping-and-ordering)
    - [중첩 감소 (Reduce Nesting)](#중첩-감소-reduce-nesting)
    - [불필요한 else (Unnecessary Else)](#불필요한-else-unnecessary-else)
    - [최상위 변수 선언 (Top-level Variable Declarations)](#최상위-변수-선언-top-level-variable-declarations)
    - [수출되지 않은 전역에 _을 붙여라 (Prefix Unexported Globals with _)](#수출되지-않은-전역에-_을-붙여라-prefix-unexported-globals-with-_)
    - [구조체에서의 임베딩 (Embedding in Structs)](#구조체에서의-임베딩-embedding-in-structs)
    - [구조체 초기화 (Initializing Structs)](#구조체-초기화-initializing-structs)
      - [구조체 초기화를 위해 필드를 사용해라 (Use Field Names to initialize Structs)](#구조체-초기화를-위해-필드를-사용해라-use-field-names-to-initialize-structs)
      - [구조체의 제로 값 필드 생략 (Omit Zero Value Fields in Structs)](#구조체의-제로-값-필드-생략-omit-zero-value-fields-in-structs)
      - [제로 값 구조체에 `var` 사용 (Use `var` for Zero Value Structs)](#제로-값-구조체에-var-사용-use-var-for-zero-value-structs)
      - [구조체 참조 초기화 (Initializing Struct References)](#구조체-참조-초기화-initializing-struct-references)
    - [지역 변수 선언 (Local Variable Declarations)](#지역-변수-선언-local-variable-declarations)
    - [nil은 유효한 슬라이스 (nil is a valid slice)](#nil은-유효한-슬라이스-nil-is-a-valid-slice)
    - [변수의 범위를 줄여라 (Reduce Scope of Variables)](#변수의-범위를-줄여라-reduce-scope-of-variables)
    - [Naked 매개변수를 피해라 (Avoid Naked Parameters)](#naked-매개변수를-피해라-avoid-naked-parameters)
    - [이스케이핑을 피하기 위해 원시 문자 리터럴 사용 (Use Raw String Literals to Avoid Escaping)](#이스케이핑을-피하기-위해-원시-문자-리터럴-사용-use-raw-string-literals-to-avoid-escaping)
    - [Map 초기화 (Initializing Maps)](#map-초기화-initializing-maps)
    - [Printf외부의 문자열 형식 (Format Strings outside Printf)](#printf외부의-문자열-형식-format-strings-outside-printf)
    - [Printf-스타일 함수의 이름 (Naming Printf-style Functions)](#printf-스타일-함수의-이름-naming-printf-style-functions)
  - [패턴 (Patterns)](#패턴-patterns)
    - [테스트 테이블 (Test Tables)](#테스트-테이블-test-tables)
      - [테스트 테이블에서 불필요한 복잡성을 피하라 (Avoid Unnecessary Complexity in Table Tests)](#테스트-테이블에서-불필요한-복잡성을-피하라-avoid-unnecessary-complexity-in-table-tests)
      - [병렬 테스트 (Parallel Tests)](#병렬-테스트-parallel-tests)
    - [기능적 옵션 (Functional Options)](#기능적-옵션-functional-options)
  - [린팅 (Linting)](#린팅-linting)
    - [린트 실행기 (Lint Runners)](#린트-실행기-lint-runners)

## 소개 (Introduction)

스타일(style)은 코드를 관리하는 규칙(convention)이다. 규칙은 오해받기 쉬운데, 단순히 `gofmt`가 수행하는 소스 코드 포맷팅 이외의 의미도 포함하기 때문이다.

이 가이드의 목표는 Uber에서 Go 코드를 작성할 때 해야 할 것과 하지 말아야 할 것을 자세히 설명하여, 컨벤션의 복잡성을 관리하는 것이다.
이 규칙은 엔지니어가 Go를 생산적으로 사용하면서도 코드를 관리 가능한 상태로 유지하기 위해 존재한다.

이는 원래 [Prashant Varanasi]와 [Simon Newton]이 동료들의 Go 개발 속도를 높이기 위해 소개했다. 수년간 피드백을 통해 꾸준히 개선되었다.

  [Prashant Varanasi]: https://github.com/prashantv
  [Simon Newton]: https://github.com/nomis52 

이 문서는 Uber에서 따르는 Go 코드 컨벤션을 정리한다. 이들 중 많은 부분이 Go에 대한 일반적 지침이고, 나머지는 외부 리소스에 따라 확장한다:

1. [Effective Go](https://go.dev/doc/effective_go)
2. [Go Common Mistakes](https://github.com/golang/go/wiki/CommonMistakes)
3. [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

모든 코드는 `golint` 및 `go vet`를 실행할 때 오류가 없어야 한다.
코드 에디터를 다음과 같이 설정할 것을 권장한다:

- 저장 시 `goimports` 자동 실행
- `golint` 및 `go vet`를 실행하여 오류 확인

여기에서 Go 도구에 대한 편집기 지원 정보를 찾을 수 있다:
<https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins>

## 가이드라인 (Guidelines)

### 인터페이스에 대한 포인터 (Pointers to Interfaces)

인터페이스에 대한 포인터는 거의 필요하지 않다.
인터페이스는 값(value)으로 전달해야 한다.
인터페이스의 기본 데이터(underlying data)는 여전히 포인터일 수 있다.

인터페이스는 두 가지 필드로 구성된다:

1. 타입별 정보(type-specific information)에 대한 포인터. 이를 "타입"으로 볼 수 있다.
2. 데이터 포인터. 저장된 데이터가 포인터일 경우 직접 저장된다. 저장된 데이터가 값이면 값에 대한 포인터가 저장된다.

인터페이스 메서드가 기본 데이터(underlying data)를 수정하도록 하려면 반드시 포인터를 사용해야 한다.

### 인터페이스 컴플라이언스 검증 (Verify Interface Compliance)

적절한 경우, 컴파일 시간에 인터페이스 컴플라이언스를 검증한다. 이는 다음을 포함한다:
- API contract의 일부로 특정 인터페이스를 구현하는데 필요한 exported 타입
- 동일한 인터페이스를 구현하는 타입의 컬렉션의 일부인 exported 또는 unexported 타입 
- 기타 인터페이스 위반으로 인해 사용자가 중단되는 경우

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Handler struct {
  // ...
}



func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  ...
}
```

</td><td>

```go
type Handler struct {
  // ...
}

var _ http.Handler = (*Handler)(nil)

func (h *Handler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

</td></tr>
</tbody></table>

`var _ http.Handler = (*Handler)(nil)`구문은 `*Handler`가 `http.Handler` 인터페이스와 일치하지 않는 경우 컴파일에 실패한다.

할당문의 우변 (the right hand side of the assignment)은 어설션된 타입의 제로 값(zero value)이어야 한다. 이것은 포인터 타입(`*Handler`와 같은), slice 및 map의 경우 `nil`이고 struct 타입의 경우 빈 구조체다. 

```go
type LogHandler struct {
  h   http.Handler
  log *zap.Logger
}

var _ http.Handler = LogHandler{}

func (h LogHandler) ServeHTTP(
  w http.ResponseWriter,
  r *http.Request,
) {
  // ...
}
```

### 리시버(Receivers)와 인터페이스(Interfaces)

값 리시버가 있는 메서드는 값뿐만 아니라 포인터에서도 호출할 수 있다.
포인터 리시버가 있는 메서드는 포인터 또는 주소 지정 가능한 값([addressable value](https://go.dev/ref/spec#Method_values))에서만 호출할 수 있다.

예를 들면,

```go
type S struct {
  data string
}

func (s S) Read() string {
  return s.data
}

func (s *S) Write(str string) {
  s.data = str
}

// 맵에 저장된 값의 포인터는 얻을 수 없다. 주소 지정이
// 불가능한 값이기 때문이다.
sVals := map[int]S{1: {"A"}}

// 값 리시버인 Read는 주소 지정이 필요 없으므로
// 맵에 저장된 값에서도 호출할 수 있다.
// 
sVals[1].Read()

// 포인터 리시버인 Write는 맵에 저장된 값의
// 포인터를 얻을 수 없으므로 호출 불가능하다.
// 
//
//  sVals[1].Write("test")

sPtrs := map[int]*S{1: {"A"}}

// 포인터를 저장하는 맵은 포인터 자체가 주소 지정 가능하므로
// Read와 Write 모두 호출할 수 있다.
sPtrs[1].Read()
sPtrs[1].Write("test")
```

마찬가지로 메서드에 값 리시버가 있더라도 인터페이스는 포인터로 충족될 수 있다.

```go
type F interface {
  f()
}

type S1 struct{}

func (s S1) f() {}

type S2 struct{}

func (s *S2) f() {}

s1Val := S1{}
s1Ptr := &S1{}
s2Val := S2{}
s2Ptr := &S2{}

var i F
i = s1Val
i = s1Ptr
i = s2Ptr

// The following doesn't compile, since s2Val is a value, and there is no value receiver for f.
//   i = s2Val
```

Effective Go에 [Pointers vs. Values]에 대한 좋은 글이 있으니 참고하기 바란다.

  [Pointers vs. Values]: https://go.dev/doc/effective_go#pointers_vs_values

### 제로 값 뮤텍스(Zero-value Mutexes)는 유효하다

`sync.Mutex` 및 `sync.RWMutex`의 제로 값(zero-value)은 유효하므로 뮤텍스에 대한 포인터가 거의 필요하지 않다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
mu := new(sync.Mutex)
mu.Lock()
```

</td><td>

```go
var mu sync.Mutex
mu.Lock()
```

</td></tr>
</tbody></table>

포인터로 구조체를 사용하는 경우, 뮤텍스는 포인터가 아닌 필드여야 한다.
구조체를 내보내지 않는 경우라도(not exported), 구조체에 뮤텍스를 포함하지 마십시오.

<table>
<tbody>
<tr><td>

```go
type smap struct {
  sync.Mutex // 오직 비공개(unexported) 타입에서만 사용

  data map[string]string
}

func newSMap() *smap {
  return &smap{
    data: make(map[string]string),
  }
}

func (m *smap) Get(k string) string {
  m.Lock()
  defer m.Unlock()

  return m.data[k]
}
```

</td><td>

```go
type SMap struct {
  mu sync.Mutex

  data map[string]string
}

func NewSMap() *SMap {
  return &SMap{
    data: make(map[string]string),
  }
}

func (m *SMap) Get(k string) string {
  m.mu.Lock()
  defer m.mu.Unlock()

  return m.data[k]
}
```

</td></tr>

</tr>
<tr>
<td>`Mutex` 필드와 `Lock` 및 `Unlock` 메서드는 의도하지 않게, `SMap`의 Exported API의 일부이다. </td>
<td>뮤텍스와 해당 메서드는 호출자에게는 숨겨진 SMap의 구현 세부 정보다.</td>
</tr>

</tbody></table>

### 바운더리에서 슬라이스 및 맵 복사 (Copy Slices and Maps at Boundaries)

슬라이스 및 맵에는 기본 데이터에 대한 포인터가 포함되어 있으므로 복사해야 하는 시나리오에 주의 할 필요가 있다.

#### 슬라이스와 맵 수신

참조(reference)를 저장하면 인수로 받은 맵이나 슬라이스를 호출자가 수정할 수 있음을 명심하자.

<table>
<thead><tr><th>Bad</th> <th>Good</th></tr></thead>
<tbody>
<tr>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = trips
}

trips := ...
d1.SetTrips(trips)

// d1.trips를 수정하려 했던 것인가?
trips[0] = ...
```

</td>
<td>

```go
func (d *Driver) SetTrips(trips []Trip) {
  d.trips = make([]Trip, len(trips))
  copy(d.trips, trips)
}

trips := ...
d1.SetTrips(trips)

// We can now modify trips[0] without affecting d1.trips.
trips[0] = ...
```

</td>
</tr>

</tbody>
</table>

#### 슬라이스와 맵 반환

마찬가지로 내부 상태를 노출하는 맵 또는 슬라이스를 호출자가 수정할 수 있으므로 주의하자.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

// Snapshot은 현재 통계를 반환한다.
func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  return s.counters
}

// snapshot은 더 이상 mutex로 보호되지 않으므로
// 접근 시 데이터 경합이 발생할 수 있다.
snapshot := stats.Snapshot()
```

</td><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  result := make(map[string]int, len(s.counters))
  for k, v := range s.counters {
    result[k] = v
  }
  return result
}

// Snapshot은 이제 복사본이다.
snapshot := stats.Snapshot()
```

</td></tr>
</tbody></table>

### Clean Up 하기 위한 Defer (Defer to Clean Up)

defer를 사용하여 파일(files) 및 잠금(locks)과 같은 리소스를 정리한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
p.Lock()
if p.count < 10 {
  p.Unlock()
  return p.count
}

p.count++
newCount := p.count
p.Unlock()

return newCount

// 여러 return으로 인해 unlock을 놓치기 쉽다
```

</td><td>

```go
p.Lock()
defer p.Unlock()

if p.count < 10 {
  return p.count
}

p.count++
return p.count

// 더 읽기 좋다
```

</td></tr>
</tbody></table>

`defer`의 오버헤드는 극히 작으므로, 함수 실행 시간이 나노초(ns) 수준임을 증명할 수 있는 경우에만 사용을 피해야 한다.
`defer` 사용으로 얻는 가독성 향상은 아주 작은 비용을 감수할 만한 가치가 있다.
이는 단순한 메모리 접근 이상의 복잡한 계산이 포함된 대형 메서드에서 특히 해당한다.

### 채널의 크기(Channel Size)는 하나(One) 혹은 제로(None) (Channel Size is One or None)

채널의 크기는 보통 1이거나 버퍼링이 없어야 한다. 기본적으로 채널은 버퍼링이 없으며 크기는 0이다. 0이 아닌 크기를 사용할 때는 충분한 검토가 필요하다. 크기를 어떻게 결정했는지, 부하 상황에서 채널이 가득 차 writer가 블록되는 것을 어떻게 방지하는지, 그리고 그런 상황이 발생했을 때 어떻게 처리할지를 고려하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// 누구에게나 충분하다!
c := make(chan int, 64)
```

</td><td>

```go
// 사이즈 1
c := make(chan int, 1) // 혹은
// 버퍼링 되지 않는 채널, 사이즈 0
c := make(chan int)
```

</td></tr>
</tbody></table>

### Enums은 1에서부터 시작하라 (Start Enums at One)

Go에서 열거형(enumeration)을 정의하는 일반적인 방법은 커스텀 타입(custom type)과 `iota`를 사용한 `const` 그룹을 선언하는 것이다.

변수의 기본값이 0이므로, 일반적으로 열거형은 0이 아닌 값으로 시작하는 것이 좋다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota
  Subtract
  Multiply
)

// Add=0, Subtract=1, Multiply=2
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

// Add=1, Subtract=2, Multiply=3
```

</td></tr>
</tbody></table>

제로 값(zero value)을 사용하는 것이 적절한 경우도 있다. 예를 들어, 0이 기본 동작으로 적합한 경우다.

```go
type LogOutput int

const (
  LogToStdout LogOutput = iota
  LogToFile
  LogToRemote
)

// LogToStdout=0, LogToFile=1, LogToRemote=2
```

<!-- TODO: section on String methods for enums -->

### 시간을 처리하려면 `"time"`을 사용하라 (Use `"time"` to handle time)

시간은 복잡하다. 시간에 대해 흔히 잘못 가정하는 것들은 다음과 같다.

1. 하루는 24시간이다
2. 한 시간은 60분이다
3. 한 주는 7일이다
4. 한 해는 365일이다
5. [그 외 많은 것들](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)

예를 들어, *1번*은 어떤 시각에 24시간을 더한다고 해서 항상 다음 날이 되는 것은 아님을 의미한다.

따라서, 시간을 다룰 때는 항상 [`"time"`](https://pkg.go.dev/time) 패키지를 사용하라. 이 패키지는 이러한 잘못된 가정들을 더 안전하고 정확하게 처리하는 데 도움을 준다.

#### 시간의 순간(instants of time)을 나타내기 위해서는 `time.Time` 를 사용하라

시간의 순간을 다룰 때는 [`time.Time`](https://pkg.go.dev/time#Time)을 사용하고, 시간을 비교하거나 더하거나 빼는 작업에는 `time.Time`의 메서드를 사용하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func isActive(now, start, stop int) bool {
  return start <= now && now < stop
}
```

</td><td>

```go
func isActive(now, start, stop time.Time) bool {
  return (start.Before(now) || start.Equal(now)) && now.Before(stop)
}
```

</td></tr>
</tbody></table>

#### 시간의 기간(periods of time)을 나타내기 위해 `time.Duration` 을 사용하라

시간의 기간을 다룰 때는 [`time.Duration`](https://pkg.go.dev/time#Duration)을 사용하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func poll(delay int) {
  for {
    // ...
    time.Sleep(time.Duration(delay) * time.Millisecond)
  }
}

poll(10) // 초인가, 밀리초인가?
```

</td><td>

```go
func poll(delay time.Duration) {
  for {
    // ...
    time.Sleep(delay)
  }
}

poll(10*time.Second)
```

</td></tr>
</tbody></table>

어떤 시각에 24시간을 더하는 예시로 돌아가서, 시간을 더하는 방법은 의도에 따라 달라진다. 같은 시각이지만 다음 날로 이동하고 싶다면 [`Time.AddDate`](https://pkg.go.dev/time#Time.AddDate)를 사용해야 한다. 반면, 이전 시각에서 정확히 24시간 후를 보장하는 시각을 원한다면 [`Time.Add`](https://pkg.go.dev/time#Time.Add)를 사용해야 한다.

```go
newDay := t.AddDate(0 /* years */, 0 /* months */, 1 /* days */)
maybeNewDay := t.Add(24 * time.Hour)
```

#### `time.Time` 과 `time.Duration`을 외부 시스템과 사용하기

외부 시스템과 상호작용할 때 가능하다면 `time.Duration`과 `time.Time`을 사용하라. 예를 들어:

- 커맨드라인 플래그: [`flag`](https://pkg.go.dev/flag)는 [`time.ParseDuration`](https://pkg.go.dev/time#ParseDuration)을 통해 `time.Duration`을 지원한다
- JSON: [`encoding/json`](https://pkg.go.dev/encoding/json)은 [`UnmarshalJSON` 메서드](https://pkg.go.dev/time#Time.UnmarshalJSON)를 통해 `time.Time`을 [RFC 3339](https://tools.ietf.org/html/rfc3339) 문자열로 인코딩하는 것을 지원한다
- SQL: [`database/sql`](https://pkg.go.dev/database/sql)은 드라이버가 지원하는 경우 `DATETIME` 또는 `TIMESTAMP` 컬럼을 `time.Time`으로 변환하는 것을 지원한다
- YAML: [`gopkg.in/yaml.v2`](https://pkg.go.dev/gopkg.in/yaml.v2)는 `time.Time`을 [RFC 3339](https://tools.ietf.org/html/rfc3339) 문자열로, `time.Duration`을 [`time.ParseDuration`](https://pkg.go.dev/time#ParseDuration)을 통해 지원한다

이러한 상호작용에서 `time.Duration`을 사용할 수 없는 경우, `int` 또는 `float64`를 사용하고 필드 이름에 단위를 포함시켜라.

예를 들어, `encoding/json`은 `time.Duration`을 지원하지 않으므로, 단위를 필드 이름에 포함시킨다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// {"interval": 2}
type Config struct {
  Interval int `json:"interval"`
}
```

</td><td>

```go
// {"intervalMillis": 2000}
type Config struct {
  IntervalMillis int `json:"intervalMillis"`
}
```

</td></tr>
</tbody></table>

이러한 상호작용에서 `time.Time`을 사용할 수 없고, 다른 방법에 합의하지 않은 경우에는 `string`을 사용하고 [RFC 3339](https://tools.ietf.org/html/rfc3339)에 정의된 형식으로 타임스탬프를 포맷하라. 이 형식은 [`Time.UnmarshalText`](https://pkg.go.dev/time#Time.UnmarshalText)에서 기본으로 사용되며, [`time.RFC3339`](https://pkg.go.dev/time#RFC3339)를 통해 `Time.Format`과 `time.Parse`에서도 사용할 수 있다.

실제로는 문제가 되지 않는 경우가 많지만, `"time"` 패키지는 윤초(leap second)가 포함된 타임스탬프 파싱([8728](https://github.com/golang/go/issues/8728))을 지원하지 않으며, 계산에서도 윤초를 고려하지 않는다([15190](https://github.com/golang/go/issues/15190)). 두 시각을 비교하면 그 사이에 발생한 윤초는 차이에 포함되지 않는다.

### 에러 (Errors)

#### 에러 형(Error Types)

에러를 선언하는 방법에는 몇 가지 옵션이 있다.
사용 케이스에 가장 적합한 옵션을 고르기 전에 다음을 고려하라.

- 호출자가 에러를 매칭하여 처리해야 하는가?
  그렇다면, 최상위 에러 변수나 커스텀 타입을 선언하여
  [`errors.Is`](https://pkg.go.dev/errors#Is) 또는 [`errors.As`](https://pkg.go.dev/errors#As) 함수를 지원해야 한다.
- 에러 메시지가 정적 문자열인가, 아니면 컨텍스트 정보가 필요한 동적 문자열인가?
  정적 문자열이면 [`errors.New`](https://pkg.go.dev/errors#New)를 사용할 수 있고,
  동적 문자열이면 [`fmt.Errorf`](https://pkg.go.dev/fmt#Errorf) 또는 커스텀 에러 타입을 사용해야 한다.
- 다운스트림 함수에서 반환된 새 에러를 전파하는가?
  그렇다면 [오류 래핑 섹션](#오류-래핑error-wrapping)을 참고하라.

| 에러 매칭 필요? | 에러 메시지 | 권장 방법 |
|---|---|---|
| No | static | [`errors.New`](https://pkg.go.dev/errors#New) |
| No | dynamic | [`fmt.Errorf`](https://pkg.go.dev/fmt#Errorf) |
| Yes | static | 최상위 `var` + [`errors.New`](https://pkg.go.dev/errors#New) |
| Yes | dynamic | 커스텀 `error` 타입 |

예를 들어, 정적 문자열 에러에는 [`errors.New`](https://pkg.go.dev/errors#New)를 사용하라.
호출자가 `errors.Is`로 매칭하여 처리해야 한다면 변수로 내보내라.

<table>
<thead><tr><th>에러 매칭 불필요</th><th>에러 매칭 필요</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open() error {
  return errors.New("could not open")
}

// package bar

if err := foo.Open(); err != nil {
  // 에러를 처리할 수 없다.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

var ErrCouldNotOpen = errors.New("could not open")

func Open() error {
  return ErrCouldNotOpen
}

// package bar

if err := foo.Open(); err != nil {
  if errors.Is(err, foo.ErrCouldNotOpen) {
    // 에러를 처리한다.
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

동적 문자열 에러의 경우, 호출자가 매칭할 필요가 없으면 [`fmt.Errorf`](https://pkg.go.dev/fmt#Errorf)를,
매칭이 필요하면 커스텀 `error` 타입을 사용하라.

<table>
<thead><tr><th>에러 매칭 불필요</th><th>에러 매칭 필요</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open(file string) error {
  return fmt.Errorf("file %q not found", file)
}

// package bar

if err := foo.Open("testfile.txt"); err != nil {
  // 에러를 처리할 수 없다.
  panic("unknown error")
}
```

</td><td>

```go
// package foo

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

func Open(file string) error {
  return &NotFoundError{File: file}
}

// package bar

if err := foo.Open("testfile.txt"); err != nil {
  var notFound *NotFoundError
  if errors.As(err, &notFound) {
    // 에러를 처리한다.
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

패키지에서 에러 변수나 타입을 내보내는 경우,
해당 타입이 패키지의 공개 API 일부가 됨에 유의하라.

<!-- TODO: Exposing the information to callers with accessor functions. -->

#### 오류 래핑(Error Wrapping)

호출이 실패할 경우 에러를 전파하는 3가지 주요 옵션이 있다:

- 추가 컨텍스트 없이 원본 에러를 그대로 반환
- `fmt.Errorf`와 `%w` 동사로 컨텍스트 추가
- `fmt.Errorf`와 `%v` 동사로 컨텍스트 추가

추가 컨텍스트가 없다면 원본 에러를 그대로 반환하라.
이렇게 하면 원본 에러 타입과 메시지가 유지된다.
에러 메시지가 출처를 충분히 추적할 수 있는 경우에 적합하다.

그렇지 않다면 가능한 경우 컨텍스트를 추가하라.
"connection refused"와 같은 모호한 에러 대신
"call service foo: connection refused"처럼 더 유용한 에러를 얻을 수 있다.

`fmt.Errorf`로 컨텍스트를 추가할 때는 호출자가 내부 원인을 매칭·추출할 수 있어야 하는지에 따라
`%w` 또는 `%v`를 선택하라.

- 호출자가 내부 에러에 접근해야 한다면 `%w`를 사용하라.
  대부분의 래핑된 에러에 권장하는 기본값이다.
  단, 호출자가 이 동작에 의존하기 시작할 수 있으니,
  래핑된 에러가 알려진 `var`나 타입인 경우에는 함수 계약(contract)의 일부로 문서화하고 테스트하라.
- 호출자가 내부 에러를 매칭하지 못하게 하려면 `%v`를 사용하라.
  필요 시 나중에 `%w`로 전환할 수 있다.

반환된 에러에 컨텍스트를 추가할 때는 간결하게 유지하라. "failed to"와 같이 당연한 말을 반복하는 문구는 에러가 스택을 타고 올라가면서 계속 쌓이게 된다:

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "failed to create new store: %w", err)
}
```

</td><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "new store: %w", err)
}
```

<tr><td>

```
failed to x: failed to y: failed to create new store: the error
```

</td><td>

```
x: y: new store: the error
```

</td></tr>
</tbody></table>

그러나, 일단 오류가 다른 시스템으로 전송되면, 그 메시지가 오류임은 분명히 해야 한다. (예를들어 `err` 태그(tag) 혹은 로그에서의 "Failed" 접두사 사용)

또한 다음의 글을 참고하라: [Don't just check errors, handle them gracefully].


  [Don't just check errors, handle them gracefully]: https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully

#### 에러 이름 짓기 (Error Naming)

전역 변수(global variables)로 저장되는 에러 값에는, 내보내기(export) 여부에 따라 `Err` 또는 `err` 접두사를 사용하라.
이 가이드라인은 [수출되지 않은 전역에 _을 붙여라](#수출되지-않은-전역에-_을-붙여라-prefix-unexported-globals-with-_) 규칙보다 우선한다.

```go
var (
  // 다음 두 에러는 내보내기(export)되어,
  // 이 패키지의 사용자가 errors.Is로 매칭할 수 있다.

  ErrBrokenLink = errors.New("link is broken")
  ErrCouldNotOpen = errors.New("could not open")

  // 이 에러는 내보내지 않는다(not exported).
  // public API의 일부로 만들고 싶지 않기 때문이다.
  // 하지만 패키지 내부에서 errors.Is로 사용할 수 있다.

  errNotFound = errors.New("not found")
)
```

커스텀 에러 타입의 경우, 대신 `Error` 접미사를 사용하라.

```go
// 마찬가지로 이 에러는 내보내기(export)되어,
// 이 패키지의 사용자가 errors.As로 매칭할 수 있다.

type NotFoundError struct {
  File string
}

func (e *NotFoundError) Error() string {
  return fmt.Sprintf("file %q not found", e.File)
}

// 이 에러는 내보내지 않는다(not exported).
// public API의 일부로 만들고 싶지 않기 때문이다.
// 하지만 패키지 내부에서 errors.As로 사용할 수 있다.

type resolveError struct {
  Path string
}

func (e *resolveError) Error() string {
  return fmt.Sprintf("resolve %q", e.Path)
}
```

#### 에러를 한 번만 처리하라 (Handle Errors Once)

호출자(caller)가 피호출자(callee)로부터 에러를 받았을 때, 에러에 대해 알고 있는 내용에 따라 다양한 방식으로 처리할 수 있다.

가능한 처리 방식은 다음과 같지만, 이에 국한되지 않는다:

- 피호출자의 계약이 특정 에러를 정의한다면, `errors.Is` 또는 `errors.As`로 에러를 매칭하여 각 분기를 다르게 처리한다.
- 에러에서 회복(recover)할 수 있다면, 에러를 로그로 남기고 우아하게(gracefully) 성능을 낮춘다.
- 에러가 도메인 특정 실패 조건(domain-specific failure condition)을 나타낸다면, 잘 정의된 에러를 반환한다.
- 에러를 [래핑(wrapped)](#오류-래핑error-wrapping)하거나 그대로 반환한다.

호출자는 에러를 어떻게 처리하든, 일반적으로 각 에러를 **한 번만** 처리해야 한다.
예를 들어, 에러를 로그로 남긴 후 반환하는 것은 좋지 않다. *상위 호출자*도 에러를 처리할 수 있기 때문이다.

예시를 통해 살펴보자:

<table>
<thead><tr><th>설명</th><th>코드</th></tr></thead>
<tbody>
<tr><td>

**Bad**: 에러를 로그로 남기고 반환한다

스택 상위의 호출자도 에러에 대해 유사한 동작을 할 가능성이 높다.
이렇게 하면 애플리케이션 로그에 불필요한 노이즈가 쌓인다.

</td><td>

```go
u, err := getUser(id)
if err != nil {
  // 나쁜 예: 위 설명 참고
  log.Printf("Could not get user %q: %v", id, err)
  return err
}
```

</td></tr>
<tr><td>

**Good**: 에러를 래핑하고 반환한다

스택 상위의 호출자가 에러를 처리한다.
`%w`를 사용하면 관련 있는 경우 `errors.Is` 또는 `errors.As`로 에러를 매칭할 수 있다.

</td><td>

```go
u, err := getUser(id)
if err != nil {
  return fmt.Errorf("get user %q: %w", id, err)
}
```

</td></tr>
<tr><td>

**Good**: 에러를 로그로 남기고 우아하게 성능을 낮춘다

작업이 엄격히 필요하지 않다면, 에러에서 회복하여 기능이 저하되지만 정상적으로 동작하는 경험을 제공할 수 있다.

</td><td>

```go
if err := emitMetrics(); err != nil {
  // 메트릭 쓰기 실패가 애플리케이션을 중단시켜서는 안 된다.
  log.Printf("Could not emit metrics: %v", err)
}

```

</td></tr>
<tr><td>

**Good**: 에러를 매칭하고 우아하게 성능을 낮춘다

피호출자가 계약에서 특정 에러를 정의하고, 실패에서 회복할 수 있다면, 해당 에러 케이스를 매칭하여 우아하게 성능을 낮춘다.
그 외의 경우, 에러를 래핑하여 반환한다.

스택 상위의 호출자가 그 외의 에러를 처리한다.

</td><td>

```go
tz, err := getUserTimeZone(id)
if err != nil {
  if errors.Is(err, ErrUserNotFound) {
    // 사용자가 존재하지 않는다. UTC를 사용한다.
    tz = time.UTC
  } else {
    return fmt.Errorf("get user %q: %w", id, err)
  }
}
```

</td></tr>
</tbody></table>

### 타입의 어설션 실패 다루기 (Handle Type Assertion Failures)

[type assertion]의 단일 반환 값 형식(the single return value form)은 잘못된 타입에 패닉 상태가 된다. 따라서 항상 "comma ok" 관용구(idiom)을 사용하는 것을 권장한다.

  [type assertion]: https://go.dev/ref/spec#Type_assertions

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
t := i.(string)
```

</td><td>

```go
t, ok := i.(string)
if !ok {
  // 에러를 적절히 처리한다
}
```

</td></tr>
</tbody></table>

<!-- TODO: There are a few situations where the single assignment form is
fine. -->

### 패닉을 피할 것 (Don't Panic)

프로덕션 환경에서 실행되는 코드는 패닉을 반드시 피해야 한다. 패닉은 [cascading failures]의 주요 원인이다. 만약 에러가 발생할 경우, 함수는 에러를 리턴하고 호출자(caller)가 오류 처리 방법을 결정할 수 있도록 해야 한다.

  [cascading failures]: https://en.wikipedia.org/wiki/Cascading_failure

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func run(args []string) {
  if len(args) == 0 {
    panic("an argument is required")
  }
  // ...
}

func main() {
  run(os.Args[1:])
}
```

</td><td>

```go
func run(args []string) error {
  if len(args) == 0 {
    return errors.New("an argument is required")
  }
  // ...
  return nil
}

func main() {
  if err := run(os.Args[1:]); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

</td></tr>
</tbody></table>

Panic/recover는 오류 처리 전략이 아니다. nil 역참조(nil dereference)와 같이 복구 불가능한 상황에서만 패닉이 발생해야 한다. 예외는 프로그램 초기화 시다: 프로그램 시작 시 프로그램을 중단시켜야 할 심각한 문제가 발생하면 패닉을 일으킬 수 있다.

```go
var _statusTemplate = template.Must(template.New("name").Parse("_statusHTML"))
```

테스트에서도, 테스트 실패를 올바르게 표기하기 위해 `panic`보다는 `t.Fatal` 혹은 `t.FailNow`를 사용하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  panic("failed to set up test")
}
```

</td><td>

```go
// func TestFoo(t *testing.T)

f, err := os.CreateTemp("", "test")
if err != nil {
  t.Fatal("failed to set up test")
}
```

</td></tr>
</tbody></table>

<!-- TODO: Explain how to use _test packages. -->

### go.uber.org/atomic의 사용

[sync/atomic] 패키지를 사용한 아토믹 연산(atomic operation)은 원시 타입 (raw type: e.g. `int32`, `int64`, etc.)에서 작동하므로, 아토믹 연산을 사용하여 변수를 읽거나 수정하는 것을 쉽게 잊어버릴 수 있다.

[go.uber.org/atomic]는 기본 타입(underlying type)을 숨겨서 이런 유형의 연산에 타입 안전성을 부여한다(add type safety). 또한, 이는 간편한 `atomic.Bool` 타입을 포함하고 있다.

  [go.uber.org/atomic]: https://pkg.go.dev/go.uber.org/atomic
  [sync/atomic]: https://pkg.go.dev/sync/atomic

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type foo struct {
  running int32  // atomic
}

func (f* foo) start() {
  if atomic.SwapInt32(&f.running, 1) == 1 {
     // already running…
     return
  }
  // start the Foo
}

func (f *foo) isRunning() bool {
  return f.running == 1  // race!
}
```

</td><td>

```go
type foo struct {
  running atomic.Bool
}

func (f *foo) start() {
  if f.running.Swap(true) {
     // already running…
     return
  }
  // start the Foo
}

func (f *foo) isRunning() bool {
  return f.running.Load()
}
```

</td></tr>
</tbody></table>

### 변경 가능한(mutable) 전역변수 피하기 (Avoid Mutable Globals)

전역 변수를 변경하는 것을 피하고, 대신 의존성 주입(dependency injection)을 선택하라. 이는 함수 포인터뿐만 아니라 다른 종류의 값에도 적용된다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// sign.go

var _timeNow = time.Now

func sign(msg string) string {
  now := _timeNow()
  return signWithTime(msg, now)
}
```

</td><td>

```go
// sign.go

type signer struct {
  now func() time.Time
}

func newSigner() *signer {
  return &signer{
    now: time.Now,
  }
}

func (s *signer) Sign(msg string) string {
  now := s.now()
  return signWithTime(msg, now)
}
```

</td></tr>
<tr><td>

```go
// sign_test.go

func TestSign(t *testing.T) {
  oldTimeNow := _timeNow
  _timeNow = func() time.Time {
    return someFixedTime
  }
  defer func() { _timeNow = oldTimeNow }()

  assert.Equal(t, want, sign(give))
}
```

</td><td>

```go
// sign_test.go

func TestSigner(t *testing.T) {
  s := newSigner()
  s.now = func() time.Time {
    return someFixedTime
  }

  assert.Equal(t, want, s.Sign(give))
}
```

</td></tr>
</tbody></table>

### 공개 구조체(public struct)에서 내장 타입들(Embedding Types) 사용하지 않기 (Avoid Embedding Types in Public Structs)

내장된 타입(embedded type)은 구현 세부사항을 노출하고, 타입 확장을 방해하며, 문서를 불명확하게 만든다.

공유되는 `AbstractList`를 사용하여 다양한 리스트 타입을 구현했다고 가정할 때, 구체적인 리스트 구현체에 `AbstractList`를 내장하는 것을 피하라. 대신, 추상 리스트에 위임(delegate)하는 메서드만 직접 작성하라.

```go
type AbstractList struct {}

// Add adds an entity to the list.
func (l *AbstractList) Add(e Entity) {
  // ...
}

// Remove removes an entity from the list.
func (l *AbstractList) Remove(e Entity) {
  // ...
}
```

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// ConcreteList is a list of entities.
type ConcreteList struct {
  *AbstractList
}
```

</td><td>

```go
// ConcreteList is a list of entities.
type ConcreteList struct {
  list *AbstractList
}

// Add adds an entity to the list.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove removes an entity from the list.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

Go는 상속과 컴포지션 사이의 절충안으로 [타입 내장(type embedding)](https://go.dev/doc/effective_go#embedding)을 허용한다. 외부 타입은 내장된 타입의 메서드를 암묵적으로 복사한다. 이 메서드들은 기본적으로 내장된 인스턴스의 동일한 메서드에 위임한다.

구조체는 타입과 동일한 이름의 필드도 갖게 된다. 따라서 내장된 타입이 공개(public)이면 필드도 공개된다. 하위 호환성을 유지하기 위해 외부 타입의 모든 이후 버전은 내장된 타입을 유지해야 한다.

내장 타입이 필요한 경우는 드물다. 번거로운 위임 메서드 작성을 피하기 위한 편의 기능이다.

호환 가능한 AbstractList *인터페이스*를 구조체 대신 내장하더라도, 개발자에게 미래에 더 많은 유연성을 제공하지만, 구체적인 리스트가 추상적 구현을 사용한다는 세부사항은 여전히 노출된다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// AbstractList is a generalized implementation
// for various kinds of lists of entities.
type AbstractList interface {
  Add(Entity)
  Remove(Entity)
}

// ConcreteList is a list of entities.
type ConcreteList struct {
  AbstractList
}
```

</td><td>

```go
// ConcreteList is a list of entities.
type ConcreteList struct {
  list AbstractList
}

// Add adds an entity to the list.
func (l *ConcreteList) Add(e Entity) {
  l.list.Add(e)
}

// Remove removes an entity from the list.
func (l *ConcreteList) Remove(e Entity) {
  l.list.Remove(e)
}
```

</td></tr>
</tbody></table>

구조체를 내장하든 인터페이스를 내장하든, 내장된 타입은 해당 타입의 발전에 제약을 가한다.

- 내장된 인터페이스에 메서드를 추가하는 것은 호환성을 깨뜨리는 변경이다.
- 내장된 구조체에서 메서드를 제거하는 것은 호환성을 깨뜨리는 변경이다.
- 내장된 타입을 제거하는 것은 호환성을 깨뜨리는 변경이다.
- 동일한 인터페이스를 만족하는 대안으로 내장된 타입을 교체하는 것도 호환성을 깨뜨리는 변경이다.

위임 메서드를 작성하는 것이 번거롭더라도, 추가적인 노력은 구현 세부사항을 숨기고, 변경 가능성을 더 많이 남기며, 문서에서 전체 List 인터페이스를 찾기 위한 간접 참조도 제거한다.

### 내장된(built-in) 이름 사용을 피하라 (Avoid Using Built-In Names)

Go [언어 명세](https://go.dev/ref/spec)에는 Go 프로그램 내에서 이름으로 사용해서는 안 되는 여러 내장 [사전 선언 식별자(predeclared identifiers)](https://go.dev/ref/spec#Predeclared_identifiers)가 나열되어 있다.

문맥에 따라 이러한 식별자를 이름으로 재사용하면, 현재 어휘 범위(및 중첩된 범위) 내에서 원래 식별자를 가리거나(shadow), 영향 받는 코드를 혼란스럽게 만든다. 최선의 경우 컴파일러가 불평하고, 최악의 경우 이런 코드는 찾기 어려운 잠재적 버그를 도입할 수 있다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
var error string
// `error` shadows the builtin

// or

func handleErrorMessage(error string) {
    // `error` shadows the builtin
}
```

</td><td>

```go
var errorMessage string
// `error` refers to the builtin

// or

func handleErrorMessage(msg string) {
    // `error` refers to the builtin
}
```

</td></tr>
<tr><td>

```go
type Foo struct {
    // 이 필드들은 엄밀히 섀도잉은 아니지만,
    // error나 string을 grep하면
    // 결과가 모호해진다.
    // 
    error  error
    string string
}

func (f Foo) Error() error {
    // error와 f.error는
    // 시각적으로 유사하다
    return f.error
}

func (f Foo) String() string {
    // string과 f.string은
    // 시각적으로 유사하다
    return f.string
}
```

</td><td>

```go
type Foo struct {
    // `error` and `string` strings are
    // now unambiguous.
    err error
    str string
}

func (f Foo) Error() error {
    return f.err
}

func (f Foo) String() string {
    return f.str
}
```

</td></tr>
</tbody></table>

사전 선언 식별자를 사용해도 컴파일러가 오류를 발생시키지 않지만, `go vet`과 같은 도구는 이러한 섀도잉 케이스를 올바르게 지적해야 한다.

### `init()` 사용을 피하라 (Avoid `init()`)

가능하면 `init()`을 피하라. `init()`이 불가피하거나 바람직한 경우, 코드는 다음을 시도해야 한다:

1. 프로그램 환경이나 호출 방식에 관계없이 완전히 결정적(deterministic)이어야 한다.
2. 다른 `init()` 함수의 순서나 부작용(side-effect)에 의존하지 않아야 한다. `init()` 순서는 잘 알려져 있지만, 코드는 변경될 수 있으므로 `init()` 함수 간의 관계는 코드를 취약하고 오류에 취약하게 만들 수 있다.
3. 머신 정보, 환경 변수, 작업 디렉토리, 프로그램 인수/입력 등 전역 또는 환경 상태에 접근하거나 조작하지 않아야 한다.
4. 파일시스템, 네트워크, 시스템 콜을 포함한 I/O를 피해야 한다.

이러한 요구사항을 만족할 수 없는 코드는 `main()`의 일부로 호출되는 헬퍼(또는 프로그램 생명주기의 다른 곳)로 만들거나, `main()` 자체의 일부로 작성되어야 한다. 특히, 다른 프로그램에서 사용하도록 의도된 라이브러리는 완전히 결정적이고 "init 마법"을 수행하지 않도록 특별히 주의해야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Foo struct {
    // ...
}

var _defaultFoo Foo

func init() {
    _defaultFoo = Foo{
        // ...
    }
}
```

</td><td>

```go
var _defaultFoo = Foo{
    // ...
}

// 또는 테스트 용이성을 위해 더 나은 방법:

var _defaultFoo = defaultFoo()

func defaultFoo() Foo {
    return Foo{
        // ...
    }
}
```

</td></tr>
<tr><td>

```go
type Config struct {
    // ...
}

var _config Config

func init() {
    // Bad: based on current directory
    cwd, _ := os.Getwd()

    // Bad: I/O
    raw, _ := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )

    yaml.Unmarshal(raw, &_config)
}
```

</td><td>

```go
type Config struct {
    // ...
}

func loadConfig() Config {
    cwd, err := os.Getwd()
    // handle err

    raw, err := os.ReadFile(
        path.Join(cwd, "config", "config.yaml"),
    )
    // handle err

    var config Config
    yaml.Unmarshal(raw, &config)

    return config
}
```

</td></tr>
</tbody></table>

위 내용을 고려했을 때, `init()`이 바람직하거나 필요할 수 있는 상황은 다음과 같다:

- 단일 할당으로 표현할 수 없는 복잡한 표현식.
- `database/sql` 방언, 인코딩 타입 레지스트리 등의 플러그인 가능한 훅(pluggable hook).
- [Google Cloud Functions](https://cloud.google.com/functions/docs/bestpractices/tips#use_global_variables_to_reuse_objects_in_future_invocations) 및 기타 형태의 결정적 사전 연산(precomputation) 최적화.

### Main에서 종료하기 (Exit in Main)

Go 프로그램은 즉시 종료하기 위해 [`os.Exit`](https://pkg.go.dev/os#Exit) 또는 [`log.Fatal*`](https://pkg.go.dev/log#Fatal)을 사용한다. (패닉은 프로그램을 종료하는 좋은 방법이 아니다. [패닉을 피할 것](#패닉을-피할-것-dont-panic)을 참고하라.)

`os.Exit` 또는 `log.Fatal*` 중 하나를 **`main()`에서만** 호출하라. 다른 모든 함수는 실패를 알리기 위해 에러를 반환해야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func main() {
  body := readFile(path)
  fmt.Println(body)
}

func readFile(path string) string {
  f, err := os.Open(path)
  if err != nil {
    log.Fatal(err)
  }

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  return string(b)
}
```

</td><td>

```go
func main() {
  body, err := readFile(path)
  if err != nil {
    log.Fatal(err)
  }
  fmt.Println(body)
}

func readFile(path string) (string, error) {
  f, err := os.Open(path)
  if err != nil {
    return "", err
  }

  b, err := io.ReadAll(f)
  if err != nil {
    return "", err
  }

  return string(b), nil
}
```

</td></tr>
</tbody></table>

여러 함수에서 종료를 수행하는 프로그램은 몇 가지 문제를 일으킨다:

- 명확하지 않은 제어 흐름: 어떤 함수든 프로그램을 종료할 수 있으므로 제어 흐름을 파악하기 어렵다.
- 테스트하기 어려움: 프로그램을 종료하는 함수는 해당 함수를 호출하는 테스트도 종료한다. 이로 인해 함수 테스트가 어려워지고 `go test`로 아직 실행되지 않은 다른 테스트를 건너뛸 위험이 생긴다.
- 클린업(cleanup) 생략: 함수가 프로그램을 종료하면, `defer` 문으로 등록된 함수 호출이 생략된다. 이는 중요한 클린업 작업을 건너뛸 위험을 초래한다.

#### 한 번만 종료하라 (Exit Once)

가능하다면, `main()`에서 `os.Exit` 또는 `log.Fatal`을 **최대 한 번만** 호출하라. 프로그램 실행을 중단하는 여러 에러 시나리오가 있다면, 해당 로직을 별도의 함수로 분리하고 에러를 반환하라.

이를 통해 `main()` 함수를 짧게 유지하고, 핵심 비즈니스 로직을 별도의 테스트 가능한 함수에 담을 수 있다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
package main

func main() {
  args := os.Args[1:]
  if len(args) != 1 {
    log.Fatal("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    log.Fatal(err)
  }
  defer f.Close()

  // 이 줄 이후에 log.Fatal을 호출하면,
  // f.Close는 호출되지 않는다.

  b, err := io.ReadAll(f)
  if err != nil {
    log.Fatal(err)
  }

  // ...
}
```

</td><td>

```go
package main

func main() {
  if err := run(); err != nil {
    log.Fatal(err)
  }
}

func run() error {
  args := os.Args[1:]
  if len(args) != 1 {
    return errors.New("missing file")
  }
  name := args[0]

  f, err := os.Open(name)
  if err != nil {
    return err
  }
  defer f.Close()

  b, err := io.ReadAll(f)
  if err != nil {
    return err
  }

  // ...
}
```

</td></tr>
</tbody></table>

위 예시는 `log.Fatal`을 사용했지만, 이 가이드라인은 `os.Exit` 또는 `os.Exit`를 호출하는 모든 라이브러리 코드에도 적용된다.

```go
func main() {
  if err := run(); err != nil {
    fmt.Fprintln(os.Stderr, err)
    os.Exit(1)
  }
}
```

필요에 따라 `run()`의 시그니처를 변경할 수 있다. 예를 들어, 프로그램이 실패 시 특정 종료 코드로 종료해야 한다면, `run()`이 에러 대신 종료 코드를 반환할 수 있다. 이를 통해 유닛 테스트에서도 이 동작을 직접 검증할 수 있다.

```go
func main() {
  os.Exit(run(args))
}

func run() (exitCode int) {
  // ...
}
```

더 일반적으로, 이 예시에서 사용된 `run()` 함수는 특정 방식을 강제하지 않는다. `run()` 함수의 이름, 시그니처, 설정에는 유연성이 있다. 다음과 같이 할 수도 있다:

- 파싱되지 않은 커맨드라인 인수를 받는다 (예: `run(os.Args[1:])`)
- `main()`에서 커맨드라인 인수를 파싱하여 `run`에 전달한다
- 커스텀 에러 타입을 사용하여 `main()`에 종료 코드를 전달한다
- `package main`과 다른 추상화 계층에 비즈니스 로직을 둔다

이 가이드라인은 `main()`에서 실제로 프로세스를 종료하는 단일한 위치가 있어야 함을 요구한다.

### 직렬화되는 구조체에 필드 태그를 사용하라 (Use field tags in marshaled structs)

JSON, YAML 또는 태그 기반 필드 이름을 지원하는 기타 포맷으로 마샬링(marshaled)되는 구조체 필드에는 관련 태그를 명시해야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Stock struct {
  Price int
  Name  string
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td><td>

```go
type Stock struct {
  Price int    `json:"price"`
  Name  string `json:"name"`
  // Name을 Symbol로 안전하게 변경할 수 있다.
}

bytes, err := json.Marshal(Stock{
  Price: 137,
  Name:  "UBER",
})
```

</td></tr>
</tbody></table>

이유: 구조체의 직렬화된 형태는 서로 다른 시스템 간의 계약(contract)이다. 필드 이름을 포함한 직렬화된 형태의 구조 변경은 이 계약을 깨뜨린다. 태그 안에 필드 이름을 명시하면 계약을 명확하게 만들고, 리팩토링이나 필드 이름 변경으로 인해 실수로 계약을 깨뜨리는 것을 방지할 수 있다.

### Goroutine을 Fire-and-Forget 방식으로 실행하지 마라 (Don't fire-and-forget goroutines)

Goroutine은 경량이지만 비용이 없는 것은 아니다: 최소한 스택을 위한 메모리와 스케줄링을 위한 CPU가 필요하다. 일반적인 goroutine 사용에서는 이 비용이 작지만, 수명이 제어되지 않는 상태로 대량으로 생성되면 심각한 성능 문제를 일으킬 수 있다. 수명이 관리되지 않는 goroutine은 미사용 객체의 가비지 컬렉션을 방해하거나 더 이상 사용되지 않는 리소스를 계속 보유하는 등의 문제를 야기할 수도 있다.

따라서, 프로덕션 코드에서 goroutine을 누수(leak)시키지 마라. goroutine을 생성할 수 있는 패키지 내의 goroutine 누수를 테스트하려면 [go.uber.org/goleak](https://pkg.go.dev/go.uber.org/goleak)을 사용하라.

일반적으로, 모든 goroutine은 다음 중 하나를 만족해야 한다:

- 실행을 멈출 예측 가능한 시점이 있어야 한다. 또는
- goroutine에 멈추라는 신호를 보낼 방법이 있어야 한다.

두 경우 모두, goroutine이 종료될 때까지 코드가 블록하여 대기할 방법이 있어야 한다.

예시:

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
go func() {
  for {
    flush()
    time.Sleep(delay)
  }
}()
```

</td><td>

```go
var (
  stop = make(chan struct{}) // goroutine에 멈추라고 알린다
  done = make(chan struct{}) // goroutine이 종료되었음을 알린다
)
go func() {
  defer close(done)

  ticker := time.NewTicker(delay)
  defer ticker.Stop()
  for {
    select {
    case <-ticker.C:
      flush()
    case <-stop:
      return
    }
  }
}()

// 다른 곳에서...
close(stop)  // goroutine에 멈추라고 신호를 보낸다
<-done       // 종료될 때까지 대기한다
```

</td></tr>
<tr><td>

이 goroutine을 멈출 방법이 없다.
애플리케이션이 종료될 때까지 계속 실행된다.

</td><td>

이 goroutine은 `close(stop)`으로 멈출 수 있고,
`<-done`으로 종료를 대기할 수 있다.

</td></tr>
</tbody></table>

#### Goroutine이 종료될 때까지 대기하라 (Wait for goroutines to exit)

시스템에 의해 생성된 goroutine의 경우, goroutine이 종료될 때까지 대기할 방법이 있어야 한다. 이를 위한 두 가지 일반적인 방법이 있다:

- 여러 goroutine의 완료를 대기하려면 `sync.WaitGroup`을 사용하라.

  ```go
  var wg sync.WaitGroup
  for i := 0; i < N; i++ {
    wg.Go(...)
  }

  // 모두 완료될 때까지 대기하려면:
  wg.Wait()
  ```

- goroutine이 하나뿐이라면, goroutine이 완료될 때 닫히는 `chan struct{}`를 추가하라.

  ```go
  done := make(chan struct{})
  go func() {
    defer close(done)
    // ...
  }()

  // goroutine이 종료될 때까지 대기하려면:
  <-done
  ```

#### `init()`에서 goroutine을 사용하지 마라 (No goroutines in `init()`)

`init()` 함수는 goroutine을 생성해서는 안 된다. [`init()` 사용을 피하라](#init-사용을-피하라-avoid-init)도 참고하라.

패키지에 백그라운드 goroutine이 필요하다면, goroutine의 수명을 관리하는 객체를 노출해야 한다. 해당 객체는 백그라운드 goroutine에 멈추라는 신호를 보내고 종료를 대기하는 메서드(`Close`, `Stop`, `Shutdown` 등)를 제공해야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func init() {
  go doWork()
}

func doWork() {
  for {
    // ...
  }
}
```

</td><td>

```go
type Worker struct{ /* ... */ }

func NewWorker(...) *Worker {
  w := &Worker{
    stop: make(chan struct{}),
    done: make(chan struct{}),
    // ...
  }
  go w.doWork()
  return w
}

func (w *Worker) doWork() {
  defer close(w.done)
  for {
    // ...
    case <-w.stop:
      return
  }
}

// Shutdown은 worker에 멈추라고 알리고
// 완료될 때까지 대기한다.
func (w *Worker) Shutdown() {
  close(w.stop)
  <-w.done
}
```

</td></tr>
<tr><td>

사용자가 이 패키지를 임포트할 때 무조건 백그라운드 goroutine을 생성한다.
사용자는 goroutine을 제어하거나 멈출 방법이 없다.

</td><td>

사용자가 요청한 경우에만 worker를 생성한다.
사용자가 worker가 사용하는 리소스를 해제할 수 있도록 worker를 종료하는 방법을 제공한다.

worker가 여러 goroutine을 관리한다면 `WaitGroup`을 사용해야 함에 유의하라.
[Goroutine이 종료될 때까지 대기하라](#goroutine이-종료될-때까지-대기하라-wait-for-goroutines-to-exit)를 참고하라.

</td></tr>
</tbody></table>

## 성능(Performance)

성능 관련 가이드라인은 성능이 중요한 경로(hot path)에서만 적용된다.

### `fmt` 보다 `strconv` 선호

원시 타입(primitive)을 문자열로 변환하거나 문자열에서 변환할 때, `strconv`가 `fmt`보다 빠르다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  s := fmt.Sprint(rand.Int())
}
```

</td><td>

```go
for i := 0; i < b.N; i++ {
  s := strconv.Itoa(rand.Int())
}
```

</td></tr>
<tr><td>

```
BenchmarkFmtSprint-4    143 ns/op    2 allocs/op
```

</td><td>

```
BenchmarkStrconv-4    64.2 ns/op    1 allocs/op
```

</td></tr>
</tbody></table>

### string-to-byte 변환을 피해라 (Avoid repeated string-to-byte conversions)

고정 문자열에서 바이트 슬라이스를 반복해서 생성하지 마라. 대신 변환을 한 번만 수행하고 결과를 저장해서 재사용하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
for i := 0; i < b.N; i++ {
  w.Write([]byte("Hello world"))
}
```

</td><td>

```go
data := []byte("Hello world")
for i := 0; i < b.N; i++ {
  w.Write(data)
}
```

</tr>
<tr><td>

```
BenchmarkBad-4   50000000   22.2 ns/op
```

</td><td>

```
BenchmarkGood-4  500000000   3.25 ns/op
```

</td></tr>
</tbody></table>

### 컨테이너 용량 지정을 선호하라 (Prefer Specifying Container Capacity)

가능하면 컨테이너 용량을 미리 지정하여 메모리를 미리 할당하라. 이를 통해 요소가 추가될 때 발생하는 후속 할당(컨테이너 복사 및 크기 조정)을 최소화할 수 있다.

#### Map 용량 힌트 지정 (Specifying Map Capacity Hints)

가능하면 `make()`로 map을 초기화할 때 용량 힌트를 제공하라.

```go
make(map[T1]T2, hint)
```

`make()`에 용량 힌트를 제공하면 초기화 시점에 map의 크기를 적절히 설정하려 시도하므로, map이 성장하고 요소가 추가될 때 발생하는 할당 필요성을 줄인다.

슬라이스와 달리, map 용량 힌트는 완전한 선제적 할당을 보장하지 않고, 필요한 hashmap 버킷 수를 근사화하는 데 사용된다. 따라서, map에 요소를 추가할 때 지정된 용량까지도 할당이 발생할 수 있다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
files, _ := os.ReadDir("./files")

m := make(map[string]os.DirEntry)
for _, f := range files {
    m[f.Name()] = f
}
```

</td><td>

```go

files, _ := os.ReadDir("./files")

m := make(map[string]os.DirEntry, len(files))
for _, f := range files {
    m[f.Name()] = f
}
```

</td></tr>
<tr><td>

`m`이 크기 힌트 없이 생성되어, 증가하면서 여러 번 할당이 발생한다.

</td><td>

`m`이 크기 힌트와 함께 생성되어, 할당 횟수가 더 적을 수 있다.

</td></tr>
</tbody></table>

#### 슬라이스 용량 지정 (Specifying Slice Capacity)

가능하면 `make()`로 슬라이스를 초기화할 때, 특히 append를 사용하는 경우 용량 힌트를 제공하라.

```go
make([]T, length, capacity)
```

map과 달리, 슬라이스 용량은 힌트가 아니다: 컴파일러는 `make()`에 제공된 용량만큼 메모리를 할당하므로, 슬라이스의 길이가 용량에 도달하기 전까지 후속 `append()` 작업에서 추가 할당이 발생하지 않는다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td><td>

```go
for n := 0; n < b.N; n++ {
  data := make([]int, 0, size)
  for k := 0; k < size; k++{
    data = append(data, k)
  }
}
```

</td></tr>
<tr><td>

```plain
BenchmarkBad-4    100000000    2.48s
```

</td><td>

```plain
BenchmarkGood-4   100000000    0.21s
```

</td></tr>
</tbody></table>

## 스타일 (Style)

### 지나치게 긴 줄을 피하라 (Avoid overly long lines)

코드를 읽는 사람이 가로로 스크롤하거나 머리를 너무 많이 돌려야 하는 코드 라인을 피하라.

소프트 라인 길이 제한으로 **99자**를 권장한다. 작성자는 이 한도에 도달하기 전에 줄 바꿈을 하는 것을 목표로 해야 하지만, 이는 엄격한 제한은 아니다. 코드가 이 한도를 초과하는 것도 허용된다.

### 일관성을 유지하라 (Be Consistent)

이 문서에서 제시하는 가이드라인 중 일부는 객관적으로 평가할 수 있지만, 일부는 상황에 따라 다르거나 주관적이다.

무엇보다도, **일관성을 유지하라**.

일관된 코드는 유지보수하기 쉽고, 이해하기 쉬우며, 인지적 부담이 적고, 새로운 관례가 등장하거나 버그 유형이 수정될 때 마이그레이션하거나 업데이트하기 쉽다.

반대로, 하나의 코드베이스 내에 여러 가지 서로 다르거나 충돌하는 스타일이 있으면 유지보수 부담, 불확실성, 인지 부조화가 발생하며, 이는 개발 속도 저하, 고통스러운 코드 리뷰, 버그로 직결될 수 있다.

이 가이드라인을 코드베이스에 적용할 때는, 패키지(또는 그 이상) 수준에서 변경하는 것을 권장한다. 서브패키지 수준에서 적용하면 동일한 코드에 여러 스타일이 도입되어 위의 문제가 발생한다.

### 그룹 유사 선언 (Group Similar Declarations)

Go는 유사한 선언 그룹화를 지원한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
import "a"
import "b"
```

</td><td>

```go
import (
  "a"
  "b"
)
```

</td></tr>
</tbody></table>

이는 또한 상수, 변수, 그리고 타입 선언에서도 유효하다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go

const a = 1
const b = 2



var a = 1
var b = 2



type Area float64
type Volume float64
```

</td><td>

```go
const (
  a = 1
  b = 2
)

var (
  a = 1
  b = 2
)

type (
  Area float64
  Volume float64
)
```

</td></tr>
</tbody></table>

관련된 선언만 그룹화하라. 관련 없는 선언은 별도로 선언하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
  EnvVar = "MY_ENV"
)
```

</td><td>

```go
type Operation int

const (
  Add Operation = iota + 1
  Subtract
  Multiply
)

const EnvVar = "MY_ENV"
```

</td></tr>
</tbody></table>

그룹화는 최상위 레벨에서만 사용하는 것이 아니다. 예를 들어, 함수 내에서도 사용할 수 있다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func f() string {
  red := color.New(0xff0000)
  green := color.New(0x00ff00)
  blue := color.New(0x0000ff)

  // ...
}
```

</td><td>

```go
func f() string {
  var (
    red   = color.New(0xff0000)
    green = color.New(0x00ff00)
    blue  = color.New(0x0000ff)
  )

  // ...
}
```

</td></tr>
</tbody></table>

예외: 함수 내부에서 변수 선언은, 인접한 다른 변수들과 함께 선언되는 경우 묶어서 그룹화해야 한다. 서로 관련이 없더라도 함께 선언된 변수라면 이렇게 하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func (c *client) request() {
  caller := c.name
  format := "json"
  timeout := 5*time.Second
  var err error

  // ...
}
```

</td><td>

```go
func (c *client) request() {
  var (
    caller  = c.name
    format  = "json"
    timeout = 5*time.Second
    err error
  )

  // ...
}
```

</td></tr>
</tbody></table>

### Import 그룹 정리/배치 (Import Group Ordering)

2가지 import 그룹들이 존재한다:

- 표준 라이브러리 (Standard library)
- 그 외 모든 것 (Everything else)

이는 기본(default)으로 `goimports`에 의해서 적용되는 그룹들이다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"
  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td><td>

```go
import (
  "fmt"
  "os"

  "go.uber.org/atomic"
  "golang.org/x/sync/errgroup"
)
```

</td></tr>
</tbody></table>

### 패키지 이름 (Package Names)

패키지 이름을 정할 때, 아래와 같은 이름을 선택하라:

- 모두 알파벳 소문자 사용, 대문자와 언더스코어 (_)는 사용하지 말 것.
- 대부분의 호출 지점에서 named import로 이름을 바꿀 필요가 없다.
- 짧고 간결하게. 이름은 모든 호출 지점에서 사용됨을 기억하라.
- 복수형(plural) 사용 금지. 예를 들어, `net/urls` 가 아닌 `net/url`.
- "common", "util", "shared", 또는 "lib"의 용어 사용 금지. 정보가 없는 좋지 못한 이름임.

또한 [Package Names] 와 [Style guideline for Go packages]를 참고하기 바란다.

  [Package Names]: https://blog.golang.org/package-names
  [Style guideline for Go packages]: https://rakyll.org/style-packages/

### 함수 이름 (Function Names)

우리는 Go 커뮤니티의 [MixedCaps for function names]의 사용에 의한 컨벤션을 따른다. 테스트 함수(test functions)는 예외이다. 이는 관련 테스트케이스를 그룹화 할 목적으로 언더스코어(_)를 포함할 수 있다, 예를들어, `TestMyFunction_WhatIsBeingTested`.

  [MixedCaps for function names]: https://go.dev/doc/effective_go#mixed-caps

### Import 별칭 (Import Aliasing)

패키지 이름이 import path의 마지막 요소와 일치하지 않을 경우 별명을 사용해야 한다.

```go
import (
  "net/http"

  client "example.com/client-go"
  trace "example.com/trace/v2"
)
```

다른 모든 시나리오의 경우, import 별칭의 사용은 import하면서 두 import간 직접적 충돌(import direct conflict)이 발생하지 않는 한 지양해야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
import (
  "fmt"
  "os"


  nettrace "golang.net/x/trace"
)
```

</td><td>

```go
import (
  "fmt"
  "os"
  "runtime/trace"

  nettrace "golang.net/x/trace"
  runtimetrace "runtime/trace"
)
```

</td></tr>
</tbody></table>

### 함수 그룹화와 정렬/배치 (Function Grouping and Ordering)

- 함수는 대략적인 호출 순서에 따라 정렬해야 한다.
- 파일 내의 함수는 리시버별로 그룹화되어야 한다.

그러므로, 수출되는 함수 (exported function)는 파일 내의 `struct`, `const`, `var`의 정의 구문 이후의 시작 부분에 나타나야 한다.

`newXYZ()`/`NewXYZ()`가 타입이 정의된 뒷부분에 나타날 수 있지만, 이는 나머지 리시버(receiver)의 메서드들 전에 나타나야 한다 (may appear after the type is defined, but before the
rest of the methods on the receiver.)

함수들은 리시버에 의해 그룹화 되므로, 일반 유틸리티 함수들(plain utility functions)는 파일의 뒷부분에 나타나야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func (s *something) Cost() {
  return calcCost(s.weights)
}

type something struct{ ... }

func calcCost(n []int) int {...}

func (s *something) Stop() {...}

func newSomething() *something {
    return &something{}
}
```

</td><td>

```go
type something struct{ ... }

func newSomething() *something {
    return &something{}
}

func (s *something) Cost() {
  return calcCost(s.weights)
}

func (s *something) Stop() {...}

func calcCost(n []int) int {...}
```

</td></tr>
</tbody></table>

### 중첩 감소 (Reduce Nesting)

코드는 에러 케이스 혹은 특수 조건(error cases / special conditions)을 먼저 처리하고 루프를 일찍 리턴하거나 계속 지속함으로써 가능한 중첩(nesting)을 줄일 수 있어야 한다. 여러 레벨로 중첩된(nested multiple levels)코드의 양을 줄이도록 해라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
for _, v := range data {
  if v.F1 == 1 {
    v = process(v)
    if err := v.Call(); err == nil {
      v.Send()
    } else {
      return err
    }
  } else {
    log.Printf("Invalid v: %v", v)
  }
}
```

</td><td>

```go
for _, v := range data {
  if v.F1 != 1 {
    log.Printf("Invalid v: %v", v)
    continue
  }

  v = process(v)
  if err := v.Call(); err != nil {
    return err
  }
  v.Send()
}
```

</td></tr>
</tbody></table>

### 불필요한 else (Unnecessary Else)

변수가 if의 두 가지 분기문에 의해서 설정될 경우, 이는 단일 `if`문 (simple if)으로 대체 할 수 있다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
var a int
if b {
  a = 100
} else {
  a = 10
}
```

</td><td>

```go
a := 10
if b {
  a = 100
}
```

</td></tr>
</tbody></table>

### 최상위 변수 선언 (Top-level Variable Declarations)

최상위 레벨에서 (At the top level), 표준 `var` 키워드를 사용해라. 표현식과 타입이 다를 경우에만 타입을 명시하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
var _s string = F()

func F() string { return "A" }
```

</td><td>

```go
var _s = F()
// F는 이미 문자열을 반환한다고 명시하고 있기 때문에
// 타입을 다시 지정할 필요가 없다.

func F() string { return "A" }
```

</td></tr>
</tbody></table>

표현식의 타입이 원하는 타입과 정확하게 일치하지 않는 경우 타입을 지정해라.

```go
type myError struct{}

func (myError) Error() string { return "error" }

func F() myError { return myError{} }

var _e error = F()
// F는 myError 타입의 객체를 반환하지만, 우리가 원하는 것은 error
```

### 수출되지 않은 전역에 _을 붙여라 (Prefix Unexported Globals with _)

비공개(unexported) 최상위 `var`와 `const`에 접두사 `_`를 붙여 전역 심볼(global symbol)임을 명확히 하라.

예외: 비공개 에러 값(unexported error values)은 `err` 접두사를 사용해야 한다.

이유: 최상위 변수와 상수는 패키지 스코프(package scope)를 가진다. 일반적인 이름을 사용하면 다른 파일에서 잘못된 값을 실수로 사용하기 쉽다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// foo.go

const (
  defaultPort = 8080
  defaultUser = "user"
)

// bar.go

func Bar() {
  defaultPort := 9090
  ...
  fmt.Println("Default port", defaultPort)

  // 만약 Bar()의 첫번째 라인이 지워지면
  // 컴파일 에러에 직면하지 않는다.
}
```

</td><td>

```go
// foo.go

const (
  _defaultPort = 8080
  _defaultUser = "user"
)
```

</td></tr>
</tbody></table>

### 구조체에서의 임베딩 (Embedding in Structs)

임베드된 타입은 구조체의 필드 목록 최상단에 위치해야 하며, 임베드된 필드와 일반 필드를 구분하는 빈 줄이 있어야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Client struct {
  version int
  http.Client
}
```

</td><td>

```go
type Client struct {
  http.Client

  version int
}
```

</td></tr>
</tbody></table>

임베딩은 기능을 추가하거나 의미적으로 적절한 방식으로 기능을 증강하는 등 실질적인 이점을 제공해야 한다. 사용자에게 부정적인 영향을 미쳐서는 안 된다. ([공개 구조체에서 내장 타입들 사용하지 않기](#공개-구조체public-struct에서-내장-타입들embedding-types-사용하지-않기-avoid-embedding-types-in-public-structs) 참고)

예외: 뮤텍스는 비공개 타입에서도 임베드해서는 안 된다. ([제로 값 뮤텍스는 유효하다](#제로-값-뮤텍스zero-value-mutexes는-유효하다-zero-value-mutexes-are-valid) 참고)

임베딩은 다음과 같은 경우에 **해서는 안 된다**:

- 단순히 외관상의 이유나 편의를 위한 경우.
- 외부 타입의 생성 또는 사용을 더 어렵게 만드는 경우.
- 외부 타입의 제로 값에 영향을 주는 경우. 외부 타입에 유용한 제로 값이 있다면, 내부 타입을 임베드한 후에도 유용한 제로 값을 가져야 한다.
- 내부 타입을 임베드하는 부작용으로 관련 없는 함수나 필드를 외부 타입에 노출하는 경우.
- 비공개 타입을 노출하는 경우.
- 외부 타입의 복사 의미론(copy semantics)에 영향을 주는 경우.
- 외부 타입의 API나 타입 의미론을 변경하는 경우.
- 내부 타입의 비표준 형식을 임베드하는 경우.
- 외부 타입의 구현 세부사항을 노출하는 경우.
- 사용자가 타입 내부를 관찰하거나 제어할 수 있게 하는 경우.
- 래핑을 통해 내부 함수의 일반적인 동작을 변경하여 사용자를 합리적으로 놀라게 하는 경우.

간단히 말해, 의식적이고 의도적으로 임베드하라. 좋은 기준은 "이 내보내진 내부 메서드/필드 전부를 외부 타입에 직접 추가할 것인가"이다. 답이 "일부" 또는 "아니오"라면 내부 타입을 임베드하지 말고 필드로 사용하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type A struct {
    // 나쁜 예: A.Lock()과 A.Unlock()이
    //      외부에 노출되어 실익은 없고
    //      사용자가 A 내부를
    //      직접 제어할 수 있게 된다.
    //      
    sync.Mutex
}
```

</td><td>

```go
type countingWriteCloser struct {
    // 좋은 예: Write()는 특정 목적을 위해
    //       외부 계층에 제공되며,
    //       실제 작업은 내부 타입의
    //       Write()에 위임한다.
    io.WriteCloser

    count int
}

func (w *countingWriteCloser) Write(bs []byte) (int, error) {
    w.count += len(bs)
    return w.WriteCloser.Write(bs)
}
```

</td></tr>
<tr><td>

```go
type Book struct {
    // 나쁜 예: 포인터로 인해 제로 값의 유용성이 사라진다
    io.ReadWriter

    // other fields
}

// later

var b Book
b.Read(...)  // panic: nil pointer
b.String()   // panic: nil pointer
b.Write(...) // panic: nil pointer
```

</td><td>

```go
type Book struct {
    // 좋은 예: 유용한 제로 값을 가진다
    bytes.Buffer

    // other fields
}

// later

var b Book
b.Read(...)  // ok
b.String()   // ok
b.Write(...) // ok
```

</td></tr>
<tr><td>

```go
type Client struct {
    sync.Mutex
    sync.WaitGroup
    bytes.Buffer
    url.URL
}
```

</td><td>

```go
type Client struct {
    mtx sync.Mutex
    wg  sync.WaitGroup
    buf bytes.Buffer
    url url.URL
}
```

</td></tr>
</tbody></table>

### 구조체 초기화 (Initializing Structs)

#### 구조체 초기화를 위해 필드를 사용해라 (Use Field Names to initialize Structs)

구조체를 초기화 할 때에는 거의 대부분 필드 명을 지정해야 한다. 이것은 이제 [`go vet`]에 의해서 강제하고 있다.

  [`go vet`]: https://pkg.go.dev/cmd/vet

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
k := User{"John", "Doe", true}
```

</td><td>

```go
k := User{
    FirstName: "John",
    LastName: "Doe",
    Admin: true,
}
```

</td></tr>
</tbody></table>

예외: 테스트 테이블에서 필드명은 3개 일때 혹은 이보다 적을 때 생략될 수 있음.

```go
tests := []struct{
  op Operation
  want string
}{
  {Add, "add"},
  {Subtract, "subtract"},
}
```

#### 구조체의 제로 값 필드 생략 (Omit Zero Value Fields in Structs)

필드 이름으로 구조체를 초기화할 때, 의미있는 컨텍스트를 제공하지 않는 한 제로 값을 가진 필드는 생략하라. 그렇지 않으면 Go가 자동으로 해당 필드를 제로 값으로 설정하게 하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
  MiddleName: "",
  Admin: false,
}
```

</td><td>

```go
user := User{
  FirstName: "John",
  LastName: "Doe",
}
```

</td></tr>
</tbody></table>

해당 컨텍스트에서 기본값인 값을 생략하면 독자에게 불필요한 노이즈를 줄여준다. 의미있는 값만 명시하라.

필드 이름이 의미있는 컨텍스트를 제공하는 경우에는 제로 값을 포함하라. 예를 들어, [테스트 테이블](#테스트-테이블-test-tables)의 테스트 케이스는 제로 값이더라도 필드 이름으로부터 이점을 얻을 수 있다.

```go
tests := []struct{
  give string
  want int
}{
  {give: "0", want: 0},
  // ...
}
```

#### 제로 값 구조체에 `var` 사용 (Use `var` for Zero Value Structs)

구조체 선언에서 모든 필드가 생략될 때는 `var` 형식으로 구조체를 선언하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
user := User{}
```

</td><td>

```go
var user User
```

</td></tr>
</tbody></table>

이렇게 하면 제로 값 구조체와 비제로 필드를 가진 구조체를 구분할 수 있으며, [map 초기화](#map-초기화-initializing-maps)에 대해 생성된 구분과 유사하고, 빈 슬라이스 선언 방식과도 일치한다.

#### 구조체 참조 초기화 (Initializing Struct References)

구조체 참조(struct reference)를 초기화 할 때, `new(T)`대신에 `&T{}`을 사용하여 구조체 초기화와 일관성을 가지도록 해라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
sval := T{Name: "foo"}

// inconsistent
sptr := new(T)
sptr.Name = "bar"
```

</td><td>

```go
sval := T{Name: "foo"}

sptr := &T{Name: "bar"}
```

</td></tr>
</tbody></table>

### 지역 변수 선언 (Local Variable Declarations)

변수를 명시적으로 특정 값으로 설정하는 경우 짧은 변수 선언 (Short variable declarations, `:=`)을 사용해야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
var s = "foo"
```

</td><td>

```go
s := "foo"
```

</td></tr>
</tbody></table>

그러나, `var` 키워드를 사용할 때 기본값(default value)가 더 명확할 때가 있다. 예를 들면, [Declaring Empty Slices].

  [Declaring Empty Slices]: https://github.com/golang/go/wiki/CodeReviewComments#declaring-empty-slices

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func f(list []int) {
  filtered := []int{}
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td><td>

```go
func f(list []int) {
  var filtered []int
  for _, v := range list {
    if v > 10 {
      filtered = append(filtered, v)
    }
  }
}
```

</td></tr>
</tbody></table>

### nil은 유효한 슬라이스 (nil is a valid slice)

`nil`은 길이가 0인 유효한 슬라이스이다. 이는 다음과 같음을 의미한다:

- 길이가 0인 슬라이스를 명시적으로 반환해서는 안된다. 대신 nil을 반환하라.

  <table>
  <thead><tr><th>Bad</th><th>Good</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  if x == "" {
    return []int{}
  }
  ```

  </td><td>

  ```go
  if x == "" {
    return nil
  }
  ```

  </td></tr>
  </tbody></table>

- 슬라이스가 비어있는지 확인하기 위해서 항상 `len(s) == 0`을 사용해라. `nil`을 체크하지 말 것.

  <table>
  <thead><tr><th>Bad</th><th>Good</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  func isEmpty(s []string) bool {
    return s == nil
  }
  ```

  </td><td>

  ```go
  func isEmpty(s []string) bool {
    return len(s) == 0
  }
  ```

  </td></tr>
  </tbody></table>

- 제로 값(The zero value), `var`로 선언된 슬라이스의 경우,은 `make()`없이 바로 사용 할 수 있다.

  <table>
  <thead><tr><th>Bad</th><th>Good</th></tr></thead>
  <tbody>
  <tr><td>

  ```go
  nums := []int{}
  // or, nums := make([]int)

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td><td>

  ```go
  var nums []int

  if add1 {
    nums = append(nums, 1)
  }

  if add2 {
    nums = append(nums, 2)
  }
  ```

  </td></tr>
  </tbody></table>

### 변수의 범위를 줄여라 (Reduce Scope of Variables)

가능한 변수의 범위를 줄여라. 만약 [중첩 감소](#중첩-감소-reduce-nesting)와 충돌하는 경우 범위를 줄이면 안된다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
err := os.WriteFile(name, data, 0644)
if err != nil {
 return err
}
```

</td><td>

```go
if err := os.WriteFile(name, data, 0644); err != nil {
 return err
}
```

</td></tr>
</tbody></table>

`if`외부에서 함수 호출의 결과가 필요한 경우, 범위를 줄이려고 시도해서는 안된다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
if data, err := os.ReadFile(name); err == nil {
  err = cfg.Decode(data)
  if err != nil {
    return err
  }

  fmt.Println(cfg)
  return nil
} else {
  return err
}
```

</td><td>

```go
data, err := os.ReadFile(name)
if err != nil {
   return err
}

if err := cfg.Decode(data); err != nil {
  return err
}

fmt.Println(cfg)
return nil
```

</td></tr>
</tbody></table>

이 원칙은 변수뿐만 아니라 상수에도 적용된다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
const (
  _defaultPort = 8080
  _defaultUser = "user"
)

func Bar() {
  fmt.Println("Default port", _defaultPort)
}
```

</td><td>

```go
func Bar() {
  const (
    defaultPort = 8080
    defaultUser = "user"
  )
  fmt.Println("Default port", defaultPort)
}
```

</td></tr>
</tbody></table>

### Naked 매개변수를 피해라 (Avoid Naked Parameters)

함수 호출에서의 naked parameters는 가독성을 떨어 뜨릴 수 있다. 의미가 명확하지 않은 경우, C언어 스타일의 주석 (`/* ... */`)을 추가하기 바란다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true, true)
```

</td><td>

```go
// func printInfo(name string, isLocal, done bool)

printInfo("foo", true /* isLocal */, true /* done */)
```

</td></tr>
</tbody></table>

더 나은 방법은, naked `bool` 타입을 가독성과 타입 안전성을 위해 커스텀 타입으로 교체하는 것이다. 이렇게 하면 나중에 두 가지 상태(true/false) 이상으로 확장할 수 있다.

```go
type Region int

const (
  UnknownRegion Region = iota
  Local
)

type Status int

const (
  StatusReady Status = iota + 1
  StatusDone
  // 향후에 StatusInProgress를 추가할 수 있다.
)

func printInfo(name string, region Region, status Status)
```

### 이스케이핑을 피하기 위해 원시 문자 리터럴 사용 (Use Raw String Literals to Avoid Escaping)

Go는 [raw string literals](https://go.dev/ref/spec#raw_string_lit)을 지원하며 여러 줄에 걸친 코드와 따옴표를 포함할 수 있다. 이스케이프 처리가 많아 읽기 어려운 문자열을 피하기 위해 원시 문자 리터럴을 사용하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
wantError := "unknown name:\"test\""
```

</td><td>

```go
wantError := `unknown error:"test"`
```

</td></tr>
</tbody></table>

### Map 초기화 (Initializing Maps)

비어있는 map과 프로그래밍 방식으로 채워지는 map에는 `make(..)`를 선호하라. 이렇게 하면 map 초기화가 선언과 시각적으로 구분되고, 나중에 크기 힌트를 추가하기도 쉽다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
var (
  // m1은 읽기/쓰기가 안전하다;
  // m2는 쓰기 시 패닉이 발생한다.
  m1 = map[T1]T2{}
  m2 map[T1]T2
)
```

</td><td>

```go
var (
  // m1은 읽기/쓰기가 안전하다;
  // m2는 쓰기 시 패닉이 발생한다.
  m1 = make(map[T1]T2)
  m2 map[T1]T2
)
```

</td></tr>
<tr><td>

선언과 초기화가 시각적으로 유사하다.

</td><td>

선언과 초기화가 시각적으로 구분된다.

</td></tr>
</tbody></table>

가능하면 `make()`로 map을 초기화할 때 용량 힌트를 제공하라. 자세한 내용은 [Map 용량 힌트 지정](#map-용량-힌트-지정-specifying-map-capacity-hints)을 참고하라.

반면에, map이 고정된 요소 목록을 담고 있다면, map 리터럴을 사용하여 map을 초기화하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
m := make(map[T1]T2, 3)
m[k1] = v1
m[k2] = v2
m[k3] = v3
```

</td><td>

```go
m := map[T1]T2{
  k1: v1,
  k2: v2,
  k3: v3,
}
```

</td></tr>
</tbody></table>

기본 규칙은 초기화 시 고정된 요소 집합을 추가할 때는 map 리터럴을 사용하고, 그렇지 않은 경우에는 `make`(가능하면 크기 힌트 지정)를 사용하는 것이다.

### Printf외부의 문자열 형식 (Format Strings outside Printf)

문자열 리터럴 외부의 `Printf`-스타일의 함수에 대한 형식 문자열(format strings)을 선언하는 경우 `const`값 (const value)로 만들어라.

이는 `go vet`이 형식 문자열의 정적 분석(static analysis) 수행하는데 도움이 된다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
msg := "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td><td>

```go
const msg = "unexpected values %v, %v\n"
fmt.Printf(msg, 1, 2)
```

</td></tr>
</tbody></table>

### Printf-스타일 함수의 이름 (Naming Printf-style Functions)

`Printf`-스타일의 함수를 선언할 때, `go vet`이 이를 감지하고 형식 문자열 (format string)을 체크 할 수 있는지 확인해라.

이것은 미리 정의된 `Printf` 스타일 함수를 사용해야 한다는 뜻이다. `go vet`이 기본적으로 이를 검사한다. 자세한 정보는 다음을 참조하기 바란다: [Printf family]

  [Printf family]: https://pkg.go.dev/cmd/vet#hdr-Printf_family

미리 정의된 이름(pre-defined names)을 사용하는 것이 옵션이 아니라면, 선택한 이름은 `f`로 끝내야 한다: `Wrap`이 아닌 `Wrapf`. `go vet`은 특정 `Printf`-스타일의 이름을 확인하도록 요청받을 수 있으나 이들의 이름은 모두 `f`로 끝나야만 한다.

```shell
$ go vet -printfuncs=wrapf,statusf
```

또한 다음을 참고해라: [go vet: Printf family check].

  [go vet: Printf family check]: https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/

## 패턴 (Patterns)

### 테스트 테이블 (Test Tables)

핵심 테스트 로직이 반복될 때는 [subtests]를 활용한 테이블 기반 테스트(table-driven test)를 사용하라.

  [subtests]: https://blog.golang.org/subtests

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestSplitHostPort(t *testing.T)

host, port, err := net.SplitHostPort("192.0.2.0:8000")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("192.0.2.0:http")
require.NoError(t, err)
assert.Equal(t, "192.0.2.0", host)
assert.Equal(t, "http", port)

host, port, err = net.SplitHostPort(":8000")
require.NoError(t, err)
assert.Equal(t, "", host)
assert.Equal(t, "8000", port)

host, port, err = net.SplitHostPort("1:8")
require.NoError(t, err)
assert.Equal(t, "1", host)
assert.Equal(t, "8", port)
```

</td><td>

```go
// func TestSplitHostPort(t *testing.T)

tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  {
    give:     "192.0.2.0:8000",
    wantHost: "192.0.2.0",
    wantPort: "8000",
  },
  {
    give:     "192.0.2.0:http",
    wantHost: "192.0.2.0",
    wantPort: "http",
  },
  {
    give:     ":8000",
    wantHost: "",
    wantPort: "8000",
  },
  {
    give:     "1:8",
    wantHost: "1",
    wantPort: "8",
  },
}

for _, tt := range tests {
  t.Run(tt.give, func(t *testing.T) {
    host, port, err := net.SplitHostPort(tt.give)
    require.NoError(t, err)
    assert.Equal(t, tt.wantHost, host)
    assert.Equal(t, tt.wantPort, port)
  })
}
```

</td></tr>
</tbody></table>

테스트 테이블을 사용하면 에러 메시지에 컨텍스트를 쉽게 추가하고, 중복된 로직을 줄일 수 있으며, 쉽게 새로운 테스트 케이스를 추가할 수 있다.

우리는 구조체 슬라이스를 `tests`라고 하고, 각 테스트 케이스를 `tt`라고 한다. 또한 각 테스트 케이스의 입력과 출력을 `give`와 `want` 접두어로 명시하는 것을 권장한다.

```go
tests := []struct{
  give     string
  wantHost string
  wantPort string
}{
  // ...
}

for _, tt := range tests {
  // ...
}
```

#### 테스트 테이블에서 불필요한 복잡성을 피하라 (Avoid Unnecessary Complexity in Table Tests)

서브테스트에 조건부 assertion이나 다른 분기 로직이 포함되어 있으면 테스트 테이블은 읽고 유지하기 어려울 수 있다. 서브테스트 내에 복잡하거나 조건부 로직이 필요한 경우(즉, `for` 루프 내의 복잡한 로직) 테스트 테이블을 **사용하지 말아야** 한다.

크고 복잡한 테스트 테이블은 테스트 실패를 디버깅하기 어렵게 만들어 가독성과 유지보수성을 해친다.

이런 테스트는 여러 테스트 테이블 또는 여러 개별 `Test...` 함수로 분리해야 한다.

목표로 삼을 이상적인 원칙:

* 가장 좁은 단위의 동작에 집중하라
* "테스트 깊이"를 최소화하고 조건부 assertion을 피하라 (아래 참고)
* 모든 테스트에서 테이블의 모든 필드가 사용되도록 하라
* 모든 테스트 로직이 모든 테이블 케이스에서 실행되도록 하라

여기서 "테스트 깊이"란 "주어진 테스트에서, 이전 assertion이 성립해야 하는 연속적인 assertion의 수"를 의미한다 (cyclomatic complexity와 유사). "얕은" 테스트는 assertion 간의 관계가 적고, 더 중요하게는 기본적으로 조건부가 될 가능성이 적다는 것을 의미한다.

구체적으로, 테스트 테이블은 여러 분기 경로(예: `shouldError`, `expectCall` 등)를 사용하거나, 특정 mock 기대에 대해 많은 `if` 문을 사용하거나(예: `shouldCallFoo`), 테이블 내부에 함수를 배치하는 경우(예: `setupMocks func(*FooMock)`) 혼란스럽고 읽기 어려워질 수 있다.

그러나 변경된 입력에 따라서만 동작이 바뀌는 경우에는, 유사한 케이스를 테스트 테이블로 묶어 모든 입력에 걸쳐 동작이 어떻게 달라지는지 더 잘 보여주는 것이 나을 수 있다. 비교 가능한 단위를 분리하여 비교하기 어렵게 만드는 것보다 낫기 때문이다.

테스트 본문이 짧고 직관적이라면, 성공 케이스와 실패 케이스에 대해 `shouldErr` 같은 테이블 필드로 에러 기대치를 지정하는 단일 분기 경로를 갖는 것도 괜찮다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func TestComplicatedTable(t *testing.T) {
  tests := []struct {
    give          string
    want          string
    wantErr       error
    shouldCallX   bool
    shouldCallY   bool
    giveXResponse string
    giveXErr      error
    giveYResponse string
    giveYErr      error
  }{
    // ...
  }

  for _, tt := range tests {
    t.Run(tt.give, func(t *testing.T) {
      // 목(mock) 설정
      ctrl := gomock.NewController(t)
      xMock := xmock.NewMockX(ctrl)
      if tt.shouldCallX {
        xMock.EXPECT().Call().Return(
          tt.giveXResponse, tt.giveXErr,
        )
      }
      yMock := ymock.NewMockY(ctrl)
      if tt.shouldCallY {
        yMock.EXPECT().Call().Return(
          tt.giveYResponse, tt.giveYErr,
        )
      }

      got, err := DoComplexThing(tt.give, xMock, yMock)

      // 결과 검증
      if tt.wantErr != nil {
        require.EqualError(t, err, tt.wantErr)
        return
      }
      require.NoError(t, err)
      assert.Equal(t, want, got)
    })
  }
}
```

</td><td>

```go
func TestShouldCallX(t *testing.T) {
  // 목(mock) 설정
  ctrl := gomock.NewController(t)
  xMock := xmock.NewMockX(ctrl)
  xMock.EXPECT().Call().Return("XResponse", nil)

  yMock := ymock.NewMockY(ctrl)

  got, err := DoComplexThing("inputX", xMock, yMock)

  require.NoError(t, err)
  assert.Equal(t, "want", got)
}

func TestShouldCallYAndFail(t *testing.T) {
  // 목(mock) 설정
  ctrl := gomock.NewController(t)
  xMock := xmock.NewMockX(ctrl)

  yMock := ymock.NewMockY(ctrl)
  yMock.EXPECT().Call().Return("YResponse", nil)

  _, err := DoComplexThing("inputY", xMock, yMock)
  assert.EqualError(t, err, "Y failed")
}
```
</td></tr>
</tbody></table>

이러한 복잡성은 테스트의 변경, 이해, 정확성 증명을 더 어렵게 만든다.

엄격한 가이드라인은 없지만, 시스템에 대한 여러 입력/출력에 대해 테스트 테이블과 개별 테스트 중 무엇을 선택할지 결정할 때는 항상 가독성과 유지보수성을 최우선으로 고려해야 한다.

#### 병렬 테스트 (Parallel Tests)

병렬 테스트는, goroutine을 생성하거나 루프 본문의 일부로 참조를 캡처하는 특수한 루프와 마찬가지로, 예상되는 값을 보유하도록 루프의 스코프 내에서 루프 변수를 명시적으로 할당하는 데 주의를 기울여야 한다.

```go
tests := []struct{
  give string
  // ...
}{
  // ...
}

for _, tt := range tests {
  t.Run(tt.give, func(t *testing.T) {
    t.Parallel()
    // ...
  })
}
```

위 예시에서, `t.Parallel()`의 사용 때문에 루프 반복에 스코프가 지정된 `tt` 변수를 선언해야 한다. 그렇지 않으면 대부분의 또는 모든 테스트가 `tt`에 대해 예상치 못한 값을 받거나, 실행되는 동안 변경되는 값을 받게 된다.

### 기능적 옵션 (Functional Options)

기능적 옵션(functional options)은 일부 내부 구조체(internal struct)에 정보를 기록하는 불투명한 `Option` 타입을 선언하는 패턴이다. 가변 개수의 옵션을 받아 내부 구조체에 기록된 전체 정보를 바탕으로 동작한다.

확장이 필요한 생성자(constructor) 및 기타 공개 API의 선택적 인수에 이 패턴을 사용하라. 특히 해당 함수에 이미 3개 이상의 인수가 있는 경우에 적합하다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// package db

func Open(
  addr string,
  cache bool,
  logger *zap.Logger
) (*Connection, error) {
  // ...
}
```

</td><td>

```go
// package db

type Option interface {
  // ...
}

func WithCache(c bool) Option {
  // ...
}

func WithLogger(log *zap.Logger) Option {
  // ...
}

// Open creates a connection.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  // ...
}
```

</td></tr>
<tr><td>

cache와 logger 파라미터는 기본값을 사용하고 싶어도 항상 제공해야 한다.

```go
db.Open(addr, db.DefaultCache, zap.NewNop())
db.Open(addr, db.DefaultCache, log)
db.Open(addr, false /* cache */, zap.NewNop())
db.Open(addr, false /* cache */, log)
```

</td><td>

옵션은 필요할 때만 제공한다.

```go
db.Open(addr)
db.Open(addr, db.WithLogger(log))
db.Open(addr, db.WithCache(false))
db.Open(
  addr,
  db.WithCache(false),
  db.WithLogger(log),
)
```

</td></tr>
</tbody></table>

이 패턴을 구현하는 권장 방법은 비공개 메서드를 가진 `Option` 인터페이스를 사용하여 비공개 `options` 구조체에 옵션을 기록하는 것이다.

```go
type options struct {
  cache  bool
  logger *zap.Logger
}

type Option interface {
  apply(*options)
}

type cacheOption bool

func (c cacheOption) apply(opts *options) {
  opts.cache = bool(c)
}

func WithCache(c bool) Option {
  return cacheOption(c)
}

type loggerOption struct {
  Log *zap.Logger
}

func (l loggerOption) apply(opts *options) {
  opts.logger = l.Log
}

func WithLogger(log *zap.Logger) Option {
  return loggerOption{Log: log}
}

// Open creates a connection.
func Open(
  addr string,
  opts ...Option,
) (*Connection, error) {
  options := options{
    cache:  defaultCache,
    logger: zap.NewNop(),
  }

  for _, o := range opts {
    o.apply(&options)
  }

  // ...
}
```

클로저(closure)를 사용해 이 패턴을 구현하는 방법도 있지만, 위의 struct 기반 패턴이 작성자에게 더 많은 유연성을 제공하고 사용자가 디버깅 및 테스트하기 더 쉽다고 판단한다. 특히 테스트와 mock에서 옵션끼리 비교가 가능하지만 클로저에서는 불가능하다. 또한 옵션이 `fmt.Stringer`를 포함한 다른 인터페이스를 구현할 수 있어 사용자가 읽을 수 있는 문자열 표현을 제공할 수 있다.

아래의 자료도 참고하기 바란다:

- [Self-referential functions and the design of options](https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html)
- [Functional options for friendly APIs](https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis)

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->

## 린팅 (Linting)

어떤 "공인된" 린터 집합보다 더 중요한 것은, 코드베이스 전반에 걸쳐 일관되게 린트하는 것이다.

최소한 다음 린터들을 사용하는 것을 권장한다. 이 린터들은 가장 일반적인 문제를 잡아내고 불필요하게 규범적이지 않으면서도 높은 코드 품질 기준을 세우는 데 도움이 된다고 판단하기 때문이다:

- [errcheck](https://github.com/kisielk/errcheck): 에러가 처리되는지 확인
- [goimports](https://pkg.go.dev/golang.org/x/tools/cmd/goimports): 코드 포맷팅 및 import 관리
- [revive](https://github.com/mgechev/revive): 일반적인 스타일 실수 지적
- [govet](https://pkg.go.dev/cmd/vet): 일반적인 실수에 대한 코드 분석
- [staticcheck](https://staticcheck.dev): 다양한 정적 분석 검사 수행

  > **참고**: [revive](https://github.com/mgechev/revive)는 현재 deprecated된 [golint](https://github.com/golang/lint)의 현대적이고 더 빠른 후계자이다.

### 린트 실행기 (Lint Runners)

Go 코드의 린트 실행기로 [golangci-lint](https://github.com/golangci/golangci-lint)를 권장한다. 주로 대규모 코드베이스에서의 성능과 여러 표준 린터를 한 번에 구성하고 사용할 수 있는 기능 때문이다. 이 저장소에는 권장 린터와 설정이 포함된 예시 [.golangci.yml](https://github.com/uber-go/guide/blob/master/.golangci.yml) 설정 파일이 있다.

golangci-lint에는 사용 가능한 [다양한 린터](https://golangci-lint.run/usage/linters/)가 있다. 위에 나열된 린터들이 기본 세트로 권장되며, 각 팀이 프로젝트에 맞는 추가 린터를 더하는 것을 장려한다.
