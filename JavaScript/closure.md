# 클로저

클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다. 클로저를 이해하려면 자바스크립트가 어떻게 변수의 유효범위를 지정하는지(Lexical scoping)를 먼저 이해해야함.

## 어휘적 범위 지정(Lexical scoping)

```js
function init() {
  var name = "Mozilla"; // name은 init에 의해 생성된 지역 변수이다.
  function displayName() {
    // displayName() 은 내부 함수이며, 클로저다.
    alert(name); // 부모 함수에서 선언된 변수를 사용한다.
  }
  displayName();
}
init();
```

`init()`은 지역 변수 `name`과 함수 `displayName()`을 생성한다. `displayName()`은 `init()`안에 정의된 내부함수이며 `init()`함수 본문에서만 사용할 수 있다.

여기서 주의할 점은 `displayName()` 내부엔 자신만의 지역변수가 없다는 점이다. 그런데 함수 내부에서 외부함수의 변수에 접근할 수 있기 때문에 `displayName()` 역시 부모 함수 `init()`에서 선언된 변수 `name`에 접근 할 수 있다.

만약 `displayName()`가 자신만의 `name`변수를 가지고 있었다면, `name`대신 `this.name`을 사용했을 것이다.

위 코드를 실행하면 `displayName()` 함수 내의 `alert()` 문이 부모 함수에서 정의한 변수 `name`의 값을 성공적으로 출력한다. 이 예시를 통해 함수가 중첩된 상황에서 파서가 어떻게 변수를 처리하는지 알 수 있다. 이는 어휘적 범위 지정(lexical scoping)의 한 예이다. 여기서 "lexical"이란, 어휘적 범위 지정(lexical scoping) 과정에서 변수가 어디에서 사용 가능한지 알기 위해 그 변수가 소스코드 내 어디에서 선언되었는지 고려한다는 것을 의미한다.
단어 "lexical"은 이런 사실을 나타낸다. 중첩된 함수는 외부 범위(scope)에서 선언한 변수에도 접근할 수 있다.

## 클로저(Closure)

```js
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    alert(name);
  }
  return displayName;
}

var myFunc = makeFunc();
//myFunc변수에 displayName을 리턴함
//유효범위의 어휘적 환경을 유지
myFunc();
//리턴된 displayName 함수를 실행(name 변수에 접근)
```

이 코드는 바로 전의 예제와 완전히 동일한 결과가 실행된다. 하지만 흥미로운 차이는 displayName()함수가 실행되기 전에 외부함수인 makeFunc()로부터 리턴되어 myFunc 변수에 저장된다는 것이다.

한 눈에 봐서는 이 코드가 여전히 작동하는 것이 직관적으로 보이지 않을 수 있다. 몇몇 프로그래밍 언어에서, 함수 안의 지역 변수들은 그 함수가 처리되는 동안에만 존재한다. makeFunc() 실행이 끝나면(displayName함수가 리턴되고 나면) name 변수에 더 이상 접근할 수 없게 될 것으로 예상하는 것이 일반적이다.

하지만 위의 예시와 자바스크립트의 경우는 다르다. 그 이유는 자바스크립트는 함수를 리턴하고, 리턴하는 함수가 클로저를 형성하기 때문이다. 클로저는 함수와 함수가 선언된 어휘적 환경의 조합이다. 이 환경은 클로저가 생성된 시점의 유효 범위 내에 있는 모든 지역 변수로 구성된다. 첫 번째 예시의 경우, myFunc은 makeFunc이 실행 될 때 생성된 displayName 함수의 인스턴스에 대한 참조다. displayName의 인스턴스는 변수 name 이 있는 어휘적 환경에 대한 참조를 유지한다. 이런 이유로 myFunc가 호출될 때 변수 name은 사용할 수 있는 상태로 남게 되고 "Mozilla" 가 alert 에 전달된다.

다음은 조금 더 흥미로운 예제인 makeAdder 함수이다:

```js
function makeAdder(x) {
  var y = 1;
  return function (z) {
    y = 100;
    return x + y + z;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);
//클로저에 x와 y의 환경이 저장됨

console.log(add5(2)); // 107 (x:5 + y:100 + z:2)
console.log(add10(2)); // 112 (x:10 + y:100 + z:2)
//함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산
```

이 예제에서 단일 인자 x를 받아서 새 함수를 반환하는 함수 makeAdder(x)를 정의했다. 반환되는 함수는 단일인자 z를 받아서 x와 y와 z의 합을 반환한다.

본질적으로 makeAdder는 함수를 만들어내는 공장이다. 이는 makeAdder함수가 특정한 값을 인자로 가질 수 있는 함수들을 리턴한다는 것을 의미한다. 위의 예제에서 add5, add10 두 개의 새로운 함수들을 만들기 위해 makeAdder함수 공장을 사용했다. 하나는 매개변수 x에 5를 더하고 다른 하나는 매개변수 x에 10을 더한다.

add5와 add10은 둘 다 클로저이다. 이들은 같은 함수 본문 정의를 공유하지만 서로 다른 맥락(어휘)적 환경을 저장한다. 함수 실행 시 add5의 맥락적 환경에서 클로저 내부의 x는 5 이지만 add10의 맥락적 환경에서 x는 10이다. 또한 리턴되는 함수에서 초기값이 1로 할당된 y에 접근하여 y값을 100으로 변경한 것을 볼 수 있다. (물론 x값도 동일하게 변경 가능하다.) 이는 클로저가 리턴된 후에도 외부함수의 변수들에 접근 가능하다는 것을 보여주는 포인트이며 클로저에 단순히 값 형태로 전달되는것이 아니라는 것을 의미한다.

## 실용적인 클로저

클로저는 어떤 데이터(어휘적 환경)와 그 데이터를 조작하는 함수를 연관시켜주기 때문에 유용하다. 이것은 객체가 어떤 데이터와(그 객체의 속성) 하나 혹은 그 이상의 메소드들을 연관시킨다는 점에서 객체지향 프로그래밍과 분명히 같은 맥락에 있다.

결론적으로 오직 하나의 메소드를 가지고 있는 객체를 일반적으로 사용하는 모든 곳에 클로저를 사용할 수 있다.

이렇게 할 수 있는 상황은 특히 웹에서 일반적이다. 프론트 엔드 자바스크립트에서 우리가 쓰는 많은 코드가 이벤트 기반이다. 우리는 몇 가지 동작을 정의한 다음 사용자에 의한 이벤트에(클릭 혹은 키 누르기 같은) 연결한다. 우리의 코드는 일반적으로 콜백으로 첨부된다: 이벤트에 응답하여 실행되는 단일 함수다.

예를 들면 페이지의 글자 크기를 조정하는 몇 개의 버튼을 추가한다고 가정하자. 이 작업을 수행하는 한 가지 방법은 body 요소의 font-size를 픽셀 단위로 지정하고 상대적인 em 단위를 사용하여 페이지의 다른 요소들의 (예: 헤더) 크기를 설정하는 것이다.

```js
function makeSizer(size) {
  return function () {
    document.body.style.fontSize = size + "px";
  };
}

var size12 = makeSizer(12);
var size14 = makeSizer(14);
var size16 = makeSizer(16);

document.getElementById("size-12").onclick = size12;
document.getElementById("size-14").onclick = size14;
document.getElementById("size-16").onclick = size16;
```

## 클로저를 이용해서 프라이빗 메소드 (private method) 흉내내기

자바와 같은 몇몇 언어들은 메소드를 프라이빗으로 선언할 수 있는 기능을 제공한다. 이는 같은 클래스 내부의 다른 메소드에서만 그 메소드들을 호출할 수 있다는 의미이다.

자바스크립트는 태생적으로는 이런 방법을 제공하지 않지만 클로저를 이용하여 프라이빗 메소드를 흉내내는 것이 가능하다. 프라이빗 메소드는 코드에 제한적인 접근만을 허용한다는 점 뿐만 아니라 전역 네임 스페이스를 관리하는 강력한 방법을 제공하여 불필요한 메소드가 공용 인터페이스를 혼란스럽게 만들지 않도록 한다.

아래 코드는 프라이빗 함수와 변수에 접근하는 퍼블릭 함수를 정의하기 위해 클로저를 사용하는 방법을 보여준다. 이렇게 클로저를 사용하는 것을 모듈 패턴이라 한다.

```js
var counter = (function () {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function () {
      changeBy(1);
    },
    decrement: function () {
      changeBy(-1);
    },
    value: function () {
      return privateCounter;
    },
  };
})();

console.log(counter.value()); // logs 0
counter.increment();
counter.increment();
console.log(counter.value()); // logs 2
counter.decrement();
console.log(counter.value()); // logs 1
```

...
[참고사이트](https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures)
