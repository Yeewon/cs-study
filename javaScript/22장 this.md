๐ก ๊ฐ์ฒด๋ ์ํ๋ฅผ ๋ํ๋ด๋ **ํ๋กํผํฐ** ์ ๋์์ ๋ํ๋ด๋ **๋ฉ์๋** ๋ฅผ ํ๋์ ๋ผ๋ฆฌ์ ์ ๋จ์๋ก ๋ฌถ์ ๋ณตํฉ์ ์ธ ์๋ฃ๊ตฌ์กฐ๋ค.

- ๋ฉ์๋๋ ์์ ์ด ์ํ ๊ฐ์ฒด์ ์ํ(ํ๋กํผํฐ)๋ฅผ ์ฐธ์กฐํ๊ณ  ๋ณ๊ฒฝํ  ์ ์์ด์ผ ํ๋ค.
- ํ๋กํผํฐ๋ฅผ ์ฐธ์กฐํ๋ ค๋ฉด ๋จผ์  ์์ ์ด ์ํ ๊ฐ์ฒด๋ฅผ ๊ฐ๋ฆฌํค๋ ์๋ณ์๋ฅผ ์ฐธ์กฐํ  ์ ์์ด์ผ ํ๋ค.

## this

**์์ ์ด ์ํ ๊ฐ์ฒด ๋๋ ์์ ์ด ์์ฑํ  ์ธ์คํด์ค๋ฅผ ๊ฐ๋ฆฌํค๋ ์๊ธฐ ์ฐธ์กฐ ๋ณ์**

- ์๋ฐ์คํฌ๋ฆฝํธ ์์ง์ ์ํด ์๋ฌต์ ์ผ๋ก ์์ฑ๋๋ค.
- this ๋ฐ์ธ๋ฉ์ ํจ์ ํธ์ถ ๋ฐฉ์์ ์ํด ๋์ ์ผ๋ก ๊ฒฐ์ ๋๋ค.
  > **๋ฐ์ธ๋ฉ์ด๋ ์๋ณ์์ ๊ฐ์ ์ฐ๊ฒฐํ๋ ๊ณผ์ ์ด๋ค.**

### 1. ์ผ๋ฐ ํจ์ ํธ์ถ

๊ธฐ๋ณธ์ ์ผ๋ก `this` ์๋ ์ ์ญ ๊ฐ์ฒด๊ฐ ๋ฐ์ธ๋ฉ๋๋ค.

- ์ผ๋ฐ ํจ์๋ก ํธ์ถ๋ ๋ชจ๋  ํจ์(์ค์ฒฉ ํจ์, ์ฝ๋ฐฑ ํจ์ ํฌํจ) ๋ด๋ถ์ `this` ์๋ ์ ์ญ ๊ฐ์ฒด๊ฐ ๋ฐ์ธ๋ฉ๋๋ค.
- this๋ ๊ฐ์ฒด์ ํ๋กํผํฐ๋ ๋ฉ์๋ ์ฐธ์กฐ๋ฅผ ์ํ ์๊ธฐ ์ฐธ์กฐ ๋ณ์์ด๋ฏ๋ก ๊ฐ์ฒด๋ฅผ ์์ฑํ์ง ์๋ ์ผ๋ฐ ํจ์์์ `this` ๋ ์๋ฏธ๊ฐ ์๋ค.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log(this); // {value: 100, foo: f}
    // ์ฝ๋ฐฑ ํจ์ ๋ด๋ถ์ this์๋ ์ ์ญ ๊ฐ์ฒด๊ฐ ๋ฐ์ธ๋ฉ๋๋ค.
    setTimeout(function () {
      console.log(this); // window
      console.log(this.value); // 1
    }, 100);
  },
};
```

๋ฉ์๋ ๋ด๋ถ์ ์ค์ฒฉ ํจ์๋ ์ฝ๋ฐฑ ํจ์์ this ๋ฐ์ธ๋ฉ์ ๋ฉ์๋์ this ๋ฐ์ธ๋ฉ๊ณผ ์ผ์น์ํค๊ธฐ ์ํ ๋ฐฉ๋ฒ

1. this ๋ฐ์ธ๋ฉ์ ๋ณ์์ ํ ๋น

   ```jsx
   var value = 1;

   const obj = {
     value: 100,
     foo() {
       const that = this;

       // ์ฝ๋ฐฑ ํจ์ ๋ด๋ถ์์ this ๋์  that์ ์ฐธ์กฐํ๋ค.
       setTimeout(function () {
         console.log(that.value); // 100
       }, 100);
     },
   };
   ```

2. this๋ฅผ ๋ช์์ ์ผ๋ก ๋ฐ์ธ๋ฉํ  ์ ์๋ `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` ๋ฉ์๋ ์ ๊ณต

   ```jsx
   var value = 1;

   const obj = {
     value: 100,
     foo() {
       // ์ฝ๋ฐฑ ํจ์์ ๋ช์์ ์ผ๋ก this๋ฅผ ๋ฐ์ธ๋ฉํ๋ค.
       setTimeout(
         function () {
           console.log(that.value); // 100
         }.bind(this),
         100
       );
     },
   };
   ```

3. ํ์ดํ ํจ์ ์ฌ์ฉ

   ```jsx
   var value = 1;

   const obj = {
     value: 100,
     foo() {
       // ํ์ดํ ํจ์ ๋ด๋ถ์ this๋ ์์ ์ค์ฝํ์ this๋ฅผ ๊ฐ๋ฆฌํจ๋ค.
       setTimeout(() => console.log(this.value), 100); // 100
     },
   };
   ```

### 2. ๋ฉ์๋ ํธ์ถ

- ๋ฉ์๋ ๋ด๋ถ์ this๋ ๋ฉ์๋๋ฅผ ์์ ํ ๊ฐ์ฒด๊ฐ ์๋ ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด์ ๋ฐ์ธ๋ฉ๋๋ค.
- this์ ๋ฐ์ธ๋ฉ๋  ๊ฐ์ฒด๋ ํธ์ถ ์์ ์ ๊ฒฐ์ ๋๋ค.

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person("Lee");
// getName ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด๋ me๋ค.
console.log(me.getName()); // 'Lee'

Person.prototype.name = "Kim";
// getName ๋ฉ์๋๋ฅผ ํธ์ถํ ๊ฐ์ฒด๋ Person.prototype์ด๋ค.
console.log(Person.prototype.getName()); // 'Kim'
```

### 3. ์์ฑ์ ํจ์ ํธ์ถ

- ์์ฑ์ ํจ์๊ฐ (๋ฏธ๋์) ์์ฑํ  ์ธ์คํด์ค๊ฐ ๋ฐ์ธ๋ฉ๋๋ค.

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

### 4. Function.prototype.apply/call/bind ๋ฉ์๋์ ์ํ ๊ฐ์  ํธ์ถ

- apply, call ๋ฉ์๋์ ๋ณธ์ง์ ์ธ ๊ธฐ๋ฅ์ ํจ์๋ฅผ ํธ์ถํ๋ ๊ฒ์ด๋ค.
- bind ๋ฉ์๋๋ ํจ์๋ฅผ ํธ์ถํ์ง ์๊ณ  this๋ก ์ฌ์ฉํ  ๊ฐ์ฒด๋ง ์ ๋ฌํ๋ค.

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
    // bind ๋ฉ์๋๋ก callback ํจ์ ๋ด๋ถ์ this ๋ฐ์ธ๋ฉ์ ์ ๋ฌ
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee
});
```
