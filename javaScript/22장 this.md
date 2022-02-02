💡 객체는 상태를 나타내는 **프로퍼티** 와 동작을 나타내는 **메서드** 를 하나의 논리적은 단위로 묶은 복합적인 자료구조다.

- 메서드는 자신이 속한 객체의 상태(프로퍼티)를 참조하고 변경할 수 있어야 한다.
- 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

## this

**자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수**

- 자바스크립트 엔진에 의해 암묵적으로 생성된다.
- this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
  > **바인딩이란 식별자와 값을 연결하는 과정이다.**

### 1. 일반 함수 호출

기본적으로 `this` 에는 전역 객체가 바인딩된다.

- 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 `this` 에는 전역 객체가 바인딩된다.
- this는 객체의 프로퍼티나 메서드 참조를 위한 자기 참조 변수이므로 객체를 생성하지 않는 일반 함수에서 `this` 는 의미가 없다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(this); // {value: 100, foo: f}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log(this); // window
      console.log(this.value); // 1
    }, 100);
  },
};
```

메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법

1. this 바인딩을 변수에 할당

   ```jsx
   var value = 1;

   const obj = {
     value: 100,
     foo() {
       const that = this;

       // 콜백 함수 내부에서 this 대신 that을 참조한다.
       setTimeout(function () {
         console.log(that.value); // 100
       }, 100);
     },
   };
   ```

2. this를 명시적으로 바인딩할 수 있는 `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` 메서드 제공

   ```jsx
   var value = 1;

   const obj = {
     value: 100,
     foo() {
       // 콜백 함수에 명시적으로 this를 바인딩한다.
       setTimeout(
         function () {
           console.log(that.value); // 100
         }.bind(this),
         100
       );
     },
   };
   ```

3. 화살표 함수 사용

   ```jsx
   var value = 1;

   const obj = {
     value: 100,
     foo() {
       // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
       setTimeout(() => console.log(this.value), 100); // 100
     },
   };
   ```

### 2. 메서드 호출

- 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.
- this에 바인딩될 객체는 호출 시점에 결정된다.

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person("Lee");
// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // 'Lee'

Person.prototype.name = "Kim";
// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // 'Kim'
```

### 3. 생성자 함수 호출

- 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply, call 메서드의 본질적인 기능은 함수를 호출하는 것이다.
- bind 메서드는 함수를 호출하지 않고 this로 사용할 객체만 전달한다.

```jsx
function getThisBinding() {
  return this;
}

const thisArg = { a: 1 };

console.log(getThisBinding()); // window

console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}

console.log(getThisBinding.bind(thisArg)); // getThisBinding
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

```jsx
const person = {
  name: "Lee",
  foo(callback) {
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee
});
```
