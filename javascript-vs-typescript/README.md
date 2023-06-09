## 자바스크립트 vs 타입스크립트
- 자바스크립트: 코드 토큰화 → AST 생성 → Interpreter에 의해 Bytecode로 변환 → 브라우저 엔진의 컴파일러에 의해 브라우저에서 동작
- 타입스크립트: 자바스크립트 프로세스 + 타입스크립트 컴파일러 (타입 생성 + 검사 과정 추가)

> 타입스크립트 컴파일러를 통해 코드가 런타임 환경에서 실행되기 전, 문법 오류와 타입 오류를 발견할 수 있고 타입 추론과 자동완성 기능을 사용할 수 있다.

<br />

## 타입스크립트 컴파일러 구조
타입스크립트가 자바스크립트로 컴파일 되기 위해서 컴파일러가 코드를 해석하고 변환하는 과정이 필요하다. 
이 과정을 크게 5가지로 나눌 수 있다.

1. Scanner
2. Parser
3. Binder
4. Checker
5. Emitter

### Scanner
> 타입스크립트의 소스 코드를 토큰화하는 과정

![](https://velog.velcdn.com/images/dohun31/post/aac194c5-9298-4df9-a27f-1afab4d32996/image.gif)

- scanner가 코드를 읽으며(scan) 코드를 의미 있는 가장 작은 단위인 토큰(SyntaxKind)로 분할한다.
- 단순히 코드를 분류하고 필요한 정보를 넣는 것 이상으로 토큰의 위치와 토큰의 text, value 등 많은 것을 알 수 있도록 정리한다.
- 정학히 토큰의 생성 시점은 Parser 단계에서 생성된다. 생성된 토큰은 AST의 구성 요소인 Node를 나타낸다.

### Parser
> Scanner에서 만들어진 토큰을 조합하여 AST를 생성하는 과정

- AST는 소스 코드의 구조를 표현하는 데이터 구조로, 소스 코드의 Syntax 정보를 포함한다. 즉, Syntax 정보를 갖고 있는 Node를 트리 구조 형태로 가지고 있는 것이다.
- 토큰으로 Node를 만드는 과정에서 1차적으로 문법 오류를 확인한다.
- 문법에 어긋나는 코드가 있으면 Parser는 오류 메세지를 생성해 사용자에게 알린다. 
- 아직 타입이 생성되지 않았기 때문에 타입 오류를 잡아주지는 않는다.

### Binder
> Parser가 생성한 AST를 기반으로 Symbol 테이블을 생성하는 과정

- Symbol 테이블은 코드 내에서 정의한 변수, 함수, 클래스 등 모든 식별자들의 정보를 담고 있다. 식별자의 스코프, 타입, 선언 위치 등이 포함된다.
- AST와 Symbol 테이블은 서로 보완적인 정보를 제공힌다. 

### Checker
> 타입 검사: 정적 타입 검사가 이루어지는 과정

- Paerser 단계에서 1차로 문법 오류를 선별했다면 Checker 단계에서는 모든 타입에 관한 규칙을 검사한다.
- 식별자가 선언된 타입에 따라 올바르게 사용되었는지, 잘못된 타입의 값이 할당되거나 반환되는지 등을 검사한다.
- 타입스크립트를 사용하면서 편하다고 느끼는 타입 검사, 타입 추론, 자동 완성 등은 대부분 Checker에 의해 일어난다.

### Emitter
> transpile 과정: Checker에 의해 검증된 타입스크립트 코드를 기반으로 자바스크립트 코드로 변환되는 과정

- 자바스크립트에는 필요 없는 타입스크립트의 구문과 기능(type, interface, enum 등)을 나눈다.
- 필요에 의해 완전히 제거하기도하여 순수한 자바스크립트 코드로 변환된다.
