# uber-go-style-guide-kr

Translated in Korean  

Currently WIP, but translation will done by 20th of Oct, 2019

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
- [Uber의 Go언어 스타일 가이드 (Uber's Go Style Guide)](#uber%ec%9d%98-go%ec%96%b8%ec%96%b4-%ec%8a%a4%ed%83%80%ec%9d%bc-%ea%b0%80%ec%9d%b4%eb%93%9c-ubers-go-style-guide)
  - [소개 (Introduction)](#%ec%86%8c%ea%b0%9c-introduction)
  - [가이드라인 (Guidelines)](#%ea%b0%80%ec%9d%b4%eb%93%9c%eb%9d%bc%ec%9d%b8-guidelines)
    - [인터페이스에 대한 포인터 (Pointers to Interfaces)](#%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4%ec%97%90-%eb%8c%80%ed%95%9c-%ed%8f%ac%ec%9d%b8%ed%84%b0-pointers-to-interfaces)
    - [수신자(Receivers)와 인터페이스(Interfaces)](#%ec%88%98%ec%8b%a0%ec%9e%90receivers%ec%99%80-%ec%9d%b8%ed%84%b0%ed%8e%98%ec%9d%b4%ec%8a%a4interfaces)
    - [제로 값 뮤텍스(Zero-value Mutexes)는 유효하다](#%ec%a0%9c%eb%a1%9c-%ea%b0%92-%eb%ae%a4%ed%85%8d%ec%8a%a4zero-value-mutexes%eb%8a%94-%ec%9c%a0%ed%9a%a8%ed%95%98%eb%8b%a4)
    - [슬라이스 복사(Copy Slices)와 바운더리 에서의 맵(Maps at Boundaries)](#%ec%8a%ac%eb%9d%bc%ec%9d%b4%ec%8a%a4-%eb%b3%b5%ec%82%accopy-slices%ec%99%80-%eb%b0%94%ec%9a%b4%eb%8d%94%eb%a6%ac-%ec%97%90%ec%84%9c%ec%9d%98-%eb%a7%b5maps-at-boundaries)
      - [Slices와 Maps의 수신(receiving)](#slices%ec%99%80-maps%ec%9d%98-%ec%88%98%ec%8b%a0receiving)
      - [슬라이스(Slices)와 맵(Maps)의 리턴](#%ec%8a%ac%eb%9d%bc%ec%9d%b4%ec%8a%a4slices%ec%99%80-%eb%a7%b5maps%ec%9d%98-%eb%a6%ac%ed%84%b4)
    - [Defer에서 Clean Up까지](#defer%ec%97%90%ec%84%9c-clean-up%ea%b9%8c%ec%a7%80)
    - [채널의 크기(Channel Size)는 하나(One) 혹은 제로(None)](#%ec%b1%84%eb%84%90%ec%9d%98-%ed%81%ac%ea%b8%b0channel-size%eb%8a%94-%ed%95%98%eb%82%98one-%ed%98%b9%ec%9d%80-%ec%a0%9c%eb%a1%9cnone)
    - [Enums은 1에서부터 시작하라](#enums%ec%9d%80-1%ec%97%90%ec%84%9c%eb%b6%80%ed%84%b0-%ec%8b%9c%ec%9e%91%ed%95%98%eb%9d%bc)
    - [에러 형(Error Types)](#%ec%97%90%eb%9f%ac-%ed%98%95error-types)
    - [오류 래핑(Error Wrapping)](#%ec%98%a4%eb%a5%98-%eb%9e%98%ed%95%91error-wrapping)
    - [타입의 어설션 실패 다루기 (Handle Type Assertion Failures)](#%ed%83%80%ec%9e%85%ec%9d%98-%ec%96%b4%ec%84%a4%ec%85%98-%ec%8b%a4%ed%8c%a8-%eb%8b%a4%eb%a3%a8%ea%b8%b0-handle-type-assertion-failures)
    - [패닉을 피할 것 (Don't Panic)](#%ed%8c%a8%eb%8b%89%ec%9d%84-%ed%94%bc%ed%95%a0-%ea%b2%83-dont-panic)
    - [Use go.uber.org/atomic](#use-gouberorgatomic)
  - [Performance](#performance)
    - [Prefer strconv over fmt](#prefer-strconv-over-fmt)
    - [Avoid string-to-byte conversion](#avoid-string-to-byte-conversion)
  - [Style](#style)
    - [Group Similar Declarations](#group-similar-declarations)
    - [Import Group Ordering](#import-group-ordering)
    - [Package Names](#package-names)
    - [Function Names](#function-names)
    - [Import Aliasing](#import-aliasing)
    - [Function Grouping and Ordering](#function-grouping-and-ordering)
    - [Reduce Nesting](#reduce-nesting)
    - [Unnecessary Else](#unnecessary-else)
    - [Top-level Variable Declarations](#top-level-variable-declarations)
    - [Prefix Unexported Globals with _](#prefix-unexported-globals-with)
    - [Embedding in Structs](#embedding-in-structs)
    - [Use Field Names to initialize Structs](#use-field-names-to-initialize-structs)
    - [Local Variable Declarations](#local-variable-declarations)
    - [nil is a valid slice](#nil-is-a-valid-slice)
    - [Reduce Scope of Variables](#reduce-scope-of-variables)
    - [Avoid Naked Parameters](#avoid-naked-parameters)
    - [Use Raw String Literals to Avoid Escaping](#use-raw-string-literals-to-avoid-escaping)
    - [Initializing Struct References](#initializing-struct-references)
    - [Format Strings outside Printf](#format-strings-outside-printf)
    - [Naming Printf-style Functions](#naming-printf-style-functions)
  - [Patterns](#patterns)
    - [Test Tables](#test-tables)
    - [Functional Options](#functional-options)

## 소개 (Introduction)

스타일은 코드를 통제하는(govern) 관습이다. 이러한 관습(convention)은 소스파일 포맷팅 (e.g. gofmt)보다 더 많은 영역을 다루기(cover) 때문에, "스타일" 이라는 단어 자체가 약간 부적절 할 수 있다.

본 가이드의 목표는 Uber에서 Go 코드를 작성할 때 해야 할 것과 하지 말아야 할 것 (Dos and Don'ts)에 대하여 자세하게 설명하여 이러한 복잡성을 관리하는 것이다. 이런 규칙들은 엔지니어들이 Go 언어의 특성을(feature) 생산적으로개계속 사용할 수 있도록 코드 베이스를 관리가능하게 유지하기위해 존재한다.

이 가이드는 원래 [Prashant Varanasi]와 [Simon Newton]이 동료들에게 Go를 사용하면서 개발속도 향상을 도모하기 위해 소개되었다. 또한, 수 년에 거쳐서 다른 사람들로부터의 피드백을 통해서 개정되 오고 있다.

  [Prashant Varanasi]: https://github.com/prashantv
  [Simon Newton]: https://github.com/nomis52 

이 문서는 Uber에서의 엔지니어들이 지향하는 Go언어 코드의 관용적 규칙을 설명한다. 상당 수의 규칙들은 Go언어에 대한 일반적인 가이드라인이며, 다른 부분에 대해서는 외부 레퍼런스에 의해 확장된다 (아래 참고)

1. [Effective Go](https://golang.org/doc/effective_go.html)
2. [The Go common mistakes guide](https://github.com/golang/go/wiki/CodeReviewComments)

모든 코드는 `golint` 와 `go vet`를 실행할 때 에러가 없어야 한다. 또한 우리는 여러분들의 에디터를 아래와 같이 설정하기를 권고한다:

- Run `goimports` on save
- Run `golint` and `go vet` to check for errors

아래의 링크를 통해서 Go 툴을 지원하는 에디터에 대한 정보를 얻을 수 있다:
<https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins>

## 가이드라인 (Guidelines)

### 인터페이스에 대한 포인터 (Pointers to Interfaces)

일반적으로 인터페이스에 대한 포인터는 거의 필요하지 않을 것이다. 여러분들은 인터페이스를 값(value)으로서 전달(passing)해야 할 것이며, 인터페이스에 대한 기본 데이터(underlying data)는 여전히 포인터가 될 수 있다.

한 인터페이스는 두 가지 필드이다:

1. 타입-특정 정보(type-specific information)에 대한 포인터. 여러분들을 이것을 "타입"으로 간주할 수 있다.
2. 데이터 포인터. 저장된 데이터가 포인터일 경우, 이것은 직접적으로 저장될 수 있다. 만약, 저장된 데이터가 값(value)인 경우, 값에 대한 포인터가 저장된다.

만약 여러분들이 기본 데이터(underlying data) 수정하기 위한 인터페이스 메서드 (interface methods)를 원한다면, 여러분들은 반드시 포인터를 사용해야 한다.

### 수신자(Receivers)와 인터페이스(Interfaces)

값 수신자 (value receivers)와 메서드(Methods)는 포인터 혹은 값에 의해서 호출 될 수 있다.

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

// 오직 값만 사용하여 Read를 호출 할 수 있다.
sVals[1].Read()

// 아래 코드는 컴파일 되지 않을 것:
//  sVals[1].Write("test")

sPtrs := map[int]*S{1: {"A"}}

// 포인터를 사용하여 Read와 Write 모두 호출 할 수 있다.
sPtrs[1].Read()
sPtrs[1].Write("test")
```

마찬가지로, 메서드가 값 수신자(value receiver)를 가지고 있다고 하더라도 포인터가 인터페이스를 충족시킬 수 있다. 

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

// s2Val이 값이고 f에 대한 수신자가 없기 때문에, 아래의 코드는 컴파일 되지 않는다.
//   i = s2Val
```

Effective Go에 [Pointers vs. Values]에 대한 좋은 글이 있으니 참고하기 바란다.

  [Pointers vs. Values]: https://golang.org/doc/effective_go.html#pointers_vs_values

### 제로 값 뮤텍스(Zero-value Mutexes)는 유효하다

`sync.Mutex`와 `sync.RWMutex` 의 제로 값은 유효하므로, 거의 대부분의 경우 뮤텍스에 대한 포인터는 필요로 하지 않는다.

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

포인터로 구조체를 사용할 경우, 뮤텍스는 포인터가 아닌 필드(non-pointer field)가 될 수 있다.

구조체의 필드를 보호하기 위해 뮤텍스를 사용한 수출되지 않는 구조체(unexported structs)는 뮤텍스를 포함(embed) 할 수 있다.

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
<td>뮤텍스 인터페이스를 구현해야 하는 전용 타입(private type) 혹은 타입에 포함됨. </td>
<td>수출되는 타입(exported type)에 대해서는 전용 필드 (private field)를 사용함.</td>
</tr>

</tbody></table>

### 슬라이스 복사(Copy Slices)와 바운더리 에서의 맵(Maps at Boundaries)

슬라이스(Slices)와 맵(maps)은 기본 데이터(underlying data)에 대한 포인터를 포함하고 있으므로 이들을 복사 해야 할 때의 시나리오에 대해서 주의할 필요가 있다.  

#### Slices와 Maps의 수신(receiving)

참조/레퍼런스(reference)를 저장할 경우, 사용자는 인수(argument)로 받는 맵 혹은 슬라이스를 수정할 수 있음을 명심하라.

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

// d1.trips을 수정할 것을 의미하는가?
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

// 이제 d1.trips에 영향을 주지 않고서 trips[0]을 수정 할 수 있다.
trips[0] = ...
```

</td>
</tr>

</tbody턴
</table>

#### 슬라이스(Slices)와 맵(Maps)의 리턴

마찬가지로, 내부 상태(internal status)를 노출시키는 슬라이스나 맵에 대한 사용자의 수정에 주의하라.

<table>
<thead><tr><th>Bad</th><th>Good</th></tr></thead>
<tbody>
<tr><td>

```go
type Stats struct {
  mu sync.Mutex
  counters map[string]int
}

// Snapshot은 현재의 stats을 반환(return)한다
func (s *Stats) Snapshot() map[string]int {
  s.mu.Lock()
  defer s.mu.Unlock()

  return s.counters
}

// snapshot은 더이상 뮤텍스에 의해서 보호되지 않는다.
// 따라서, snapshot에 대한 access가 안정되지 않는다. (any access to the snapshot is racy.)
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

// Snapshot는 카피(copy)다.
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

`defer`는 오버헤드가 상당히 작으며 함수 실행 시간이 나노초 단위임을 증명할 수 있을 경우가 아닌 이상 피하지 않고 사용해야 한다. `defer`의 사용으로 인한 가독성의 이점으로 인하여 지연을 사용하는 비용은 적다. 간단한 메모리 접근(simple memory accesses)이상을 가지는 거대한 메서로가 있는 경우, 다른 계산이 `defer`보다 더 중요하다.

### 채널의 크기(Channel Size)는 하나(One) 혹은 제로(None)

채널의 크기는 일반적으로 1이거나 혹은 버퍼링 되지 않아야 한다. 기본적으로, 채널은 버퍼링되지 않으며 크기는 0이다. 0 이외의 다른 크기는 높은 수준의 철저한 검토 혹은 정밀조사(scrutiny)를 받아야 한다. 어떻게 크기를 결정(determined)할 지 고려하라. 무엇이 채널이 로드할 경우 가득 차거나 writer가 막히는(blocked) 것을 예방하는지 그리고 이러한 것이 발생할 경우 어떤 일이 일어날 지 충분히 생각해야 한다.

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

- 추가적인 컨텍스트(additional context)가 없고 원래의 에러 타입을 유지하려는 경우 본래의 에러(original error)를 반환하라.
- 에러 메시지가 더 많은 컨텍스트를 제공하면서 [`"pkg/errors".Cause`]가 원래 오류를 추출하는데 사용될 수 있도록 [`"pkg/errors".Wrap`]을 사용하여 컨텍스트를 추가하라.
- 호출자(callers)가 특정한 에러 케이스를(specific error case)를 감지하거나 다룰(handle) 필요가 없는 경우 [`fmt.Errorf`]를 사용하라.

"connection refused"와 같은 모호한 오류보다, 컨첵스트를 추가하는 것을 추천한다. 따라서 여러분들은 "call service foo: connection refused."와 같이 더욱 유용한 에러를 얻을 수 있을 것이다.

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

### Use go.uber.org/atomic

Atomic operations with the [sync/atomic] package operate on the raw types
(`int32`, `int64`, etc.) so it is easy to forget to use the atomic operation to
read or modify the variables.

[go.uber.org/atomic] adds type safety to these operations by hiding the
underlying type. Additionally, it includes a convenient `atomic.Bool` type.

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

## Performance

Performance-specific guidelines apply only to the hot path.

### Prefer strconv over fmt

When converting primitives to/from strings, `strconv` is faster than
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

### Avoid string-to-byte conversion

Do not create byte slices from a fixed string repeatedly. Instead, perform the
conversion once and capture the result.

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

## Style

### Group Similar Declarations

Go supports grouping similar declarations.

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

This also applies to constants, variables, and type declarations.

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

Only group related declarations. Do not group declarations that are unrelated.

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

Groups are not limited in where they can be used. For example, you can use them
inside of functions.

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

### Import Group Ordering

There should be two import groups:

- Standard library
- Everything else

This is the grouping applied by goimports by default.

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

### Package Names

When naming packages, choose a name that is:

- All lower-case. No capitals or underscores.
- Does not need to be renamed using named imports at most call sites.
- Short and succinct. Remember that the name is identified in full at every call
  site.
- Not plural. For example, `net/url`, not `net/urls`.
- Not "common", "util", "shared", or "lib". These are bad, uninformative names.

See also [Package Names] and [Style guideline for Go packages].

  [Package Names]: https://blog.golang.org/package-names
  [Style guideline for Go packages]: https://rakyll.org/style-packages/

### Function Names

We follow the Go community's convention of using [MixedCaps for function
names]. An exception is made for test functions, which may contain underscores
for the purpose of grouping related test cases, e.g.,
`TestMyFunction_WhatIsBeingTested`.

  [MixedCaps for function names]: https://golang.org/doc/effective_go.html#mixed-caps

### Import Aliasing

Import aliasing must be used if the package name does not match the last
element of the import path.

```go
import (
  "net/http"

  client "example.com/client-go"
  trace "example.com/trace/v2"
)
```

In all other scenarios, import aliases should be avoided unless there is a
direct conflict between imports.

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

### Function Grouping and Ordering

- Functions should be sorted in rough call order.
- Functions in a file should be grouped by receiver.

Therefore, exported functions should appear first in a file, after
`struct`, `const`, `var` definitions.

A `newXYZ()`/`NewXYZ()` may appear after the type is defined, but before the
rest of the methods on the receiver.

Since functions are grouped by receiver, plain utility functions should appear
towards the end of the file.

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

### Reduce Nesting

Code should reduce nesting where possible by handling error cases/special
conditions first and returning early or continuing the loop. Reduce the amount
of code that is nested multiple levels.

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

### Unnecessary Else

If a variable is set in both branches of an if, it can be replaced with a
single if.

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

### Top-level Variable Declarations

At the top level, use the standard `var` keyword. Do not specify the type,
unless it is not the same type as the expression.

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
// Since F already states that it returns a string, we don't need to specify
// the type again.

func F() string { return "A" }
```

</td></tr>
</tbody></table>

Specify the type if the type of the expression does not match the desired type
exactly.

```go
type myError struct{}

func (myError) Error() string { return "error" }

func F() myError { return myError{} }

var _e error = F()
// F returns an object of type myError but we want error.
```

### Prefix Unexported Globals with _

Prefix unexported top-level `var`s and `const`s with `_` to make it clear when
they are used that they are global symbols.

Exception: Unexported error values, which should be prefixed with `err`.

Rationale: Top-level variables and constants have a package scope. Using a
generic name makes it easy to accidentally use the wrong value in a different
file.

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

  // We will not see a compile error if the first line of
  // Bar() is deleted.
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

### Embedding in Structs

Embedded types (such as mutexes) should be at the top of the field list of a
struct, and there must be an empty line separating embedded fields from regular
fields.

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

### Use Field Names to initialize Structs

You should almost always specify field names when initializing structs. This is
now enforced by [`go vet`].

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

Exception: Field names *may* be omitted in test tables when there are 3 or
fewer fields.

```go
tests := []struct{
  op Operation
  want string
}{
  {Add, "add"},
  {Subtract, "subtract"},
}
```

### Local Variable Declarations

Short variable declarations (`:=`) should be used if a variable is being set to
some value explicitly.

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

However, there are cases where the default value is clearer when the `var`
keyword is use. [Declaring Empty Slices], for example.

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

### nil is a valid slice

`nil` is a valid slice of length 0. This means that,

- You should not return a slice of length zero explicitly. Return `nil`
  instead.

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

- To check if a slice is empty, always use `len(s) == 0`. Do not check for
  `nil`.

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

- The zero value (a slice declared with `var`) is usable immediately without
  `make()`.

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

### Reduce Scope of Variables

Where possible, reduce scope of variables. Do not reduce the scope if it
conflicts with [Reduce Nesting](#reduce-nesting).

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

If you need a result of a function call outside of the if, then you should not
try to reduce the scope.

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

### Avoid Naked Parameters

Naked parameters in function calls can hurt readability. Add C-style comments
(`/* ... */`) for parameter names when their meaning is not obvious.

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

Better yet, replace naked `bool` types with custom types for more readable and
type-safe code. This allows more than just two states (true/false) for that
parameter in the future.

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
  // Maybe we will have a StatusInProgress in the future.
)

func printInfo(name string, region Region, status Status)
```

### Use Raw String Literals to Avoid Escaping

Go supports [raw string literals](https://golang.org/ref/spec#raw_string_lit),
which can span multiple lines and include quotes. Use these to avoid
hand-escaped strings which are much harder to read.

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

### Initializing Struct References

Use `&T{}` instead of `new(T)` when initializing struct references so that it
is consistent with the struct initialization.

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

### Format Strings outside Printf

If you declare format strings for `Printf`-style functions outside a string
literal, make them `const` values.

This helps `go vet` perform static analysis of the format string.

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

### Naming Printf-style Functions

When you declare a `Printf`-style function, make sure that `go vet` can detect
it and check the format string.

This means that you should use pre-defined `Printf`-style function
names if possible. `go vet` will check these by default. See [Printf family]
for more information.

  [Printf family]: https://golang.org/cmd/vet/#hdr-Printf_family

If using the pre-defined names is not an option, end the name you choose with
f: `Wrapf`, not `Wrap`. `go vet` can be asked to check specific `Printf`-style
names but they must end with f.

```shell
$ go vet -printfuncs=wrapf,statusf
```

See also [go vet: Printf family check].

  [go vet: Printf family check]: https://kuzminva.wordpress.com/2017/11/07/go-vet-printf-family-check/

## Patterns

### Test Tables

Use table-driven tests with [subtests] to avoid duplicating code when the core
test logic is repetitive.

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

Test tables make it easier to add context to error messages, reduce duplicate
logic, and add new test cases.

We follow the convention that the slice of structs is referred to as `tests`
and each test case `tt`. Further, we encourage explicating the input and output
values for each test case with `give` and `want` prefixes.

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

### Functional Options

Functional options is a pattern in which you declare an opaque `Option` type
that records information in some internal struct. You accept a variadic number
of these options and act upon the full information recorded by the options on
the internal struct.

Use this pattern for optional arguments in constructors and other public APIs
that you foresee needing to expand, especially if you already have three or
more arguments on those functions.

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

See also,

- [Self-referential functions and the design of options]
- [Functional options for friendly APIs]

  [Self-referential functions and the design of options]: https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html
  [Functional options for friendly APIs]: https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->
