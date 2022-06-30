# Lexical Environment

## Exclusion Context?

- JavaScript Code가 실행되기 위해 필요한 정보들을 추상화 하기 위해 만들어진 객체이다
    
    ![스크린샷 2022-06-24 오전 11.50.36.png](Lexical%20Environment%20a5c0358e53494c94a94326450bbcc502/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-06-24_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_11.50.36.png)
    

  

## Exclution Context가 만들어지는 Case

- Global Execution Context (GEC)
- Functional Execution Context (FEC)
- Eval Function Execution Context (EFEC)

## ****Execution Stack****

- LIFO (Last In, FIrst out) 자료 구조를 사용하며 실행 컨텍스트들이 LIFO 자료 구조 상태로 저장 됩니다.
- Javascript Engine이 <Script> 태그를 만나게 되면 GEO를 생성 후 현재 실행 되고 해당 Stack에 push 합니다.
- Javascript Call Stack은 최대 Stack Size가 정해져있기 때문에 Call Stack에 쌓인 Context Stack이 최대치를 넘게 될 경우 ‘RangeError: Maximum call stack size exceeded’라는 에러가 발생합니다. 이에러는 Stack Overflow라고 부르기도 합니다.

```jsx
var x = 'xxx';

function foo () {
  var y = 'yyy';

  function bar () {
    var z = 'zzz';
    console.log(x + y + z);
  }
  
  bar();
}

foo();
```

![Untitled](Lexical%20Environment%20a5c0358e53494c94a94326450bbcc502/Untitled.png)

![1_bDebsOuhRx9NMyvLHY2zxA.gif](Lexical%20Environment%20a5c0358e53494c94a94326450bbcc502/1_bDebsOuhRx9NMyvLHY2zxA.gif)

## ****Global Execution Context (GEC)****

- JavaScript engine이 script 파일을 받으면 가장 처음으로 GEC가 생성 됩니다.
- GEC는 어떠한 함수나 객체에도 속하지 않으며 전역공간에 놓인 모든 코드들이 속하는 EC입니다.
- 모든 JavaScript 파일에는 하나의 GEC만 있을 수 있습니다. (싱글 스레드 기반)
- window object를 생성합니다.
- this를 global object로 할당합니다.

## Functional Execution Context (FEC)

- 함수가 실행될 때마다 완전히 새로운 FEC가 생성 됩니다.
- 모든 함수 호출에는 고유한 FEC가 있으므로 스크립트의 런타임에 둘 이상의 FEC가 있을 수 있습니다.

## Eval Function Execution Context (EFEC)

![Untitled](Lexical%20Environment%20a5c0358e53494c94a94326450bbcc502/Untitled%201.png)

## ****Creation Phase****

- JavaScript Engine이 함수를 호출했지만 실행이 시작되지 않은 단계입니다.
- Creation Phase에서 JS engine은 compile phase에 있으며 code를 compile하기 위해 함수 코드를 스캔하고 코드를 실행하지 않습니다
- Lexical Environment 구성 요소 생성
- Variable Environment 구성 요소 생성

```jsx
ExecutionContext ={
	LexicalEnvironment = <ref. to LexicalEnvironment in memory>,
	VariableEnvironment = <ref. to VariableEnvironment in memory>,
}
```

## Lexical Environment

**식별자가 선언되는 환경을 의미함. (identifier-variable가 mapping 됨)**

```jsx
var a = 20;
var b = 40;

function foo(){
	console.log('bar');
}

lexicalEnvironment = {
  a : 20,
  b : 40,
  foo : <ref. to foo function>
}
```

identifer: 함수와 변수의 이름과 같이 어떤 대상을 다른 대상과 구분하여 식별할 수 있게 해줌

- environmentRecord
- outer-EnvironmentReference

### EnvironmentRecord

현재 컨텍스트와 관련된 코드의 식별자 정보들이 저장됨

- 매개변수 식별자
- 함수 자체
- 함수 내부의 식별자

현재 실행될 컨텍스트의 대상 코드 내에 어떤 식별자들이 있는지만을 먼저 수집함

변수를 인식할 때 식별자만 끌어올리고 할당 과정은 원래 자리에 순서대로 남겨둠 (호이스팅)

### ****Hoisting****

- Javascript Engine은 identier들을 최상단으로 끌어올려놓은 후에 실제 코드를 실행함
- Javascript Engine이 실제로 변수를 끌어올리지는 않지만, 편의상 끌어올리는 것으로 간주함

### Variable-Hoisting

```jsx
function a () {
  var x = 1; // 매개변수 할당
  console.log(x);
  var x ;
  console.log(x);
  var x = 2;
  console.log(x);
}
a();
```

```jsx
호이스팅 발생 후...

function a () {
  var x;
  var x;
  var x;

  x = 1;
  console.log(x); // 1
  console.log(x); // 1
  x = 2;
  console.log(x); // 2
}

a();

정의부만 호이스팅 됩니다.
```

### Function-Hoisting

```jsx
function a () {
  console.log(b);
  var b = 'bbb';
  console.log(b);
  function b () {};
  console.log(b);
}

a();
```

```jsx
호이스팅 발생 후...

function a () {
  var b;
  function b () {};

  console.log(b); // f b () {}
  b = 'bbb';
  console.log(b); // bbb
  console.log(b); // bbb
}

a();

함수 전체가 호이스팅 됩니다.
```

```jsx
console.log(iceVar)
console.log(letItGo);
console.log(constantine);

var iceVar = 1;
let letItGo = 2;
const constantine = 3;
```

### ****Temporal Dead Zone****

- 선언은 되어있지만 아직 초기화가 되지않아 변수에 담길 값을 위한 공간이 메모리에 할당되지 않은 상태
- var ⇒ 선언, 초기화 후 할당
- let ⇒ 선언, 초기화, 할당이 따로 이루어짐
- const => 선언, 초기화, 할당이 동시에 이루어짐

선언 (Declaration): 스코프와 변수 객체가 생성되고 스코프가 변수 객체를 참조함

초기화(Initalization): 변수 객체가 가질 값을 위해 메모리에 공간을 할당한다. (초기화되는 값은 undefined)

할당(Assignment):변수 객체에 값을 할당한다.

### Outer-EnvironmentReference

- 현재 호출된 함수가 선언된 컨텍스트의 lexical environment에 대한 참조를 가짐 ⇒ 해당 함수가 선언된 시점의 lexical environment를 참조함
- 중첩된 자바스크립트 코드에서 스코프 탐색을 하기 위해 사용

```jsx
function foo() {
  var a = 1;
  var b = 2;
  function bar() {
    console.log(a);
  }
  return bar;
}

var bar = foo();
bar(); // 1 출력
```

bar함수의 outer environment reference는 foo 함수의 lexical environment를 참조중입니다.

bar 함수에서 a라는 식별자가 없음에도, outer environment reference를 통해 foo의 a 변수에 접근할 수 있습니다

```jsx
function foo() {
  const a = 1;
  const b = 2;
  const c = 3;
  function bar() {}

  // 2. Running execution context

  // ...
}

foo(); // 1. Call

// Running execution context의 LexicalEnvironment

{
  environmentRecord: {
    a: 1,
    b: 2,
    c: 3,
    bar: <Function>
  },
  outer: foo.[[Environment]]
}
```

## ****Declarative Environment Record****

- ECMAScript 언어로 표현되는 값들과 식별자들(var, const, let, class, module, import, function)이 스코프 내에서 선언된 경우의 바인딩을 관리
- Environment Record를 상속한 서브클래스임
    
    ### Function Environment Record
    
    - 함수가 호출되면 만들어집니다.
    - 함수 안에 있는 변수와 함수들을 관리합니다.
        
        ### This Binding
        
        - this로 지정된 객체가 저장됨
        
        ### ****Global Execution Context 일 때****
        
        - 전역 객체를 나타냄 브라우저라면  window-object를 나타냄
        
        ### Functional Execution Context 일 때
        
        - 함수가 실행 될 때 동적으로 바인딩 된다.
        - 함수 단위로 Exclution Context를 만든 후 this 의 바인딩이 됨
        
        ### Function Object
        
        - 엔진의 코드 해석 단계에서 var test = function()이라는 문장을 만나면 built-in Function Object의 prototype에 연결된 메소드로 Function Object를 생성
    
    ### Module Environment Record
    
    - 다른 환경 레코드에 존재하는 바인딩에 간접적으로 접근할 수 있는 불변 import 바인딩을 제공
    - 

## ****Global Environment Record****

- 최상위 스코프에 선언된 식별자와 전역 객체를 바인딩 함.
- built-in object, Object Environment Record의 properties 그리고 Global Scope 에서의 선언에 대한 바인딩을 관리

## Object Environment Record

- 각 Object Environment Record는 Binding Object 라는 Object와 연결됩니다. (window도 당연히 연결됩니다)
- Global Environment Record에서 추가적으로 저장 되며 전역 환경에서 제공되는 객체들이 저장됨

## Execution Phase

- Creation Phase에서 코드 실행을 위한 환경 정보 값이 결정되었다면 모든 식별자에 대한 할당이 완료 되고 코드가 최종적으로 실행 됨
- Javascript Engine이 다시 함수를 검색하여 변수 개체를 변수 값으로 업데이트하고 코드를 실행합니다.
- 

참고 자료

[https://www.freecodecamp.org/news/execution-context-how-javascript-works-behind-the-scenes/](https://www.freecodecamp.org/news/execution-context-how-javascript-works-behind-the-scenes/)

[https://reese-dev.netlify.app/javascript/execution-context/](https://reese-dev.netlify.app/javascript/execution-context/)

[https://books.google.co.kr/books?id=JQ1sEAAAQBAJ&pg=PA370&lpg=PA370&dq="Global+Environment+Record"&source=bl&ots=jzJFAJH8iK&sig=ACfU3U00WGgvmpVb4SrBfq8ZD1stVSyqMw&hl=ko&sa=X&ved=2ahUKEwjM2czTx7z4AhUAxosBHc32CkMQ6AF6BAglEAM#v=onepage&q="Global Environment Record"&f=false](https://books.google.co.kr/books?id=JQ1sEAAAQBAJ&pg=PA370&lpg=PA370&dq=%22Global+Environment+Record%22&source=bl&ots=jzJFAJH8iK&sig=ACfU3U00WGgvmpVb4SrBfq8ZD1stVSyqMw&hl=ko&sa=X&ved=2ahUKEwjM2czTx7z4AhUAxosBHc32CkMQ6AF6BAglEAM#v=onepage&q=%22Global%20Environment%20Record%22&f=false)

[https://velog.io/@marulloc/8-Lexical-Env와-Function-Object](https://velog.io/@marulloc/8-Lexical-Env%EC%99%80-Function-Object)

[https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0?gi=c88d694aad3f](https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0?gi=c88d694aad3f)
