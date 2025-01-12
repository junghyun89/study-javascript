# 심볼형
자바스크립트는 객체 프로퍼티 키로 오직 문자형과 심볼형만을 허용합니다. 숫자형, 불린형 모두 불가능하고 오직 문자형과 심볼형만 가능하죠.

지금까지는 프로퍼티 키가 문자형인 경우만 살펴보았습니다. 이번 챕터에선 프로퍼티 키로 심볼값을 사용해 보면서, 심볼형 키를 사용할 때의 이점에 대해 살펴보도록 하겠습니다.

## 심볼
'심볼(symbol)'은 유일한 식별자(unique identifier)를 만들고 싶을 때 사용합니다.

`Symbol()`을 사용하면 심볼값을 만들 수 있습니다.

```js
// id는 새로운 심볼이 됩니다.
let id = Symbol();
```

심볼을 만들 때 심볼 이름이라 불리는 설명을 붙일 수도 있습니다. 심볼 이름은 디버깅 시 아주 유용합니다.

```js
// 심볼 id에는 "id"라는 설명이 붙습니다.
let id = Symbol("id");
```

심볼은 유일성이 보장되는 자료형이기 때문에, 설명이 동일한 심볼을 여러 개 만들어도 각 심볼값은 다릅니다. 심볼에 붙이는 설명(심볼 이름)은 어떤 것에도 영향을 주지 않는 이름표 역할만을 합니다.

설명이 같은 심볼 두 개를 만들고 이를 비교해보겠습니다. 동일 연산자(`==`)로 비교 시 `false`가 반환되는 것을 확인할 수 있습니다.

```js run
let id1 = Symbol("id");
let id2 = Symbol("id");

alert(id1 == id2); // false

```

참고로 Ruby 등의 언어에서도 '심볼'과 유사한 개념을 사용하는데, 자바스크립트의 심볼은 이들 언어에 쓰이는 심볼과는 다르기 때문에 혼동하지 마시길 바랍니다.

```
"심볼은 문자형으로 자동 형 변환되지 않습니다."

자바스크립트에선 문자형으로의 암시적 형 변환이 비교적 자유롭게 일어나는 편입니다. `alert` 함수가 거의 모든 값을 인자로 받을 수 있는 이유가 이 때문이죠. 그러나 심볼은 예외입니다. 심볼형 값은 다른 자료형으로 암시적 형 변환(자동 형 변환)되지 않습니다.
아래 예시에서 `alert`는 에러를 발생시킵니다.
```
```js run
let id = Symbol("id");

alert(id); // TypeError: Cannot convert a Symbol value to a string

```
문자열과 심볼은 근본이 다르기 때문에 우연히라도 서로의 타입으로 변환돼선 안 됩니다. 자바스크립트에선 '언어 차원의 보호장치(language guard)'를 마련해 심볼형이 다른 형으로 변환되지 않게 막아줍니다.

심볼을 반드시 출력해줘야 하는 상황이라면 아래와 같이 `.toString()` 메서드를 명시적으로 호출해주면 됩니다.
```js run
let id = Symbol("id");

alert(id.toString()); // Symbol(id)가 얼럿 창에 출력됨

```
`symbol.description` 프로퍼티를 이용하면 설명만 보여주는 것도 가능합니다.
```js run
let id = Symbol("id");

alert(id.description); // id

```

## '숨김' 프로퍼티
심볼을 이용하면 '숨김(hidden)' 프로퍼티를 만들 수 있습니다. 숨김 프로퍼티는 외부 코드에서 접근이 불가능하고 값도 덮어쓸 수 없는 프로퍼티입니다. 

서드파티 코드에서 가지고 온 `user`라는 객체가 여러 개 있고, `user`를 이용해 어떤 작업을 해야 하는 상황이라고 가정해 봅시다. `user`에 식별자를 붙여주도록 합시다.

식별자는 심볼을 이용해 만들도록 하겠습니다.

```js run
let user = { // 서드파티 코드에서 가져온 객체
  name: "John"
};
let id = Symbol("id");
user[id] = 1;
alert( user[id] ); // 심볼을 키로 사용해 데이터에 접근할 수 있습니다.
```

그런데 문자열 `"id"`를 키로 사용해도 되는데 `Symbol("id")`을 사용한 이유가 무엇일까요?

`user`는 서드파티 코드에서 가지고 온 객체이므로 함부로 새로운 프로퍼티를 추가할 수 없습니다. 그런데 심볼은 서드파티 코드에서 접근할 수 없기 때문에, 심볼을 사용하면 서드파티 코드가 모르게 `user`에 식별자를 부여할 수 있습니다.

상황 하나를 더 가정해보겠습니다. 제3의 스크립트(자바스크립트 라이브러리 등)에서 `user`를 식별해야 하는 상황이 벌어졌다고 해보죠. `user`의 원천인 서드파티 코드, 현재 작성 중인 스크립트, 제3의 스크립트가 각자 서로의 코드도 모른 채 `user`를 식별해야 하는 상황이 벌어졌습니다.

제3의 스크립트에선 아래와 같이 `Symbol("id")`을 이용해 전용 식별자를 만들어 사용할 수 있습니다.

```js
// ...
let id = Symbol("id");
user[id] = "제3 스크립트 id 값";
```

심볼은 유일성이 보장되므로 우리가 만든 식별자와 제3의 스크립트에서 만든 식별자가 충돌하지 않습니다. 이름이 같더라도 말이죠.

만약 심볼 대신 문자열 `"id"`를 사용해 식별자를 만들었다면 충돌이 발생할 **가능성이** 있습니다.

```js run
let user = { name: "John" };
// 문자열 "id"를 사용해 식별자를 만들었습니다.
user.id = "스크립트 id 값";
// 만약 제3의 스크립트가 우리 스크립트와 동일하게 문자열 "id"를 이용해 식별자를 만들었다면...
user.id = "제3 스크립트 id 값"
// 의도치 않게 값이 덮어 쓰여서 우리가 만든 식별자는 무의미해집니다.
```

### Symbols in a literal
객체 리터럴 `{...}`을 사용해 객체를 만든 경우, 대괄호를 사용해 심볼형 키를 만들어야 합니다.

예시:

```js
let id = Symbol("id");
let user = {
  name: "John",

  [id]: 123 // "id": 123은 안됨

};
```
`"id: 123"`이라고 하면, 심볼 `id`가 아니라 문자열 "id"가 키가 됩니다. 

### 심볼은 for..in 에서 배제됩니다
키가 심볼인 프로퍼티는 `for..in` 반복문에서 배제됩니다.

예시:

```js run
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

for (let key in user) alert(key); // name과 age만 출력되고, 심볼은 출력되지 않습니다.

// 심볼로 직접 접근하면 잘 작동합니다.
alert( "직접 접근한 값: " + user[id] );
```

`Object.keys(user)`에서도 키가 심볼인 프로퍼티는 배제됩니다. '심볼형 프로퍼티 숨기기(hiding symbolic property)'라 불리는 이런 원칙 덕분에 외부 스크립트나 라이브러리는 심볼형 키를 가진 프로퍼티에 접근하지 못합니다.

그런데 [Object.assign](mdn:js/Object/assign)은 키가 심볼인 프로퍼티를 배제하지 않고 객체 내 모든 프로퍼티를 복사합니다.

```js run
let id = Symbol("id");
let user = {
  [id]: 123
};
let clone = Object.assign({}, user);
alert( clone[id] ); // 123
```

뭔가 모순이 있는 것 같아 보이지만, 이는 의도적으로 설계된 것입니다. 객체를 복사하거나 병합할 때, 대개 `id` 같은 심볼을 포함한 프로퍼티 **전부**를 사용하고 싶어 할 것이라는 생각에서 이렇게 설계되었습니다.

## 전역 심볼
앞서 살펴본 것처럼, 심볼은 이름이 같더라도 모두 별개로 취급됩니다. 그런데 이름이 같은 심볼이 같은 개체를 가리키길 원하는 경우도 가끔 있습니다. 애플리케이션 곳곳에서 심볼 `"id"`를 이용해 특정 프로퍼티에 접근해야 한다고 가정해 봅시다.

**전역 심볼 레지스트리(global symbol registry)** 는 이런 경우를 위해 만들어졌습니다. 전역 심볼 레지스트리 안에 심볼을 만들고 해당 심볼에 접근하면, 이름이 같은 경우 항상 동일한 심볼을 반환해줍니다.

레지스트리 안에 있는 심볼을 읽거나, 새로운 심볼을 생성하려면 `Symbol.for(key)`를 사용하면 됩니다.

이 메서드를 호출하면 이름이 `key`인 심볼을 반환합니다. 조건에 맞는 심볼이 레지스트리 안에 없으면 새로운 심볼 `Symbol(key)`을 만들고 레지스트리 안에 저장합니다.

예시:

```js run
// 전역 레지스트리에서 심볼을 읽습니다.
let id = Symbol.for("id"); // 심볼이 존재하지 않으면 새로운 심볼을 만듭니다.
// 동일한 이름을 이용해 심볼을 다시 읽습니다(좀 더 멀리 떨어진 코드에서도 가능합니다).
let idAgain = Symbol.for("id");
// 두 심볼은 같습니다.
alert( id === idAgain ); // true
```

전역 심볼 레지스트리 안에 있는 심볼은 **전역 심볼**이라고 불립니다. 애플리케이션에서 광범위하게 사용해야 하는 심볼이라면 전역 심볼을 사용하세요.

```
"Ruby랑 비슷해 보이네요."

Ruby 등의 몇몇 언어에선 이름이 같으면 심볼도 같습니다.
자바스크립트에선 전역 심볼에만 이런 특징이 적용됩니다.
```

### Symbol.keyFor
전역 심볼을 찾을 때 사용되는 `Symbol.for(key)`에 반대되는 메서드도 있습니다. `Symbol.keyFor(sym)`를 사용하면 이름을 얻을 수 있습니다.

예시:

```js run
// 이름을 이용해 심볼을 찾음
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");
// 심볼을 이용해 이름을 얻음
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
```

`Symbol.keyFor`는 전역 심볼 레지스트리를 뒤져서 해당 심볼의 이름을 얻어냅니다. 검색 범위가 전역 심볼 레지스트리이기 때문에 전역 심볼이 아닌 심볼에는 사용할 수 없습니다. 전역 심볼이 아닌 인자가 넘어오면 `Symbol.keyFor`는 `undefined`를 반환합니다.

전역 심볼이 아닌 모든 심볼은 `description` 프로퍼티가 있습니다. 일반 심볼에서 이름을 얻고 싶으면 `description` 프로퍼티를 사용하면 됩니다.

예시:

```js run
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");
alert( Symbol.keyFor(globalSymbol) ); // name, 전역 심볼
alert( Symbol.keyFor(localSymbol) ); // undefined, 전역 심볼이 아님
alert( localSymbol.description ); // name
```

## 시스템 심볼
'시스템 심볼(system symbol)'은 자바스크립트 내부에서 사용되는 심볼입니다. 시스템 심볼을 활용하면 객체를 미세 조정할 수 있습니다.

명세서 내의 표, [잘 알려진 심볼(well-known symbols)](https://tc39.github.io/ecma262/#sec-well-known-symbols)에서 어떤 시스템 심볼이 있는지 살펴보세요.

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`
- 기타 등등

객체가 어떻게 원시형으로 변환되는지 알기 위해선 `Symbol.toPrimitive`에 대해 알아야 하는데, 자세한 내용은 곧 다루도록 하겠습니다.

시스템 심볼 각각에 대한 내용은 연관되는 자바스크립트 기능을 학습하면서 알아보겠습니다.

## 요약
`Symbol`은 원시형 데이터로, 유일무이한 식별자를 만드는 데 사용됩니다.

`Symbol()`을 호출하면 심볼을 만들 수 있습니다. 설명(이름)은 선택적으로 추가할 수 있습니다.

심볼은 이름이 같더라도 값이 항상 다릅니다. 이름이 같을 때 값도 같길 원한다면 전역 레지스트리를 사용해야 합니다. `Symbol.for(key)`는 `key`라는 이름을 가진 전역 심볼을 반환합니다. `key`라는 이름을 가진 전역 심볼이 없으면 새로운 전역 심볼을 만들어줍니다. `key`가 같다면 `Symbol.for`는 어디서 호출하든 상관없이 항상 같은 심볼을 반환해 줍니다.

심볼의 주요 유스 케이스는 다음과 같습니다.

1. 객체의 '숨김' 프로퍼티 -- 
    외부 스크립트나 라이브러리에 '속한' 객체에 새로운 프로퍼티를 추가해 주고 싶다면 심볼을 만들고, 이를 프로퍼티 키로 사용하면 됩니다. 키가 심볼인 경우엔 `for..in`의 대상이 되지 않아서 의도치 않게 프로퍼티가 수정되는 것을 예방할 수 있습니다. 외부 스크립트나 라이브러리는 심볼 정보를 갖고 있지 않아서 프로퍼티에 직접 접근하는 것도 불가능합니다. 심볼형 키를 사용하면 프로퍼티가 우연히라도 사용되거나 덮어씌워 지는 걸 예방할 수 있습니다.

    이런 특징을 이용하면 원하는 것을 객체 안에 '은밀하게' 숨길 수 있습니다. 외부 스크립트에선 우리가 숨긴 것을 절대 볼 수 없습니다.

2. 자바스크립트 내부에서 사용되는 시스템 심볼은 `Symbol.*`로 접근할 수 있습니다. 시스템 심볼을 이용하면 내장 메서드 등의 기본 동작을 입맛대로 변경할 수 있습니다. [iterable 객체](info:iterable)에선 `Symbol.iterator`를, [객체를 원시형으로 변환하기](info:object-toprimitive)에선 `Symbol.toPrimitive`이 어떻게 사용되는지 알아보겠습니다.

사실 심볼을 완전히 숨길 방법은 없습니다. 내장 메서드 [Object.getOwnPropertySymbols(obj)](mdn:js/Object/getOwnPropertySymbols)를 사용하면 모든 심볼을 볼 수 있고, 메서드 [Reflect.ownKeys(obj)](mdn:js/Reflect/ownKeys)는 심볼형 키를 포함한 객체의 **모든** 키를 반환해줍니다. 그런데 대부분의 라이브러리, 내장 함수 등은 이런 메서드를 사용하지 않습니다.
