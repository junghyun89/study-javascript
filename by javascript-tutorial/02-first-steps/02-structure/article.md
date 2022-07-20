# 코드 구조
처음으로 배울 것은 코드 블록을 만드는 방법입니다.
## 문
문(statement)은 어떤 작업을 수행하는 문법 구조(syntax structure)와 명령어(command)를 의미합니다.

앞서 'Hello, world!' 메시지를 보여주는 `alert('Hello, world!')` 문을 확인한 바 있습니다.

코드에는 원하는 만큼 문을 작성할 수 있습니다. 이때, 서로 다른 문은 세미콜론으로 구분합니다.

아래 코드는 'Hello World'를 두 개의 alert 문으로 나눈 예시입니다.
```
alert('Hello'); alert('World');
```
코드의 가독성을 높이기 위해 각 문은 서로 다른 줄에 작성하는 것이 일반적입니다.
```
alert('Hello');
alert('World');
```
## 세미콜론 [#semicolon]
줄 바꿈이 있다면 세미콜론을 생략할 수 있습니다.

아래 코드는 에러 없이 동작합니다.
```
alert('Hello')
alert('World')
```
자바스크립트는 줄 바꿈이 있으면 이를 '암시적' 세미콜론으로 해석합니다. 이런 동작 방식을 세미콜론 자동 삽입(automatic semicolon insertion)이라 부릅니다.

**대부분의 경우, 줄바꿈은 세미콜론을 의미합니다. 하지만 '대부분의 경우'가 '항상'을 의미하진 않습니다.**

아래와 같이 줄 바꿈이 세미콜론을 의미하지 않는 경우도 있습니다.

```js run no-beautify
alert(3 +
1
+ 2);
```
세미콜론 자동 삽입이 일어나지 않았기 때문에 6이 출력됩니다. 어떤 줄이 `"+"`로 끝나면, 그 줄은 '불완전한 표현식'이므로 세미콜론이 필요하지 않다는 걸 직감하실 겁니다. 위 코드도 이런 의도로 동작합니다.

**반면, 세미콜론이 정말로 필요하지만 자바스크립트가 이를 추정하지 '못하는' 상황도 존재합니다.**

이런 상황에서 발생하는 에러는 찾거나 고치기가 상당히 어렵습니다.

자바스크립트가 세미콜론을 자동으로 삽입해주지 못하는 구체적인 상황은 다음과 같습니다.

```js run
[1, 2].forEach(alert)
```
지금은 대괄호 `[]`와 `forEach`는 아직 배우지 않았으므로, 깊이 생각하지 말고 코드를 살펴봅시다. 이들에 대해서는 추후에 학습할 예정입니다. 코드를 실행하면 결과가 `1`과 `2`가 출력된다는 사실만 기억하세요.

이제 위에서 작성한 코드 앞쪽에 **세미콜론을 붙이지 않은 채** `alert`을 추가해 봅시다.

```js run no-beautify
alert("에러가 발생합니다.")

[1, 2].forEach(alert)
```
예제를 실행해 보면 새롭게 추가한 `alert`만 제대로 출력되고 에러가 발생하는 걸 확인할 수 있습니다.

`alert` 끝에 세미콜론을 추가해 다시 실행하면 코드는 잘 작동합니다.
```js run
alert("제대로 동작합니다.");

[1, 2].forEach(alert)
```
"제대로 동작합니다." 메시지 다음에 `1`과 `2`가 나타나는 것을 확인할 수 있죠.

세미콜론이 없을 때 에러가 발생했던 이유는 자바스크립트가 대괄호 `[...]` 앞에는 세미콜론이 있다고 가정하지 않기 때문입니다.

따라서 세미콜론 자동 삽입이 일어나지 않고 첫 번째 예제는 단일 문으로 처리됩니다. 자바스크립트 엔진이 보게 될 코드는 다음과 같아지죠.
```js run no-beautify
alert("에러가 발생합니다.")[1, 2].forEach(alert)
```
하지만 원래 이 코드는 단일 문이 아닌 두 개의 서로 다른 문이었습니다. 문이 잘못 합쳐지면서 에러가 발생한 것이죠. 이 예제 외에도 이런 상황이 발생할 여지는 언제나 있습니다.

줄 바꿈으로 문을 나눴더라도, 문 사이에는 세미콜론을 넣는 것이 좋습니다. 자바스크립트 커뮤니티에서도 이를 규칙으로 정해 권장하고 있습니다. 다시 한번 정리하도록 하겠습니다. 세미콜론은 **생략할 수 있습니다.** 하지만 세미콜론을 사용하는 것이 더 안전하므로 이를 기억하고 따르도록 합시다. 입문자라면 이를 더 잘 지키도록 합시다.

## 주석[#code-comments]
시간이 흐름에 따라 자바스크립트 프로그램은 더욱더 복잡해졌습니다. 이로 인해 무슨 일이 왜 벌어지고 있는지를 설명해주는 주석(comment)의 필요성이 요구되었습니다.

주석은 스크립트의 어느 곳에나 작성할 수 있습니다. 자바스크립트 엔진은 주석을 무시하기 때문에 주석의 위치는 실행에 영향을 주지 않습니다.

**한 줄짜리 주석은 두 개의 슬래시 `//`로 시작됩니다.**

슬래시 뒤엔 주석을 적어주면 됩니다. 한 줄을 주석이 다 차지하는 형태도 있고 문 다음에 주석이 이어지는 형태도 있습니다.

아래와 같이 말이죠.
```js run
// 이 주석은 한 줄을 다 차지합니다.
alert('Hello');
alert('World'); // 이 주석은 문 다음 이어집니다.
```
**여러 줄의 주석은 슬래시와 별표 `/*`로 시작해 별표와 슬래시 `*/`로 끝납니다.**

아래와 같이 말이죠.
```js run
/* 두 줄짜리 주석 예제
이것은 여러 줄의 주석입니다.
*/
alert('Hello');
alert('World');
```
주석의 내용은 무시됩니다. 주석 `/* ... */` 안에 코드가 들어가도 이 코드는 실행되지 않습니다.

이를 이용하면 코드 일부를 일시적으로 비활성화할 수 있습니다.
```js run
/* 코드 주석 처리하기
alert('Hello');
*/
alert('World');
```
대부분의 에디터는 주석 처리 단축키를 지원합니다. 블록을 지정하고 단축키를 누르면 원하는 코드를 주석 처리할 수 있죠. `key:Ctrl+/`를 사용하면 해달 줄의 코드를 한 줄짜리 주석으로 처리할 수 있으며, 여러 줄의 주석은 `key:Ctrl+Shift+/`를 통해 처리할 수 있습니다. Mac에서는 `key:Ctrl` 대신 `key:Cmd`를, `key:Shift` 대신 `key:Option`를 사용하세요.

`/*...*/`안에 또 다른 `/*...*/`이 있을 수 없습니다.

주석을 중첩해 쓰면 에러가 발생합니다.
```js run no-beautify
/*
  /* 중첩 주석 ?!? */
*/
alert( 'World' );
```
주석 달기를 두려워하지 마세요.

주석을 달면 코드의 전체적인 길이가 증가합니다. 하지만 이는 전혀 문제가 되지 않습니다. 프로덕션 서버에 배포하기 전에 코드를 압축해주는 도구가 많이 있고, 이 도구들은 주석을 삭제해주기 때문입니다. 실행 중인 스크립트에는 주석이 들어가지 않으므로, 주석은 최종으로 배포되는 코드에 부정적인 영향을 끼치지 않습니다.

주석을 잘 쓰는 방법에 대해서는 info:code-quality 챕터에서 더 이야기 나눠보도록 하겠습니다.