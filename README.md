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
    - [go.uber.org/atomic의 사용](#gouberorgatomic%ec%9d%98-%ec%82%ac%ec%9a%a9)
  - [성능(Performance)](#%ec%84%b1%eb%8a%a5performance)
    - [`fmt` 보다 `strconv` 선호](#fmt-%eb%b3%b4%eb%8b%a4-strconv-%ec%84%a0%ed%98%b8)
    - [string-to-byte 변환을 피해라](#string-to-byte-%eb%b3%80%ed%99%98%ec%9d%84-%ed%94%bc%ed%95%b4%eb%9d%bc)
  - [스타일 (Style)](#%ec%8a%a4%ed%83%80%ec%9d%bc-style)
    - [그룹 유사 선언 (Group Similar Declarations)](#%ea%b7%b8%eb%a3%b9-%ec%9c%a0%ec%82%ac-%ec%84%a0%ec%96%b8-group-similar-declarations)
    - [Import 그룹 정리/배치 (Import Group Ordering)](#import-%ea%b7%b8%eb%a3%b9-%ec%a0%95%eb%a6%ac%eb%b0%b0%ec%b9%98-import-group-ordering)
    - [패키지 이름 (Package Names)](#%ed%8c%a8%ed%82%a4%ec%a7%80-%ec%9d%b4%eb%a6%84-package-names)
    - [함수 이름 (Function Names)](#%ed%95%a8%ec%88%98-%ec%9d%b4%eb%a6%84-function-names)
    - [Import 별칭 (Import Aliasing)](#import-%eb%b3%84%ec%b9%ad-import-aliasing)
    - [함수 그룹화와 정렬/배치 (Function Grouping and Ordering)](#%ed%95%a8%ec%88%98-%ea%b7%b8%eb%a3%b9%ed%99%94%ec%99%80-%ec%a0%95%eb%a0%ac%eb%b0%b0%ec%b9%98-function-grouping-and-ordering)
    - [중첩 감소 (Reduce Nesting)](#%ec%a4%91%ec%b2%a9-%ea%b0%90%ec%86%8c-reduce-nesting)
    - [불필요한 else (Unnecessary Else)](#%eb%b6%88%ed%95%84%ec%9a%94%ed%95%9c-else-unnecessary-else)
    - [최상위 변수 선언 (Top-level Variable Declarations)](#%ec%b5%9c%ec%83%81%ec%9c%84-%eb%b3%80%ec%88%98-%ec%84%a0%ec%96%b8-top-level-variable-declarations)
    - [수출되지 않은 전역에 _을 붙여라 (Prefix Unexported Globals with _)](#%ec%88%98%ec%b6%9c%eb%90%98%ec%a7%80-%ec%95%8a%ec%9d%80-%ec%a0%84%ec%97%ad%ec%97%90-%ec%9d%84-%eb%b6%99%ec%97%ac%eb%9d%bc-prefix-unexported-globals-with)
    - [구조체에서의 임베딩 (Embedding in Structs)](#%ea%b5%ac%ec%a1%b0%ec%b2%b4%ec%97%90%ec%84%9c%ec%9d%98-%ec%9e%84%eb%b2%a0%eb%94%a9-embedding-in-structs)
    - [구조체 초기화를 위해 필드을 사용해라 (Use Field Names to initialize Structs)](#%ea%b5%ac%ec%a1%b0%ec%b2%b4-%ec%b4%88%ea%b8%b0%ed%99%94%eb%a5%bc-%ec%9c%84%ed%95%b4-%ed%95%84%eb%93%9c%ec%9d%84-%ec%82%ac%ec%9a%a9%ed%95%b4%eb%9d%bc-use-field-names-to-initialize-structs)
    - [지역 변수 선언 (Local Variable Declarations)](#%ec%a7%80%ec%97%ad-%eb%b3%80%ec%88%98-%ec%84%a0%ec%96%b8-local-variable-declarations)
    - [nil은 유효한 슬라이스 (nil is a valid slice)](#nil%ec%9d%80-%ec%9c%a0%ed%9a%a8%ed%95%9c-%ec%8a%ac%eb%9d%bc%ec%9d%b4%ec%8a%a4-nil-is-a-valid-slice)
    - [변수의 범위를 줄여라 (Reduce Scope of Variables)](#%eb%b3%80%ec%88%98%ec%9d%98-%eb%b2%94%ec%9c%84%eb%a5%bc-%ec%a4%84%ec%97%ac%eb%9d%bc-reduce-scope-of-variables)
    - [Naked 매개변수를 피해라 (Avoid Naked Parameters)](#naked-%eb%a7%a4%ea%b0%9c%eb%b3%80%ec%88%98%eb%a5%bc-%ed%94%bc%ed%95%b4%eb%9d%bc-avoid-naked-parameters)
    - [이스케이핑을 피하기 위해 원시 문자 리터럴 사용 (Use Raw String Literals to Avoid Escaping)](#%ec%9d%b4%ec%8a%a4%ec%bc%80%ec%9d%b4%ed%95%91%ec%9d%84-%ed%94%bc%ed%95%98%ea%b8%b0-%ec%9c%84%ed%95%b4-%ec%9b%90%ec%8b%9c-%eb%ac%b8%ec%9e%90-%eb%a6%ac%ed%84%b0%eb%9f%b4-%ec%82%ac%ec%9a%a9-use-raw-string-literals-to-avoid-escaping)
    - [구조체 참조 초기화 (Initializing Struct References)](#%ea%b5%ac%ec%a1%b0%ec%b2%b4-%ec%b0%b8%ec%a1%b0-%ec%b4%88%ea%b8%b0%ed%99%94-initializing-struct-references)
    - [Printf외부의 문자열 형식 (Format Strings outside Printf)](#printf%ec%99%b8%eb%b6%80%ec%9d%98-%eb%ac%b8%ec%9e%90%ec%97%b4-%ed%98%95%ec%8b%9d-format-strings-outside-printf)
    - [Printf-스타일 함수의 이름 (Naming Printf-style Functions)](#printf-%ec%8a%a4%ed%83%80%ec%9d%bc-%ed%95%a8%ec%88%98%ec%9d%98-%ec%9d%b4%eb%a6%84-naming-printf-style-functions)
  - [패턴 (Patterns)](#%ed%8c%a8%ed%84%b4-patterns)
    - [테스트 테이블 (Test Tables)](#%ed%85%8c%ec%8a%a4%ed%8a%b8-%ed%85%8c%ec%9d%b4%eb%b8%94-test-tables)
    - [기능적 옵션 (Functional Options)](#%ea%b8%b0%eb%8a%a5%ec%a0%81-%ec%98%b5%ec%85%98-functional-options)

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

성능-특정의(performance-specific)가이드라인은 hot path에만 적용된다.

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

그룹화를 사용하는 장소는 제한되어 있지 않다. 예를 들어, 함수 내에서 그룹화는 사용 가능 하다.

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

이는 디폴트로 `goimports`에 의해서 적용되는 그룹들이다.

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
- "common", "util", "shared", 또는 "lib"의 용어 사용 금지. 정보가 없는 나쁜 이름들임.

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

`newXYZ()`/`NewXYZ()`가 타입이 정의된 뒷부분에 나타날 수 있지만, 이는 나머지 수신자(receiver)의 메서드들 전에 나타나야 한다 (may appear after the type is defined, but before the
rest of the methods on the receiver.)

함수들은 수신자에 의해 그룹화 되므로, 일반 유틸리티 함수들(plain utility functions)는 파일의 뒷부분에 나타나야 한다.

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

변수가 if의 두 가지 분기문에 의해서 설정될 경우, 이는 단일 `if`문 (simple if)으로 대체 될 수 있다.

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

이유: 최상위 변수 및 상수 (Top-level variables and constants)는 패키지 범위(package scope)를 가진다. 제네릭 이름(generic names)을 사용하는 것은 다른 파일에서 잘못된 값을 실수로 쉽게 사용할 수 있다.

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

뮤텍스와 같은 임베드된 타입은 구조체의 필드 목록 가장 상위층에 있어야 하고, 임베드 된 필드를 일반 필드와 분리하는 empty ilie이 있어야 한다.

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

### 구조체 초기화를 위해 필드을 사용해라 (Use Field Names to initialize Structs)

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

`nil`은 길이가 0인 유요한 슬라이스이다. 이는 다음과 같음을 의미한다:

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

가능한 변수의 범위를 줄여라. 만약 [Reduce Nesting](#reduce-nesting)과의 출돌하는 경우 범위를 줄이면 안된다.

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

함수 호출에서의 naked parameteres는 가독성을 떨어 뜨릴 수 있다. 의미가 명확하지 않은 경우, C언어 스타일의 주석 (`/* ... */`)을 추가하기 바란다.

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

문자열 리터럴 외부의 `Printf`-스타일의 함수에 대한 형식 문자열(format strings)을 선언하는 경우 `const`값 (const value)로 만들라.

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

기능적 옵션 (functional options)d은 일부 내부 구조체 (internal struct)에 정보를 기록하는 불투명한 `Option` 타입 (opaque option type)을 선언하는 패턴이다. 여러분들은 다양한 옵션 (variadic number of these options)을 받아들이고 내부 구조체의 옵션에 의해 기록된 모든 정보에 따라 행동하게 된다(act opon the full info. recorded by the options on the internal struct).

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

또한 다음을 참고하라,

- [Self-referential functions and the design of options]
- [Functional options for friendly APIs]

  [Self-referential functions and the design of options]: https://commandcenter.blogspot.com/2014/01/self-referential-functions-and-design.html
  [Functional options for friendly APIs]: https://dave.cheney.net/2014/10/17/functional-options-for-friendly-apis

<!-- TODO: replace this with parameter structs and functional options, when to
use one vs other -->
