# 원시값의 메서드
자바스크립트는 원시값(문자열, 숫자 등)을 마치 객체처럼 다룰 수 있게 해줍니다. 원시값에도 객체에서처럼 메서드를 호출할 수 있죠. 원시값의 메서드에 대해선 곧 학습할 예정인데 그 전에, 원시값은 객체가 아니란 것을 상기하도록 합시다.

원시값과 객체는 다음과 같은 차이점이 있습니다.

원시값:

- 원시형 값입니다.
- 원시형의 종류는 `문자(string)`, `숫자(number)`, `bigint`, `불린(boolean)`, `심볼(symbol)`, `null`, `undefined`형으로 총 일곱 가지 입니다.

객체:

- 프로퍼티에 다양한 종류의 값을 저장할 수 있습니다.
- `{name : "John", age : 30}`와 같이 대괄호 `{}`를 사용해 만들 수 있습니다. 자바스크립트에는 여러 종류의 객체가 있는데, 함수도 객체의 일종입니다.

객체의 장점 중 하나는 함수를 프로퍼티로 저장할 수 있다는 것입니다.

```js run
let john = {
  name: "John",
  sayHi: function() {
    alert("친구야 반갑다!");
  }
};
john.sayHi(); // 친구야 반갑다!
```

객체 `john`을 만들고, 거기에 메서드 `sayHi`를 정의해보았습니다.

자바스크립트는 날짜, 오류, HTML 요소(HTML element) 등을 다룰 수 있게 해주는 다양한 내장 객체를 제공합니다. 이 객체들은 고유한 프로퍼티와 메서드를 가집니다.

하지만, 이런 기능을 사용하면 시스템 자원이 많이 소모된다는 단점이 있습니다.

객체는 원시값보다 "무겁고", 내부 구조를 유지하기 위해 추가 자원을 사용하기 때문입니다.

## 원시값을 객체처럼 사용하기
자바스크립트 창안자(creator)는 다음과 같은 모순적인 상황을 해결해야만 했었습니다.

- 문자열이나 숫자와 같은 원시값을 다루어야 하는 작업이 많은데, 메서드를 사용하면 작업을 수월하게 할 수 있을 것 같다는 생각이 듭니다.
- 그런데 원시값은 가능한 한 빠르고 가벼워야 합니다.

조금 어색해 보이지만, 자바스크립트 창안자는 아래와 같은 방법을 사용해 해결책을 모색하였습니다.

1. 원시값은 원시값 그대로 남겨둬 단일 값 형태를 유지합니다.
2. 문자열, 숫자, 불린, 심볼의 메서드와 프로퍼티에 접근할 수 있도록 언어 차원에서 허용합니다.
3. 이를 가능하게 하기 위해, 원시값이 메서드나 프로퍼티에 접근하려 하면 추가 기능을 제공해주는 특수한 객체, "원시 래퍼 객체(object wrapper)"를 만들어 줍니다. 이 객체는 곧 삭제됩니다.

"래퍼 객체"는 원시 타입에 따라 종류가 다양합니다. 각 래퍼 객체는 원시 자료형의 이름을 그대로 차용해, `String`,`Number`,`Boolean`, `Symbol`라고 부릅니다. 래퍼 객체 마다 제공하는 메서드 역시 다릅니다.

인수로 받은 문자열의 모든 글자를 대문자로 바꿔주는 메서드 [str.toUpperCase()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)를 예로 들어보겠습니다.

메서드는 아래와 같이 동작합니다.

```js run
let str = "Hello";
alert( str.toUpperCase() ); // HELLO
```

간단하죠? 아래는 `str.toUpperCase ()`가 호출될 때 내부에서 실제로 일어나는 일입니다.

1. 문자열 `str`은 원시값이므로 원시값의 프로퍼티(toUpperCase)에 접근하는 순간 특별한 객체가 만들어집니다. 이 객체는 문자열의 값을 알고 있고, `toUpperCase()`와 같은 유용한 메서드를 가지고 있습니다.
2. 메서드가 실행되고, 새로운 문자열이 반환됩니다(`alert` 창에 이 문자열이 출력됩니다).
3. 특별한 객체는 파괴되고, 원시값 `str`만 남습니다.

이런 내부 프로세스를 통해 원시값을 가볍게 유지하면서 메서드를 호출할 수 있는 것입니다.

자바스크립트 엔진은 위 프로세스의 최적화에 많은 신경을 씁니다. 원시 래퍼 객체를 만들지 않고도 마치 원시 래퍼 객체를 생성(명세에 언급됨)한 것처럼 동작하게끔 해주죠. 

숫자형도 고유한 메서드를 지원합니다. 메서드 [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed)를 이용하면 원하는 자리에서 소수점 아래 숫자를 반올림할 수 있습니다. 

```js run
let n = 1.23456;
alert( n.toFixed(2) ); // 1.23
```

<info:number>, <info:string>에서 더 많은 메서드에 대해 알아보겠습니다.


```
"`String/Number/Boolean`를 생성자론 쓰지 맙시다."

Java 등의 몇몇 언어에선 `new Number(1)` 또는 `new Boolean(false)`와 같은 문법을 사용해 원하는 타입의 "래퍼 객체"를 직접 만들 수 있습니다. 
자바스크립트에서도 하위 호환성을 위해 이 기능을 남겨 두었는데, 이런 식으로 래퍼 객체를 만드는 건 추천하지 않습니다. 몇몇 상황에서 혼동을 불러일으키기 때문입니다.
```
예시:
```js run
alert( typeof 0 ); // "number"
alert( typeof new Number(0) ); // "object"!
```
객체는 논리 평가 시 항상 참을 반환하기 때문에, 아래 예시에서 얼럿창은 무조건 열립니다.
```js run
let zero = new Number(0);
if (zero) { // 변수 zero는 객체이므로, 조건문이 참이 됩니다.
  alert( "그런데 여러분은 zero가 참이라는 것에 동의하시나요!?!" );
}
```
그런데, `new`를 붙이지 않고 `String / Number / Boolean`을 사용하는 건 괜찮습니다. `new` 없이 사용하면 상식에 맞게 인수를 원하는 형의 원시값(문자열, 숫자, 불린 값)으로 바꿔줍니다. 아주 유용하죠.
예시:
```js
let num = Number("123"); // 문자열을 숫자로 바꿔줌
```

```
"`null/undefined`는 메서드가 없습니다."

특수 자료형인 `null`과 `undefined`의 원시값(`null/undefined`)은 위와 같은 법칙을 따르지 않습니다. 이 자료형과 연관되는 "래퍼 객체"도 없고, 메서드도 제공하지 않습니다. 어떤 의미에서는 두 자료형이 "가장 원시적"이라 할 수 있을 것 같습니다.
두 자료형에 속한 값의 프로퍼티에 접근하려 하면 에러가 발생합니다.
```
```js run
alert(null.test); // error
```

## Summary
- 'null'과 'undefined'를 제외한 원시값에 다양한 메서드를 호출할 수 있습니다. 이에 대해선 별도의 챕터에서 곧 알아보도록 하겠습니다.
- 원시값에 메서드를 호출하려 하면 임시 객체가 만들어집니다. 그런데 자바스크립트 엔진은 내부 최적화가 잘 되어있어 메서드를 호출해도 많은 리소스를 쓰지 않습니다.
