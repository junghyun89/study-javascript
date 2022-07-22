# alert, prompt, confirm을 이용한 상호작용
브라우저를 데모 환경으로 사용 중이므로 브라우저 환경에서 사용되는 최소한의 사용자 인터페이스 기능인 `alert`, `prompt`, `confirm`에 대해 알아보겠습니다.

## alert
`alert` 함수는 앞선 예제에서 살펴본 바 있습니다. 이 함수가 실행되면 사용자가 '확인(OK)' 버튼을 누를 때까지 메시지를 보여주는 창이 계속 떠있게 됩니다.

예시를 살펴봅시다.

```js run
alert("Hello");
```
메시지가 있는 작은 창은 *모달 창(modal window)* 이라고 부릅니다. '모달'이란 단어엔 페이지의 나머지 부분과 상호 작용이 불가능하다는 의미가 내포되어 있습니다. 따라서 사용자는 모달 창 바깥에 있는 버튼을 누른다든가 하는 행동을 할 수 없습니다. 확인 버튼을 누르기 전까지 말이죠.

## prompt
브라우저에서 제공하는 `prompt` 함수는 두 개의 인수를 받습니다.

```js no-beautify
result = prompt(title, [default]);
```

함수가 실행되면 텍스트 메시지와 입력 필드(input field), 확인(OK) 및 취소(Cancel) 버튼이 있는 모달 창을 띄워줍니다.

`title`
: 사용자에게 보여줄 문자열

`default`
: 입력 필드의 초깃값(선택값)

```
"인수를 감싸는 대괄호 `[...]`의 의미"

`default`를 감싸는 대괄호는 이 매개변수가 필수가 아닌 선택값이라는 것을 의미합니다.
```

사용자는 프롬프트 대화상자의 입력 필드에 원하는 값을 입력하고 확인을 누를 수 있습니다. 값을 입력하길 원하지 않는 경우는 취소(Cancel) 버튼을 누르거나 `key:Esc`를 눌러 대화상자를 빠져나가면 됩니다.

`prompt` 함수는 사용자가 입력 필드에 기재한 문자열을 반환합니다. 사용자가 입력을 취소한 경우는 `null`이 반환됩니다.

예시:

```js run
let age = prompt('나이를 입력해주세요.', 100);
alert(`당신의 나이는 ${age}살 입니다.`); // 당신의 나이는 100살입니다.
```

```
"Internet Explorer(IE)에서는 항상 '기본값'을 넣어주세요."

프롬프트 함수의 두 번째 매개변수는 선택사항이지만, 이 매개변수가 없는 경우 IE는 `"undefined"`를 입력 필드에 명시합니다.
```
아래 코드를 IE에서 실행해 보세요.
```js run
let test = prompt("Test");
```
IE 사용자를 비롯한 모든 사용자에게 깔끔한 프롬프트를 보여주려면 아래와 같이 두 번째 매개변수를 항상 전달해 줄 것을 권장합니다. 
```js run
let test = prompt("Test", ''); // <-- IE 사용자를 위한 매개변수 처리
```
## 컨펌 대화상자
문법:

```js
result = confirm(question);
```

`confirm` 함수는 매개변수로 받은 `question(질문)`과 확인 및 취소 버튼이 있는 모달 창을 보여줍니다.

사용자가 확인버튼를 누르면 `true`, 그 외의 경우는 `false`를 반환합니다.

예시:

```js run
let isBoss = confirm("당신이 주인인가요?");
alert( isBoss ); // 확인 버튼을 눌렀다면 true가 출력됩니다.
```

## 요약
브라우저는 사용자와 상호작용할 수 있는 세 가지 함수를 제공합니다.

`alert`
: 메시지를 보여줍니다.

`prompt`
: 사용자에게 텍스트를 입력하라는 메시지를 띄워줌과 동시에, 입력 필드를 함께 제공합니다. 확인을 누르면 `prompt` 함수는 사용자가 입력한 문자열을 반환하고, 취소 또는 `key:Esc`를 누르면 `null`을 반환합니다.  

`confirm`
: 사용자가 확인 또는 취소 버튼을 누를 때까지 메시지가 창에 보여집니다. 사용자가 확인 버튼을 누르면 `true`를, 취소 버튼이나 `key:Esc`를 누르면 `false`를 반환합니다. 

위 함수들은 모두 모달 창을 띄워주는데, 모달 창이 떠 있는 동안은 스크립트의 실행이 일시 중단됩니다. 사용자가 창을 닫기 전까진 나머지 페이지와 상호 작용이 불가능합니다.

지금까지 살펴본 세 함수엔 두 가지 제약사항이 있습니다.

1. 모달 창의 위치는 브라우저가 결정하는데, 대개 브라우저 중앙에 위치합니다.
2. 모달 창의 모양은 브라우저마다 다릅니다. 개발자는 창의 모양을 수정할 수 없습니다.

이런 제약사항은 간결성을 위해 치러야 할 대가입니다. 창을 더 멋지게 꾸미고 복잡한 상호작용을 가능하게 해주는 다른 방법도 있긴 하지만, '멋을 위한 부가 기능'이 필요하지 않다면 지금까지 소개해드린 기본 메서드만으로 충분합니다.