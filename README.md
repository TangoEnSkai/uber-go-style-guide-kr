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
    - [인터페이스 컴플라이언스 검증](#인터페이스-컴플라이언스-검증)
    - [리시버(Receivers)와 인터페이스(Interfaces)](#리시버receivers와-인터페이스interfaces)
    - [제로 값 뮤텍스(Zero-value Mutexes)는 유효하다](#제로-값-뮤텍스zero-value-mutexes는-유효하다)
    - [바운더리에서 슬라이스 및 맵 복사](#바운더리에서-슬라이스-및-맵-복사)
      - [슬라이스와 맵 수신](#슬라이스와-맵-수신)
      - [슬라이스와 맵 반환](#슬라이스와-맵-반환)
    - [Defer에서 Clean Up까지](#defer에서-clean-up까지)
    - [채널의 크기(Channel Size)는 하나(One) 혹은 제로(None)](#채널의-크기channel-size는-하나one-혹은-제로none)
    - [Enums은 1에서부터 시작하라](#enums은-1에서부터-시작하라)
    - [에러 형(Error Types)](#에러-형error-types)
    - [오류 래핑(Error Wrapping)](#오류-래핑error-wrapping)
    - [타입의 어설션 실패 다루기 (Handle Type Assertion Failures)](#타입의-어설션-실패-다루기-handle-type-assertion-failures)
    - [패닉을 피할 것 (Don't Panic)](#패닉을-피할-것-dont-panic)
    - [go.uber.org/atomic의 사용](#gouberorgatomic의-사용)
  - [성능(Performance)](#성능performance)
    - [`fmt` 보다 `strconv` 선호](#fmt-보다-strconv-선호)
    - [string-to-byte 변환을 피해라](#string-to-byte-변환을-피해라)
  - [스타일 (Style)](#스타일-style)
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
    - [구조체 초기화를 위해 필드를 사용해라 (Use Field Names to initialize Structs)](#구조체-초기화를-위해-필드를-사용해라-use-field-names-to-initialize-structs)
    - [지역 변수 선언 (Local Variable Declarations)](#지역-변수-선언-local-variable-declarations)
    - [nil은 유효한 슬라이스 (nil is a valid slice)](#nil은-유효한-슬라이스-nil-is-a-valid-slice)
    - [변수의 범위를 줄여라 (Reduce Scope of Variables)](#변수의-범위를-줄여라-reduce-scope-of-variables)
    - [Naked 매개변수를 피해라 (Avoid Naked Parameters)](#naked-매개변수를-피해라-avoid-naked-parameters)
    - [이스케이핑을 피하기 위해 원시 문자 리터럴 사용 (Use Raw String Literals to Avoid Escaping)](#이스케이핑을-피하기-위해-원시-문자-리터럴-사용-use-raw-string-literals-to-avoid-escaping)
    - [구조체 참조 초기화 (Initializing Struct References)](#구조체-참조-초기화-initializing-struct-references)
    - [Printf외부의 문자열 형식 (Format Strings outside Printf)](#printf외부의-문자열-형식-format-strings-outside-printf)
    - [Printf-스타일 함수의 이름 (Naming Printf-style Functions)](#printf-스타일-함수의-이름-naming-printf-style-functions)
  - [패턴 (Patterns)](#패턴-patterns)
    - [테스트 테이블 (Test Tables)](#테스트-테이블-test-tables)
    - [기능적 옵션 (Functional Options)](#기능적-옵션-functional-options)

## 소개 (Introduction)

스타일(styles)은 코드를 관리(govern)하는 컨벤션/규칙(conventions)이다. 컨벤션은 잘 못 이해 될 수 있는데 왜냐하면 단순히 `gofmt`가 수행하는 소스 코드 포맷팅 이외의 의미도 포함하기 때문이다.

이 가이드의 목표는 Uber에서 Go 코드를 작성할 때 해야 할 것과 하지 말아야 할 것을 자세히 설명하여, 컨벤션의 복잡성을 관리하는 것이다.
이 컨벤션은 엔지니어가 Go언어을 생산적으로 사용할 수 있도록 하면서 코드를 관리 가능하게 유지하기 위해 존재한다.

이는 원래 [Prashant Varanasi]와 [Simon Newton]이 일부 동료들에게 Go를 사용하면서 개발속도 향상을 도모하기 위해 소개되었다. 수 년에 걸쳐 피드백을 통해 개선하고 있다.

  [Prashant Varanasi]: https://github.com/prashantv
  [Simon Newton]: https://github.com/nomis52 

이 문서는 Uber에서 따르는 Go 코드 컨벤션을 정리한다. 이들 중 많은 부분이 Go에 대한 일반적 지침이고, 나머지는 외부 리소스에 따라 확장한다:

1. [Effective Go](https://golang.org/doc/effective_go.html)
2. [Go Common Mistakes](https://github.com/golang/go/wiki/CommonMistakes)
3. [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)

모든 코드는 `golint` 및 `go vet`를 실행할 때 오류가 없어야 한다.
코드 에디터를 다음와 같이 설정하기를 권장한다:

- 코드 저장시 `goimports` 실행
- `golint` 및 `go vet`를 실행하여 오류 확인

여기에서 Go 도구에 대한 편집기 지원 정보를 찾을 수 있다:
<https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins>

## 가이드라인 (Guidelines)

### 인터페이스에 대한 포인터 (Pointers to Interfaces)

인터페이스에 대한 포인터는 거의 필요하지 않다.
인터페이스는 값(value)으로 전달해야 한다.
인터페이스에 대한 기본 데이터(underlying data)는 여전히 포인터 일 수 있다.


하나의 인터페이스는 두 가지 필드이다:

1. 타입-특정 정보(type-specific information)에 대한 포인터. 이것을 "타입"으로 간주할 수 있다.
2. 데이터 포인터. 저장된 데이터가 포인터일 경우 직접 저장된다. 저장된 데이터가 값이면 값에 대한 포인터가 저장된다.

인터페이스 메서드가 기본 데이터(underlying data)를 수정하도록 하려면 반드시 포인터를 사용해야 한다.

### 인터페이스 컴플라이언스 검증

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

값 리시버가 있는 메서드는 값 뿐만 아니라 포인터에서도 호출할 수 있습니다.
포인터 리시버 있는 메서드는 포인터 또는 주소 지정 가능한 값([addressable value](https://golang.org/ref/spec#Method_values))에서만 호출할 수 있습니다.

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

sVals := map[int]S{1: {"A"}}

// You can only call Read using a value
sVals[1].Read()

// This will not compile:
//  sVals[1].Write("test")

sPtrs := map[int]*S{1: {"A"}}

// You can call both Read and Write using a pointer
sPtrs[1].Read()
sPtrs[1].Write("test")
```

마찬가지로 메서드에 값 리시버가 있더라도 인터페이스는 포인터로 충족될 수 있습니다.

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

  [Pointers vs. Values]: https://golang.org/doc/effective_go.html#pointers_vs_values

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
  sync.Mutex // 오직 수출되지 않은 타입을 위해서 사용

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

### 바운더리에서 슬라이스 및 맵 복사

슬라이스 및 맵에는 기본 데이터에 대한 포인터가 포함되어 있으므로 복사해야 하는 시나리오에 주의 할 필요가 있다.

#### 슬라이스와 맵 수신

참조/레퍼런스(reference)를 저장하면 인수(argument)로 받은 맵이나 슬라이스를 사용자가 수정할 수 있음을 명심하자.

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

// Did you mean to modify d1.trips?
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

마찬가지로 내부 상태(internal state)를 노출하는 맵 또는 슬라이스에 대한 사용자 수정에 주의하자.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

// Snapshot returns the current stats.
func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  return s.counters
}

// snapshot is no longer protected by the mutex, so any
// access to the snapshot is subject to data races.
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

// Snapshot is now a copy.
snapshot := stats.Snapshot()
```

</td></tr>
</tbody></table>

### Defer에서 Clean Up까지

`defer`를 사용해여 파일(files)과 잠금(locks)과 같은 리소스를 정리하라.

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

// 여러개의 return으로 인해서 Unlock호출을 놓치기 쉬움
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

// 더 나은 가독성 
```

</td></tr>
</tbody></table>

`defer`는 오버헤드가 상당히 작으며 함수 실행 시간이 나노초 단위임을 증명할 수 있을 경우가 아닌 이상 피하지 않고 사용해야 한다. `defer`의 사용으로 인한 가독성의 이점으로 인하여 지연을 사용하는 비용은 적다. 간단한 메모리 접근(simple memory accesses)이상을 가지는 거대한 메서드가 있는 경우, 다른 계산이 `defer`보다 더 중요하다.

### 채널의 크기(Channel Size)는 하나(One) 혹은 제로(None)

채널의 크기는 일반적으로 1 이거나 혹은 버퍼링 되지 않아야 한다. 기본적으로, 채널은 버퍼링되지 않으며 크기는 0이다. 0 이외의 다른 크기는 높은 수준의 철저한 검토 혹은 정밀조사(scrutiny)를 받아야 한다. 어떻게 크기를 결정(determined)할 지 고려하라. 무엇이 채널이 로드할 경우 가득 차거나 writer가 막히는(blocked) 것을 예방하는지 그리고 이러한 것이 발생할 경우 어떤 일이 일어날 지 충분히 생각해야 한다.

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

### Enums은 1에서부터 시작하라

Go에서 열거형(enumerations)을 도입하는 일반적 방식(standard way)은 사용자정의형(a custom type) 그리고 `const`그룹을 `iota`와 함께 을 선언(declare)하는 것이다.

변수의 기본값(default value)는 0이기 때문에, 여러분들은 일반적으로 열거형을 0이 아닌 값(non-zero value)로 시작해야 한다.

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

제로 값(zero value)를 사용하는 것이 적절할 때도 있다. 예를 들면, 제로 값이 0인 경우 바람직한 기본 동작(default behaviour)이다.

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

### 에러 형(Error Types)

에러를 선언하는데 있어서 다양한 옵션들이 존재한다:

- [`errors.New`] 간단한 정적 문자열(simple static strings)과 함께하는 에러
- [`fmt.Errorf`] 형식화된 오류 문자열
- `Error()` 메서드를 구현한 커스텀 타입 (Custom types)
- [`"pkg/errors".Wrap`]를 사용하여 래핑 된(wrapped) 오류

오류를 반환할 때, 가장 좋은 선택을 하기 위해서 아래의 사항을 고려하라:

- 추가 정보가 필요없는 간단한 에러인가? 그렇다면, [`errors.New`]가 충분하다.
- 클라이언트가 오류를 감지하고 처리(handle)해야 하는가? 그렇다면, 커스텀 타입을 사용해야 하고 `Error()` 메서드를 구현해야 한다.
- 다운스트림 함수(downstream function)에 의해 반환된 에러를 전파(propagating)하고 있는가? 그렇다면, [오류 포장(Error Wrapping)](#%ec%98%a4%eb%a5%98-%eb%9e%98%ed%95%91error-wrapping)을 참고하라.
- 이외의 경우, [`fmt.Errorf`] 로 충분하다.

  [`errors.New`]: https://golang.org/pkg/errors/#New
  [`fmt.Errorf`]: https://golang.org/pkg/fmt/#Errorf
  [`"pkg/errors".Wrap`]: https://godoc.org/github.com/pkg/errors#Wrap

만약 클라이언트가 오류를 감지해야 하고, 여러분들이 [`errors.New`]을 사용하여 간단한 에러를 생성한 경우, `var`에 에러를 사용해라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// package foo

func Open() error {
  return errors.New("could not open")
}

// package bar

func use() {
  if err := foo.Open(); err != nil {
    if err.Error() == "could not open" {
      // handle
    } else {
      panic("unknown error")
    }
  }
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
  if err == foo.ErrCouldNotOpen {
    // handle
  } else {
    panic("unknown error")
  }
}
```

</td></tr>
</tbody></table>

만약 클라이언트가 감지해야 할 오류가 있고 여러분들이 이를 추가하려고 하는 경우, 그것에 대한 자세한 정보를 추가하고 싶을 것이다. (예를들어, 정적 문자열이 아닌 경우), 이러할 경우, 여러분들은 커스텀 타입을 사용해야 한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func open(file string) error {
  return fmt.Errorf("file %q not found", file)
}

func use() {
  if err := open(); err != nil {
    if strings.Contains(err.Error(), "not found") {
      // handle
    } else {
      panic("unknown error")
    }
  }
}
```

</td><td>

```go
type errNotFound struct {
  file string
}

func (e errNotFound) Error() string {
  return fmt.Sprintf("file %q not found", e.file)
}

func open(file string) error {
  return errNotFound{file: file}
}

func use() {
  if err := open(); err != nil {
    if _, ok := err.(errNotFound); ok {
      // handle
    } else {
      panic("unknown error")
    }
  }
}
```

</td></tr>
</tbody></table>

사용자 정의 오류 타입(custom error types)을 직접적으로 내보내는(exporting) 경우 주의해야 한다. 왜냐하면 그들은 패키지의 공용 API (the public API of the package)의 일부가 되기 때문이다. 대신에, 오류를 확인하기 위해서 매처 함수(matcher functions)를 노출하는 것이 좋다(preferable).

```go
// package foo

type errNotFound struct {
  file string
}

func (e errNotFound) Error() string {
  return fmt.Sprintf("file %q not found", e.file)
}

func IsNotFoundError(err error) bool {
  _, ok := err.(errNotFound)
  return ok
}

func Open(file string) error {
  return errNotFound{file: file}
}

// package bar

if err := foo.Open("foo"); err != nil {
  if foo.IsNotFoundError(err) {
    // handle
  } else {
    panic("unknown error")
  }
}
```

<!-- TODO: Exposing the information to callers with accessor functions. -->

### 오류 래핑(Error Wrapping)

호출이 실패할 경우 에러를 전파(propagating)하기 위한 3가지 주요 옵션이 있다:

- 추가적인 컨텍스트(additional context)가 없고 원래의 에러 타입을 유지하려는 경우 본래의 에러(original error)를 반환.
- 에러 메시지가 더 많은 컨텍스트를 제공하면서 [`"pkg/errors".Cause`]가 원래 오류를 추출하는데 사용될 수 있도록 [`"pkg/errors".Wrap`]을 사용하여 컨텍스트를 추가.
- 호출자(callers)가 특정한 에러 케이스를(specific error case)를 감지하거나 다룰(handle) 필요가 없는 경우 [`fmt.Errorf`]를 사용.

"connection refused"와 같은 모호한 오류보다, 컨텍스트를 추가하는 것을 추천한다. 따라서 여러분들은 "call service foo: connection refused."와 같이 더욱 유용한 에러를 얻을 수 있을 것이다.

반환된 오류에서 컨텍스트를 추가 할 때, "failed to"와 같은 사족의 명백한 문구를 피하며 컨텍스트를 간결하게 유지하도록 해라. 이러한 문구들이 에러가 스택에 퍼지면서/스며들면서(percolates) 계속해서 쌓이게 된다:

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "failed to create new store: %s", err)
}
```

</td><td>

```go
s, err := store.New()
if err != nil {
    return fmt.Errorf(
        "new store: %s", err)
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

  [`"pkg/errors".Cause`]: https://godoc.org/github.com/pkg/errors#Cause
  [Don't just check errors, handle them gracefully]: https://dave.cheney.net/2016/04/27/dont-just-check-errors-handle-them-gracefully

### 타입의 어설션 실패 다루기 (Handle Type Assertion Failures)

[type assertion]의 단일 반환 값 형식(the single return value form)은 잘못된 타입에 패닉 상태가 된다. 따라서 항상 "comma ok" 관용구(idiom)을 사용하는 것을 권장한다.

  [type assertion]: https://golang.org/ref/spec#Type_assertions

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
  // handle the error gracefully
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
func foo(bar string) {
  if len(bar) == 0 {
    panic("bar must not be empty")
  }
  // ...
}

func main() {
  if len(os.Args) != 2 {
    fmt.Println("USAGE: foo <bar>")
    os.Exit(1)
  }
  foo(os.Args[1])
}
```

</td><td>

```go
func foo(bar string) error {
  if len(bar) == 0 {
    return errors.New("bar must not be empty")
  }
  // ...
  return nil
}

func main() {
  if len(os.Args) != 2 {
    fmt.Println("USAGE: foo <bar>")
    os.Exit(1)
  }
  if err := foo(os.Args[1]); err != nil {
    panic(err)
  }
}
```

</td></tr>
</tbody></table>

Panic/recover는 오류 처리 전략(error handling strategy)이 이니다. nil dereference와 같이 복구 할 수 없는 일이 발생하는 경우에만 프로그램이 패닉 상태여야 한다. 프로그램 초기화는 여기에서 예외다: 프로그램을 시작 할 때, 프로그램을 중단해야 할 정도의 좋지 못한 일(bad things)이 발생할 경우 패닉을 일으킬 수 있다.

```go
var _statusTemplate = template.Must(template.New("name").Parse("_statusHTML"))
```

테스트에서 조차도, 테스트가 실패한 것으로 표기되는 것을 보장하기 위해 `panic`보다는 `t.Fatal` 혹은 `t.FailNow`가 선호된다.  

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// func TestFoo(t *testing.T)

f, err := ioutil.TempFile("", "test")
if err != nil {
  panic("failed to set up test")
}
```

</td><td>

```go
// func TestFoo(t *testing.T)

f, err := ioutil.TempFile("", "test")
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

  [go.uber.org/atomic]: https://godoc.org/go.uber.org/atomic
  [sync/atomic]: https://golang.org/pkg/sync/atomic/

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

## 성능(Performance)

성능-특정의(performance-specific)가이드라인은 성능에 민감한(hot path) 경우에만 적용된다.

### `fmt` 보다 `strconv` 선호

프리미티브(primitives)를 문자열로 / 문자열에서 변환 할 때, `strconv`가 `fmt`보다 빠르다.
`fmt`.

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

### string-to-byte 변환을 피해라

고정 문자열(fixed string)에서 바이트 슬라이스(byte slices)를 반복해서 생성하지 마라. 대신 변환(conversion)을 한번 실행하고, 결과를 캡쳐해라.

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

## 스타일 (Style)

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

오직 관련된 선언만 그룹화 할 것. 관련되지 않은 선언들에 대해서는 그룹화 하지 말것.

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
  ENV_VAR = "MY_ENV"
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

const ENV_VAR = "MY_ENV"
```

</td></tr>
</tbody></table>

그룹화를 사용하는 장소는 제한되어 있지 않다. 예를 들어, 함수 내에서도 그룹화를 사용할 수 있다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
func f() string {
  var red = color.New(0xff0000)
  var green = color.New(0x00ff00)
  var blue = color.New(0x0000ff)

  ...
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

  ...
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
- 대부분의 호출 지점(call sites)에서 named import를 사용하여 재명명(renamed)을 할 필요가 없다.
- 짧고 간결하게. 이름(name)은 모든 호출 지점(call site)에서 식별됨을 상기하라.
- 복수형(plural) 사용 금지. 예를 들어, `net/urls` 가 아닌 `net/url`.
- "common", "util", "shared", 또는 "lib"의 용어 사용 금지. 정보가 없는 좋지 못한 이름임.

또한 [Package Names] 와 [Style guideline for Go packages]를 참고하기 바란다.

  [Package Names]: https://blog.golang.org/package-names
  [Style guideline for Go packages]: https://rakyll.org/style-packages/

### 함수 이름 (Function Names)

우리는 Go 커뮤니티의 [MixedCaps for function names]의 사용에 의한 컨벤션을 따른다. 테스트 함수(test functions)는 예외이다. 이는 관련 테스트케이스를 그룹화 할 목적으로 언더스코어(_)를 포함할 수 있다, 예를들어, `TestMyFunction_WhatIsBeingTested`.

  [MixedCaps for function names]: https://golang.org/doc/effective_go.html#mixed-caps

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
)
```

</td></tr>
</tbody></table>

### 함수 그룹화와 정렬/배치 (Function Grouping and Ordering)

- 함수는 대략적 호출 순서에 의해서 정렬되어야 한다.
- 파일내에서의 함수는 리시버에 의해서 그룹지어져야 한다.

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

최상위 레벨에서 (At the top level), 표준 `var` 키워드를 사용해라. 표현식(expression)r과같은 같은 타입이 아닌 이상, 타입을 특정짓지 말라.

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

수출되지 않은 최상위(top-level) `var`와 `const`에 접두사 `_`를 붙임으로써 그들이 사용될 때, 전역 기호(global symbols)임을 명확하게 해라.

예외: 수출되지 않는 에러 값 (Unexported error values)은 `err`의 접두사를 가져야 한다.

이유: 최상위 변수 및 상수 (Top-level variables and constants)는 패키지 범위(package scope)를 가진다. 제네릭 이름(generic names)을 사용 하는 것은 다른 파일에서 잘못된 값을 실수로 쉽게 사용 할 수 있다.

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

뮤텍스와 같은 임베드된 타입은 구조체의 필드 목록 가장 상위층에 있어야 하고, 임베드 된 필드를 일반 필드와 분리하는 empty line이 있어야 한다.

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

### 구조체 초기화를 위해 필드를 사용해라 (Use Field Names to initialize Structs)

구조체를 초기화 할 때에는 거의 대부분 필드 명을 지정해야 한다. 이것은 이제 [`go vet`]에 의해서 강제하고 있다.

  [`go vet`]: https://golang.org/cmd/vet/

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

가능한 변수의 범위를 줄여라. 만약 [Reduce Nesting](#reduce-nesting)과의 충돌하는 경우 범위를 줄이면 안된다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
err := ioutil.WriteFile(name, data, 0644)
if err != nil {
 return err
}
```

</td><td>

```go
if err := ioutil.WriteFile(name, data, 0644); err != nil {
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
if data, err := ioutil.ReadFile(name); err == nil {
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
data, err := ioutil.ReadFile(name)
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

더 나은 방법은, naked `bool` 타입을 더 읽기 쉽고 타입-안정적(type-safe)인 코드를 위해서 사용자 정의 타입(custom type)으로 대체해라. 이를 통해서 향후 해당 매개변수에 대해서 두개 이상의 상태 (true/false)를 허용할 수 있다.

```go
type Region int

const (
  UnknownRegion Region = iota
  Local
)

type Status int

const (
  StatusReady = iota + 1
  StatusDone
  // 향후에 StatusInProgress를 추가할 수 있다.
)

func printInfo(name string, region Region, status Status)
```

### 이스케이핑을 피하기 위해 원시 문자 리터럴 사용 (Use Raw String Literals to Avoid Escaping)

Go는 [raw string literals](https://golang.org/ref/spec#raw_string_lit)을 지원하며 여러 줄에 걸쳐친 코드와 따옴표를 함께 포함할 수 있다. 읽기 어려운 hand-escaped strings를 피하기 위해서 원시 문자 리터럴을 사용해라.

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

### 구조체 참조 초기화 (Initializing Struct References)

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

이것은 미리 정의 된 `Printf`-스타일 함수를 사용해야 한다는 것을 의미한다. `go vet`이 이를 디폴트로 체크한다. 자세한 정보는 다음을 참조하기 바란다: [Printf family]

  [Printf family]: https://golang.org/cmd/vet/#hdr-Printf_family

미리 정의된 이름(pre-defined names)을 사용하는 것이 옵션이 아니라면, 선택한 이름을 f로 끝내도록 해라: `Wrap`이 아닌 `Wrapf`. `go vet`은 특정 `Printf`-스타일의 이름을 확인하도록 요청받을 수 있으나 이들의 이름은 모두 `f`로 끝나야만 한다.

```shell
$ go vet -printfuncs=wrapf,statusf
```

또한 다음을 참고해라: [go vet: Printf family check].

  [go vet: Printf family check]: https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/

## 패턴 (Patterns)

### 테스트 테이블 (Test Tables)

핵심적 테스트 로직(the core test logic)이 반복적일 때, 코드 중복을 피하려면 [subtests]와 함께 table-driven tests를 사용해라.

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

우리는 구조체 슬라이스를 `tests`라고 하고, 각 테스트 케이스를 `tt`라고 한다. 또한 각 테스트 케이스의 입력 및 출력 값을 `give` 및 `want` 접두어를 사용하여 설명(explicating)하는 것을 권장한다.

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

### 기능적 옵션 (Functional Options)

기능적 옵션(functional options)은 일부 내부 구조체 (internal struct)에 정보를 기록하는 불투명한 `Option` 타입 (opaque option type)을 선언하는 패턴이다. 여러분들은 다양한 옵션 (variadic number of these options)을 받아들이고 내부 구조체의 옵션에 의해 기록된 모든 정보에 따라 행동하게 된다(act opon the full info. recorded by the options on the internal struct).

확장 할 필요가 있는 생성자(constructors) 및 기타 공용 API (other public APIs)의 선택적 인수 (optional arguments), 특히나 해당하는 함수에 이미 3개 이상의 인수가 있는 경우에 이 패턴을 사용하기를 권장한다.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
// package db

func Connect(
  addr string,
  timeout time.Duration,
  caching bool,
) (*Connection, error) {
  // ...
}

// Timeout and caching must always be provided,
// even if the user wants to use the default.

db.Connect(addr, db.DefaultTimeout, db.DefaultCaching)
db.Connect(addr, newTimeout, db.DefaultCaching)
db.Connect(addr, db.DefaultTimeout, false /* caching */)
db.Connect(addr, newTimeout, false /* caching */)
```

</td><td>

```go
type options struct {
  timeout time.Duration
  caching bool
}

// Option overrides behavior of Connect.
type Option interface {
  apply(*options)
}

type optionFunc func(*options)

func (f optionFunc) apply(o *options) {
  f(o)
}

func WithTimeout(t time.Duration) Option {
  return optionFunc(func(o *options) {
    o.timeout = t
  })
}

func WithCaching(cache bool) Option {
  return optionFunc(func(o *options) {
    o.caching = cache
  })
}

// Connect creates a connection.
func Connect(
  addr string,
  opts ...Option,
) (*Connection, error) {
  options := options{
    timeout: defaultTimeout,
    caching: defaultCaching,
  }

  for _, o := range opts {
    o.apply(&options)
  }

  // ...
}

// Options must be provided only if needed.

db.Connect(addr)
db.Connect(addr, db.WithTimeout(newTimeout))
db.Connect(addr, db.WithCaching(false))
db.Connect(
  addr,
  db.WithCaching(false),
  db.WithTimeout(newTimeout),
)
```

</td></tr>
</tbody></table>

또한, 아래의 자료를 참고하기 바란다:

- [Self-referential functions and the design of options]
- [Functional options for friendly APIs]

  [Self-referential functions and the design of options]: https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html
  [Functional options for friendly APIs]: https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->
