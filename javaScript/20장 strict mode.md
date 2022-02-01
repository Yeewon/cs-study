- 자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다. 이때 전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용할 수 있다. **암묵적 전역 (implicit global)**
- ES5부터 strict mode(엄격 모드) 추가
  - strict mode는 자바스크립트 언어늬 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.
- 린트 도구는 strcit mode가 제한하는 오류는 물론 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과를 얻을 수 있다. 👉 strict mode보다 린트 도구의 사용을 선호

```jsx
"use strict";

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

## 전역에 strict mode를 적용하는 것은 피하자

전역에 적용한 strict mode는 스크립트 단위로 적용된다.

다른 스크립트에 영향을 주지 않는다. strict mode 스크립트와 non-strict mode 스크립트 혼용은 오류를 발생시킬 수 있다.

## 함수 단위로 strict mode를 적용하는 것도 피하자

strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 발생할 수 있다.

## strict mode가 발생시키는 에러

### 1. 암묵적 전역

선언하지 않은 변수를 참조하면 `ReferenceError` 가 발생한다.

### 2. 변수, 함수, 매개변수의 삭제

delete 연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError` 가 발생한다.

### 3. 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 `SyntaxError` 가 발생한다.

### 4. with 문의 사용

with 문을 사용하면 `SyntaxError` 가 발생한다.

> with문은 동일한 객체의 프로퍼티를 반복해서 사용할 대 객체 이름을 생략할 수 있어서 코드가 간단해지지만 성능과 가독성이 나빠진다.
