## 호이스팅(hoisting)

javascript에서 호이스팅(hoisting) 이란, 인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미함.
var로 선언한 변수의 경우 호이스팅 시 undefined로 변수를 초기화 함. 반면 let, const로 선언한 변수의 경우 호이스팅 시 변수를 초기화 하지 않음.

호이스팅을 설명할 땐 주로 "변수의 선언과 초기화를 분리한 후, 선언만 코드의 최상단으로 옮기는 것".
따라서 변수를 정의하는 코드보다 사용하는 코드가 앞서 등장할 수 있음. 다만 선언과 초기화를 함께 수행하는 경우, 선언코드까지 실행해야 변수가 초기화된 상태가 됨을 주의.

※ 호이스팅은(let, const 포함) ECMAScript 2015 언어 명세 이전의 표준 명세에는 나타나지 않음.
당시에는 호이스팅이 Javascript에서 실행 맥락, 특히 생성 및 실행단계의 동작 방식을 설명하는 일반적인 방법이였음.

### hoisting 대상

Javascript는 함수의 코드를 실행하기 전에 함수 선언에 대한 메모리부터 할당함.
덕분에 함수를 호출하는 코드를 함수 선언보다 앞서 배치할 수 있음.

```js
dogName("왈프");
function dogName(name) {
  console.log("제 강아지 이름은 " + name + "입니다");
}
/*
결과: "제 강아지 이름은 왈프입니다"
*/
```

```js
console.log(num); // 호이스팅한 var 선언으로 인해 undefined 출력
var num; // 선언
num = 6; // 초기화
/*
결과: 6
*/
```

```js
console.log(num2); // ReferenceError
num2 = 6; // 초기화
/*
결과: 6
*/
```

```js
// y만 호이스팅 대상

x = 1; // x 초기화. x를 선언하지 않은 경우 선언. 그러나 명령문에 var가 없으므로 호이스팅이 발생하지 않음
console.log(x + " " + y);
// JavaScript는 선언만 호이스팅하므로, 윗줄의 y는 undefined
var y = 2; // y를 선언하고 초기화
/*
결과: 1 undefined
*/
```

```js
// 호이스팅은 없지만, 변수 초기화는 (아직 하지 않은 경우) 변수 선언까지 병행하므로 변수를 사용할 수 있음
a = "크랜"; // a 초기화
b = "베리"; // b 초기화

console.log(a + "" + b);
/*
결과: 크랜베리
*/
```

## let과 const 호이스팅

`let`과 `const`로 선언한 변수도 호이스팅 대상이지만, `var`와 달리 호이스팅 시 undefined로 변수를 초기화 하지는 않음.
따라서 변수의 포기화를 수행하기 전에 읽는 코드가 먼저 나타나면 예외가 발생함

```js
console.log(v); // v is not defined
let v = 2;
```

## let

`let` 명령문은 블록 스코프의 범위를 가지는 지역 변수를 선언하며, 선언과 동시에 임의의 값으로 초기화 할 수 있음.

```js
let x = 1;

if (x === 1) {
  let x = 2;

  console.log(x);
  // expected output: 2
}

console.log(x);
// expected output: 1
```

### 스코프 규칙

`let`으로 선언한 변수는 자신을 선언한 블록과 하위 블록을 스스로의 스코프로 가짐. 이런 점에서는 `let`, `var` 둘은 유사함.
그러나 둘의 중요한 차이는 `var`의 경우 스코프가 '자신을 선언한 블록'이 아니라, 자신의 선언을 포함하는 함수라는 점..

```js
function varTest() {
  var x = 1;
  if (true) {
    var x = 2; // 같은 변수!
    console.log(x); // 2
  }
  console.log(x); // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2; // 다른 변수
    console.log(x); // 2
  }
  console.log(x); // 1
}
```

프로그램 최상위에서 사용할 경우 `var`는 전역 객체에 속성을 추가하지만 `let`은 추가하지 않음.

```js
var x = "global";
let y = "global";
console.log(this.x); // "global"
console.log(this.y); // undefined
```

### 비공개 멤버

생성자와 `let`을 함께 사용하면 클로저를 사용하지 않아도 비공개 멤버를 나타낼 수 있음.

[TODO](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let#%EB%B9%84%EA%B3%B5%EA%B0%9C_%EB%A9%A4%EB%B2%84_%EB%AA%A8%EC%82%AC)
