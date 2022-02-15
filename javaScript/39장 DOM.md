- 브라우저의 `렌더링 엔진`은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 **DOM**을 생성한다.
- HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환된다.
- HTML 요소는 중첩 관계를 갖는다.
- 모든 노드 객체들을 트리 자료 구조로 구성한다.

> DOM : 노트 객체들로 구성된 트리 자료구조. 노드 객체의 계층적인 구조로 구성된다.

<aside>
💡 DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API를 제공한다.

</aside>

## 노드 타입

- 표준 빌트인 객체가 아닌 브라우저 환경에서 추가적으로 제공하는 **호스트 객체**다.

### 1. 문서 노드

- 최상위에 존재하는 루트 노드
- document 객체를 가리키는 객체
- DOM 트리의 노드들에 접근하기 위한 **진입점** 역할을 담당한다.

### 2. 요소 노드

- HTML 요소를 가리키는 객체
- 문서 구조를 표현

### 3. 어트리뷰트 노드

- HTML 요소의 어트리뷰트를 가리키는 객체

### 4. 텍스트 노드

- HTML 요소의 텍스트를 가리키는 객체

## 요소 노드 취득

```html
<!DOCTYPE html>

<body>
    <ul id="fruits">
        <li>Apple</li>
        <li>Banana</li>
        <li>Orange</li>
    </ul>
    <ul>
        <li>HTML</li>
    </ul>
    <script>
        const $fruits = document.getElementById('fruits');
        const $lisFromFruits = document.getElementsByTagName('li');
        console.log($lisFromFruits) // HTMLCollection(4) [li, li, li, li]
    </script>
</body>

</html>
```

### 1. getElementById

- id는 HTML에서 유일한 값이어야 한다.
- 여러개가 존재해도 첫 번째 요소 노드만 반환한다.

### 2. getElementByTagName

- 태그 이름을 갖는 모든 요소 노드를 탐색하여 반환한다.
- 반환 : **HTMLCollection** ( DOM 컬렉션 객체 )
  > HTMLCollection : 유사 배열 객체이면서 이터러블

```jsx
const $fruits = document.getElementById("fruits");
const $lisFromFruits = document.getElementsByTagName("li");

console.log($lisFromFruits); // HTMLCollection(4) [li, li, li, li]
```

### 3. getElementsByClassName

- 반환 : **HTMLCollection** ( DOM 컬렉션 객체 )

### 4. querySelector

- 여러 개인 경우 첫 번째 요소 노드만 반환한다.
- 존재하지 않는 경우 null을 반환한다.

### 5. querySelectorAll

- 만족시키는 모든 요소 노드를 탐색하여 반환한다.
- 반환 : **NodeList** ( DOM 컬렉션 객체 )
  > **NodeList** : 유사 배열 객체이면서 이터러블

### ⭐ Point

- `getElementById` 가 `querySelector`, `querySelectorAll` 보다 빠르다.
- `getElementById` 가 CSS 선택자 문법을 사용하여 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있고 일관된 방식으로 요소 노드를 취득할 수 있다.
- id 어트리뷰트가 있는 요소 노드를 취득하는 경우에만 `getElementById` 를 사용하고 그 외에는 `querySelector`, `querySelectorAll` 메서드를 사용하는 것을 권장한다.

## HTMLCollection 과 NodeList

- 유사 배열 객체이면서 이터러블이므로 `for•••of` 문으로 순회할 수 있고 `스프레드 문법`을 사용하여 간단히 배열로 변환할 수 있다.

### HTMLCollection

- 노드 객체의 상태 변화를 실시간으로 반영하는 **살아 있는 DOM 컬렉션 객체**다. **= live 객체**
- 배열로 변환하여 `고차 함수(forEach, map, filter, reduce 등)`을 사용할 수 있다.

### NodeList

- 실시간으로 노드 객체의 **상태 변경을 반영하지 않는 객체**다. **= non-live 객체**

## DOM 조작

### 1. innerHTML

- 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.

**단점**

- **XSS(크로스 사이트 스크립팅 공격)** 에 취약하므로 위험하다.
- 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM을 변경한다.
- 요소 삽입할 때 위치를 지정할 수 없다.

### 2. insertAdjacentHTML

- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다.
- innerHTML 봐 빠르다.

**단점**

- **XSS(크로스 사이트 스크립팅 공격)** 에 취약하므로 위험하다.

### 3. DocumentFragment

- 기존 DOM과 별도로 존재하여 기존 DOM에는 어떠한 변경도 발생하지 않는다.
- 자신은 제거되고 자신의 자식 노드만 DOM에 추가된다.

```html
<!DOCTYPE html>

<body>
    <ul id="fruits">
    </ul>
    <ul>
        <li>HTML</li>
    </ul>
</body>

</html>
```

```jsx
const $fruits = document.getElementById("fruits");
// DocumentFragment 노드 생성
const $fragment = document.createDocumentFragment();

["Apple", "Banana", "Orange"].forEach((text) => {
  const $li = document.createElement("li");
  const textNode = document.createTextNode(text);

  $li.appendChild(textNode);
  $fragment.appendChild($li);
});

$fruits.appendChild($fragment);
```

- 실제로 DOM 변경이 발생하는 것은 한 번뿐이며 리플로우와 리페인트도 한 번만 실행된다.
- 따라서 여러 개의 노드를 DOM에 추가하는 경우 `DocumentFragment` 노드를 사용하는 것이 더 효율적이다.

## HTML 어트리뷰트 조작

- `setAttribute` / `setAttribute`
- `data` 어트리뷰트와 `dataset` 프로퍼티

```html
<!DOCTYPE html>

<body>
    <ul class="users">
        <li id="1" data-user-id="1234" data-role="admin">Jung</li>
        <li id="2" data-user-id="5342" data-role="subscriber">Lee</li>
    </ul>
    <script>
        const users = [...document.querySelector('.users').children]
        const user = users.find(({ dataset }) => dataset.userId === '1234')
        console.log(user.dataset.role) // admin

        user.dataset.role = 'subscriber'
        console.log(user.dataset.role) // subscriber
    </script>
</body>

</html>
```

```jsx
const users = [...document.querySelector(".users").children];
const user = users.find(({ dataset }) => dataset.userId === "1234");
console.log(user.dataset.role); // admin

user.dataset.role = "subscriber";
console.log(user.dataset.role); // subscriber
```

## 클래스 조작

### 1. add( ...className )

2. remove( ...className )
3. item( index )
4. contains( className )
5. replace( oldClassName, newClassName )
6. toggle( className )
