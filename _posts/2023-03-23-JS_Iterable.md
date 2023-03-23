---
layout: post
title: 이터러블, 이터레이터, 이터레이션 프로토콜.. 그래서 뭐가 뭔데?
date: 2023-03-23 23:00:00 +0900
categories: [JavaScript]
tags: [JavaScript, Iterable, Iterator, Iteration Protocol]
description: 자바스크립트, JS, JavaScript, 이터러블, 이터레이터, 이터레이션 프로토콜, Iterable, Iterator, Iteration Protocol
toc: true
math: true
mermaid: true
# image:
#   src: /assets/img/ts-thum.png
#   width: 1000   # in pixels
#   height: 400   # in pixels
#   alt: 타입스크립트
---

<!-- 썸네일 -->

![image](https://user-images.githubusercontent.com/101175828/223528673-e65789e8-783d-4e48-99d1-58cc2f17028b.png)

## 🌈 Intro

---

<div style="text-align:center"><img src="https://user-images.githubusercontent.com/101175828/227130667-9555b41e-00a3-4b85-a296-377abefd1acf.png"/></div>

최근 이터러블이면서 이터레이터인 제너레이터에 대해 학습했었고, 이터러블이면서 이터레이터 인건 뭐지..? 라는 의문이 들었다 🤔  
이터러블에 대해서는 인지하고 있었지만 완벽하게 이해하지 않았기에 그러한 의문이 들었다는 생각에 이터러블에 확실하게 알고 넘어가고 싶었다.

이번 포스팅은 이전 포스트인 [심볼](https://chobae.github.io/posts/JS_Symbol/)을 먼저 읽어본다면 도움이 될 것 같다 😊

## 🌲 알고 가면 좋은 배경 지식

---

### 우린 이미 이터러블에 대해 알고 있다.

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227132670-24811ae4-6127-4259-92c7-31d980e04ee7.png" alt="이미지 설명"/>
  <figcaption>갑자기 분위기 공포영화 🥶</figcaption>
</figure>

```js
const arr = [1, 2, 3];
for (const item of arr) {
  console.log(item); // 1, 2, 3
}
```

이 코드를 한번쯤 작성해보셨을 것이라고 생각한다.  
그냥 평소처럼 배열을 `for of `문을 통해서 순회했다. ~~뭐 어쩌라고...~~  
사실 `for of `문은 **배열이 이터러블**이기 때문에 가능한 것이다.  
내가 코테 연습을 할때 자주하는 실수는 객체에 `for of `문을 사용하는 것인데 그럼 이 녀석을 만나게 된다.  
**`TypeError: obj is not iterable`** 🙃  
그렇다...! 객체는 이터러블이 아니기 때문에 `for of `문을 사용할 수 없는 것이다.  
배열 이외에도 이터러블한 녀석들이 많은데 이제 본격적으로 알아보자🤗

## ❔ iterable..?

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227135784-177f2b73-dfe0-40a4-908c-3510fe978e47.png" alt="이미지 설명"/>
  <figcaption>뭐가 이렇게 많아..</figcaption>
</figure>

이터러블에 대해서 알아보기 이전에 이터레이션 프로토콜에 대해서 알아 봐야한다. ~~이거 뭐 오마카세도 아니고..~~

### 이터레이션 프로토콜..?

ES6 도입된 이터레이션 프로토콜은 순회 가능한 데이터 자료구조를 만들기 위해 ECMAScript 사양에 정의하여 약속한 규칙이다.  
쉽게 풀어말하면 **`이제 순회 방식 통일할거야!`** 라는 뜻이다.

### 이터러블 프로토콜..?

`빌트인 심볼(Well-known Symbol)`인 **`Symbol.iterator`** 를

1. 프로퍼티 키로 사용한 메서드를 직접 구현하거나
2. 프로토타입 체인을 통해 상속받거나

두 경우를 통해 **`Symbol.iterator`** 메서드를 호출하면 이터레이터 프로토콜을 준수한 **이터레이터를 반환**한다.  
이러한 규약을 **이터러블 프로토콜**이라 한다.  
**이터러블 프로토콜을 준수한 객체를 이터러블**이라하고,  
이터러블은 **for … of 문으로 순회**할 수 있으며 **스프레드 문법**과 **구조 분해 할당**의 대상으로 사용 할 수 있다.

어떻게 쉽게 전달할까 고민이 많았는데 **이터레이터 메서드를 가지고 있는 객체를 이터러블하다**라고 이해하면 될 것 같다.

### 이터레이터 프로토콜..?

이터러블의 **`Symbol.iterator`** 메서드를 호출하면 이터레이터 프로토콜을 준수한 **이터레이터**를 반환한다고 했다.  
이터레이터는 **`next`** 메서드를 소유하고 해당 메서드 호출 시 **이터러블을 순회하며 `value`와 `done` 프로퍼티를 갖는 이터레이터 리절트 객체를 반환**한다.  
이러한 규약을 이터레이터 프로토콜이라고 한다.
**이터레이터 프로토콜을 준수한 객체를 이터레이터**라 한다.
이터레이터는 이터러블 요소를 탐색하기 위한 포인터 역할을 한다고 할 수 있다.

꼬리에 꼬리를 무는 관계라서 어렵다.  
이터레이터를 다른 녀석들과의 차이점을 쉽게 이해하려면 **`next` 메서드를 가지고 있는 객체를 이터레이터**라고 이해하면 된다.

## 💪🏻 그래서 어떻게 씀?

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227141668-51bb02f6-8a3d-4e3e-9b14-4cab9227df9a.png" alt="이미지 설명"/>
  <figcaption>😇</figcaption>
</figure>

### 1. 이터러블

아까 언급했듯이 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.  
즉, 이터러블은 Symbol.iterator 를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

```js
const isIterable = (v) =>
  v !== null && typeof v[Symbol.iterator] === "function";

// 배열, 문자열, Map, Set, arguments, NodeList, TypedArray 등은 모두 이터러블이다.
console.log(isIterable([])); // true
console.log(isIterable("")); // true
console.log(isIterable(new Map())); // true
console.log(isIterable(new Set())); // true
console.log(isIterable(arguments)); // true
// console.log(isIterable(document.querySelectorAll("*"))); // true
console.log(isIterable(new Uint8Array())); // true
console.log(isIterable({})); // false
```

위의 코드를 통해서 해당 자료 구조들이 이터러블한지 체크할 수 있다.  
우리가 자주 접하는 배열 또한 이터러블하다고 확인할 수 있는데 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는다.  
즉, 아래의 기능들을 활용 할 수 있다.

```js
const arr = [1, 2, 3];
// 1. 이터러블한 배열은 for ... of 문을 사용할 수 있다.
for (const a of arr) {
  console.log(a); // 1 2 3
}
// 2. 이터러블한 배열은 스프레드 문법을 사용할 수 있다.
console.log([...arr]); // [1, 2, 3]
// 3. 이터러블한 배열은 구조 분해 할당을 사용할 수 있다.
const [one, two, three] = arr; // 1 2 3
```

반대의 경우를 보면 이해하기 더 쉬울 것 같다.
우리가 또 자주 접하는 **객체는 이터러블이 아니다.**  
그렇기에 당연하게도`for … of` 문, `구조 분해 할당` 지원하지 않지만 **스프레드 문법은 허용**한다.(21년 1월 기준)

```js
const obj = { a: 1, b: 2, c: 3 };
// 객체는 Symbol.iterator 메서드를 갖고 있지 않다.
console.log(Symbol.iterator in obj); // undefined
// 이터러블이 아닌 객체는 for ... of 문을 사용할 수 없다.
for (const a of obj) {
  //TypeError: obj is not iterable
  console.log(a);
}
// 21년 1월 기준으로 객체에 스프레드 문법 사용이 허용되었다.
console.log({ ...obj }); // { a: 1, b: 2, c: 3 }
```

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227142710-9833ee37-3540-4e1e-9f1e-a885effef228.png" alt="이미지 설명"/>
  <figcaption>야.. 너두 할 수 있어!</figcaption>
</figure>

이렇게 객체는 **이터러블 하지 않지만 객체도 이터러블 프로토콜을 준수하도록 구현하면 이터러블**이 될 수 있다.

### 2. 이터레이터

이터러블의 **`Symbol.iterator`** 메서드를 호출하면 이터레이터 프로토콜을 준수한 **이터레이터를 반환**한다.  
이터레이터는 **`next` 메서드를 가지고 있고**, **`next`** 메서드는 **이터러블의 각 요소를 순회**하기 위한 포인터 역할을 한다.  
**`next`** 메서드 호출 시 이터러블을 **순차적으로 한 단계씩 순회하며 결과를 나타내는 `이터레이터 리절트 객체`를 반환**한다.

```js
const arr = [1, 2, 3];
// Symbol.iterator 메서드는 이터레이터를 반환
const iterator = arr[Symbol.iterator]();
// next 메서드를 통해 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환
// 아래처럼 순차적으로 순회할 수 있다.
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

`이터레이터 리절트 객체`의 `value` 프로퍼티는 현재 순회 중인 **이터러블의 값**이고,  
`done` 프로퍼티는 **이터러블의 순회 완료 여부**를 나타낸다.

## 🧐 한번 뜯어볼까?

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227144492-e841847a-44ee-4908-97ce-5ccfa457c597.png" alt="이미지 설명"/>
  <figcaption>본격적으로 조..져</figcaption>
</figure>

아직 애매하다. 더 깊게 이해하기 위해 `for .. of` 문을 한번 뜯어보자.

```js
const arr = [1, 2, 3];
// for of는 아래와 같이 동작한다.
for (const a of arr) {
  console.log(a); // 1 2 3
}
// for of의 내부적인 동작
const iterator = arr[Symbol.iterator]();
while (true) {
  // cur는 이터레이터 리절트 객체이다.
  const cur = iterator.next();
  if (cur.done) break;
  const a = cur.value;
  log(a); // 1 2 3
}
```

동작을 설명한다면

1. 이터레이터의 `next 메소드`를 호출한다.
2. 순회하면서 `next 메소드`가 `이터레이터 리절트 객체`를 반환한다.
3. `이터레이터 리절트 객체`의 `value` 프로퍼티 값을 `for …of` 문의 변수에 할당한다.
4. `done` 프로퍼티가 `true`가 될 때까지 1~3을 반복한다.

`for … of` 문은 이런 과정들 통해 내부적으로 이터레이터를 사용하여 순회하는 것이다.

## 🪡 그래서 왜 필요함..?

---

**'지금까지 순회 잘하고 있었잖아..? 우리 좋았잖아..'**  
이터러블에 대해서 공부하면서 **'왜 필요할까?'** 라는 의문이 계속해서 들었다.

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227151670-4e980847-5d2d-4ba3-ad26-f7d1901082d4.png" alt="이미지 설명"/>
  <figcaption>그래서 이터러블이 없던 시절로 돌아가보려고 한다 🧐</figcaption>
</figure>

### 이터러블이 없었던 ES6 이전

ES6 이전에는 순회 가능한 데이터 자료구조, 즉 `배열, 문자열, 유사 배열 객체, DOM` 등은 **통일된 규약없이 각자 나름의 구조**를 가지고 `for 문`, `for … in 문`, `forEach` 메서드 등 다양한 방법으로 순회 가능했다.

```js
const arr = [1, 2, 3];
// for 문
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1, 2, 3
}
// for … in 문
for (const item in arr) {
  console.log(item); // 1, 2, 3
}
// forEach 메서드
arr.forEach((item) => {
  console.log(item); // 1, 2, 3
});
```

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227153062-8c93aa51-f301-4446-bf5c-c56a0b35f3b9.png" alt="이미지 설명"/>
  <figcaption>여기서 어디가 불편할지 고민해보자.</figcaption>
</figure>

**'음.. 데이터 자료구조가 엄청 많네..?'**
.
.
.
**'어, 그러면 `for 문`같은 녀석들이 모든 데이터 자료구조를 파악하고 있어야겠는데? 힘들겠다...🥲'**

그렇다. 순회를 제공하는 `for 문` 같은 녀석들은 모든 데이터 자료구조를 파악하고 있어야 한다.
이터러블은 이러한 문제를 해결해주기 위해 등장했다.

### 이터러블이 등장한 ES6 이후

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227154177-cbaba37d-84db-4a56-8bc2-4019259fcb32.png" alt="이미지 설명"/>
  <figcaption>기특한 녀석...</figcaption>
</figure>

출처 : [semnil5202님 블로그](https://velog.io/@semnil5202/%EC%9D%B4%ED%84%B0%EB%9F%AC%EB%B8%94)

이터러블이 등장하면서 데이터 자료구조는 이터레이션 프로토콜을  
준수하는 이터러블로 통일하여 `for … of` 문, `스프레드 문법`, `구조 분해 할당`의 대상으로 사용할 수 있도록 **일원화**했다.  
쉽게 말해 순회 방식을 통일하였기 때문에 `for .. of 문`은 **모든 데이터 자료 구조를 파악할 필요가 없어진 것**이다.

## 🤗 우당탕탕 이터러블 활용기

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227158056-7573c677-6425-49c4-a6da-124daa2a2c21.png" alt="이미지 설명"/>
  <figcaption>개꿀인데?</figcaption>
</figure>

처음 `스프레드 문법`이나 `구조 분해 할당`을 경험했을때 '개꿀인데..?'라는 생각이 들었다.  
이터러블 덕분에 사용할 수 있는 녀석들을 알아보자.

### 스프레드 문법

스프레드는 **이터러블을 펼쳐서 개별 요소로 나열하는 문법**이다.
사용할 수 있는 대상은 `배열`, `문자열`, `Map`, `Set`, `arguments`, `DOM 컬렉션(nodeList, HTMLCollection)` 등이 있고, `for .. of 문`으로 순회할 수 있는 녀석들에 한정된다.

```js
// 배열 ...[1,2,3]은 1,2,3으로 펼쳐진다.
console.log(...[1, 2, 3]); // 1 2 3
// 문자열 ...'abc'는 'a', 'b', 'c'로 펼쳐진다.
console.log(..."abc"); // a b c
// Map, Set 또한 이터러블이다.
const map = new Map([
  ["a", 1],
  ["b", 2],
  ["c", 3],
]);
const set = new Set([1, 2, 3]);
console.log(...map); // ['a', 1] ['b', 2] ['c', 3]
console.log(...set); // 1 2 3
// 일반 객체는 이터러블이 아니다.
console.log(...{ a: 1, b: 2, c: 3 }); // TypeError: ... is not iterable
```

스프레드가 이런 거다라고 생각하고 넘어 가고, 어디에 활용할 수 있을지 알아보자.

#### 1. arugments로 활용

내가 자주 겪는 상황을 예시를 들어본다.

```js
const arr = [1, 2, 3];
const max = Math.max(arr); // NaN
// ㅎㅎ 아맞당..
const max = Math.max(...arr); // 3
```

스프레드 문법을 통해 배열을 펼쳐서 바로 넣어줄 수 있다.  
없다면 어떻게 해야할까?

```js
Math.max(1); // 1
Math.max(1, 2); // 2
Math.max(1, 2, 3); // 3
```

~~그만 알아보자..~~

그럼 스프레드 이전에는 어떻게 했을까?  
`apply`를 사용했다.

```js
const arr = [1, 2, 3];
const max = Math.max.apply(null, arr); // 3
// 스프레드 등장
const max = Math.max(...arr); // 3
```

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227163337-09c52e36-88ca-4549-baeb-8ace98b1c875.png" alt="이미지 설명"/>
  <figcaption>선..녀</figcaption>
</figure>

스프레드 문법을 통해서 **훨씬 간결하고, 가독성 좋은 코드를 작성**할 수 있다.

#### 2. 배열 병합(concat)

스프레드 문법은 특히 배열 리터럴에서 사용하면 기존의 방식보다 **간결하고, 가독성 좋은 코드 작성**이 가능하다.

**스프레드 이전의 방식**

```js
const arr = [1, 2].concat([3, 4]);
console.log(arr); // [1,2,3,4]
```

`concat` 메서드를 사용해서 배열을 합칠 수 있다.

**'음.. 이것도 나쁘진 않다.'**

**스프레드 이후의 방식**

```js
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1,2,3,4]
```

**'??? 머리속에서 concat은 잊었다.'**

위의 코드처럼 스프레드 문법을 통해서 별도의 메서드를 사용하지 않고도 배열을 합칠 수 있다.

#### 3. 배열 추가 및 제거(splice)

보통 배열을 추가하거나 제거할 때는 `splice` 메서드를 사용한다.  
`splice` 메서드의 세 번째 인수로 배열을 전달하면 배열 자체가 추가 된다.

```js
const arr1 = [1, 4];
const arr2 = [2, 3];

// 세 번째 인수 arr2를 해체하여 전달해야한다.
// 그렇지 않으면..
arr1.splice(1, 0, arr2); // [1, [2, 3], 4]
```

우리는 코드를 통해 `[1, 2, 3, 4]`라는 결과를 원했다.  
다시 스프레드 없이 원하는 결과를 얻어보자.

```js
const arr1 = [1, 4];
const arr2 = [2, 3];

Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]
```

과정은 이렇다.

1. `splice` 메서드를 `apply`를 통해 호출한다.
2. `apply`의 첫 번째 인수로 `arr1`을 전달한다.
3. `apply`의 두 번째 인수로 `[1, 0].concat(arr2)`를 전달한다.
4. `[1, 0].concat(arr2)`는 `[1, 0, 2, 3]`이 된다.
5. `splice` 메서드는 `arr1`의 1번째 인덱스에 0개를 제거하고, 2, 3을 추가한다.
6. `arr1`은 `[1, 2, 3, 4]`가 된다.

이 과정을 스프레드 문법을 통한다면..?

```js
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227168620-d27387d8-ea5a-4751-8269-14d85e7d1881.png" alt="이미지 설명"/>
  <figcaption>'지금까지 뭐한거지..?'</figcaption>
</figure>

스프레드 문법을 통해 **훨씬 간결하고, 가독성 좋아졌다.**

#### 4. 배열 복사(slice)

스프레드 연산자 이전에는 `slice` 메서드를 사용해서 배열을 복사했다.

```js
const arr = [1, 2, 3];
const copy = arr.slice();
console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false
```

`slice` 메서드는 배열의 얕은 복사본을 반환한다.  
스프레드 문법을 사용해보자.

```js
const arr = [1, 2, 3];
const copy = [...arr];
console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false
```

**스프레드 문법 또한 얕은 복사본을 반환한다.**

#### 5. 객체 병합(assign)

TC39 프로세스의 stage 4(Finished) 단계에 제안되어 있는 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에도 스프레드 문법을 사용할 수 있다.  
원래 객체는 이터러블이 아니기 때문에 스프레드 문법을 사용할 수 없었지만 허용 해주겠다는 것이다.

스프레드 이전에는 객체 병합시 `Object.assign` 메서드를 사용했다.

```js
const mergedObj = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(mergedObj); // {x: 1, y: 10, z: 3}
```

**'수정과 병합을 동시에 할 수 있다는 점이 좋다는 생각이 들었다.'**  
스프레드를 적용해보자.

```js
const mergedObj = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(mergedObj); // {x: 1, y: 10, z: 3}
```

스프레드의 경우도 수정과 병합을 동시에 할 수 있는데 순서상에 **뒤에 있는 객체의 프로퍼티가 앞에 있는 객체의 프로퍼티를 덮어**쓴다.

#### 🚨 Rest 파라미터 (혼동 주의)

스프레드와 혼동될 수 있는 주제라서 가져와봤다.  
Rest 파라미터는 함수의 파라미터 목록에 사용할 수 있는 문법이다.

```js
function restTest(...rest) {
  // ...rest는 나열된 파라미터를 배열로 만들어준다.
  console.log(rest); // [1, 2, 3, 4, 5]
}

console.log(restTest(1, 2, 3, 4, 5));
```

정확히 스프레드 문법의 반대 개념이라고 할 수 있다.  
우린 스프레드를 통해서 배열을 펼쳐서 함수의 파라미터로 넘겼다면, Rest 파라미터는 **함수의 파라미터값들을 배열로 만들어준다.**

### 구조 분해 할당

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227200725-5259baad-0bc9-4a14-bcdc-5d93dc7bffa6.png" alt="이미지 설명"/>
  <figcaption>짝꿍..</figcaption>
</figure>

스프레드 문법도 충분한 것 같은데 더 있다..!  
바로 **`Desturcturing Assignment(구조 분해 할당)`** 이다.  
구조화된 배열과 같은 이터러블 또는 객체를 destructuring(분해)하여 그 값을 개별 변수에 담을 수 있다.

먼저 이터러블이 등장하기 이전인 ES5에서 배열을 분해해보자.

#### 1. 배열 구조 분해 할당

```js
const arr = [1, 2, 3];
const one = arr[0];
const two = arr[1];
const three = arr[2];
console.log(one, two, three); // 1 2 3
```

보통 이런식으로 작성했을 것이라고 생각한다.  
ES6에서는 이렇게 작성할 수 있다.

```js
const arr = [1, 2, 3];
const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3
```

오.. 뭔가 직관적이다.

**주의할점은 할당의 대상(우변)은 반드시 이터러블이어야 하고, 할당 기준은 배열의 인덱스이다.**

이런 경우도 생각해볼 것 같다.
**'요소 개수를 반드시 일치 시켜야 하나?'**  
흔히 생각하는 적게 할당하는 경우와 초과하는 경우를 살펴보자.

```js
const arr = [1, 2, 3];
const [one, two] = arr;
console.log(one, two); // 1 2

// 배열의 인덱스를 초과하면 undefined가 할당된다.
const [one, two, three, four] = arr;
console.log(one, two, three, four); // 1 2 3 undefined
```

추가적인 기능으로 할당을 위한 변수에 기본값을 설정할 수 있다.

```js
const arr = [1, 2];
const [one, two, three = 3] = arr;
console.log(one, two, three); // 1 2 3

const [e, f = 10, g = 20] = [1, 2];
console.log(e, f, g); // 1 2 20
```

이처럼 배열 디스트럭처링 할당은 배열과 같은 이터러블에서 **필요한 요소만 쏙쏙 뽑아서 사용**하고 싶을때 아주 유용하다.

지금도 충분히 좋지만 더 쓸모있게 사용해보자.
실제 프로젝트에 활용할만한 구조 분해 할당 예시이다.

```js
function parseUrl(url = "") {
  const parsedUrl = url.match(/([^?=&]+)(=([^&]*))/g) || [];
  console.log(parsedUrl);
  /* 
  [
    'https://chobae.github.io/posts/JS_Symbol/',
    'https',
    'chobae.github.io',
    'posts/JS_Symbol/',
    index : 0,
    input : 'https://chobae.github.io/posts/JS_Symbol/',
    groups : undefined
  ]
  */

  if (!parsedUrl) return {};
  // 구조 분해 할당을 통해 필요한 값만 추출!
  const [, protocol, host, path] = parsedUrl;
  return { protocol, host, path };
}
const parsedURL = parseUrl("https://chobae.github.io/posts/JS_Symbol/");
console.log(parsedURL);
/* 
{
  protocol: 'https',
  host: 'chobae.github.io',
  path: 'posts/JS_Symbol/'
} 
*/
```

url을 파싱하는 함수를 만들어봤다.  
필요없는 부분은 재끼고, 필요한 부분인 프로토콜, 호스트, 경로만 추출하여 객체로 반환해줬다.  
코드 자체의 양도 줄었고, 직관적이다.

마지막으로 구조 분해 할당을 위한 변수에 Rest 파라미터와 유사하게 **Rest 요소**를 사용할 수 있다.

```js
const arr = [1, 2, 3, 4, 5];
const [one, ...rest] = arr;
console.log(one, rest); // 1 [2, 3, 4, 5]
```

주의 해야 할점은 Rest 요소는 반드시 **마지막에 위치**해야 한다.

#### 2. 객체 구조 분해 할당

아직 끝나지 않았다.. 객체도 구조 분해 할당이 가능하다.

ES5에서 객체를 분해해보자.

```js
const user = { firstName: "bae", lastName: "Cho" };

const firstName = user.firstName;
const lastName = user.lastName;
console.log(firstName, lastName); // bae Cho
```

너무 자주 사용하는 패턴이라 익숙하고, 불편한지 모르겠다.
하지만 ES6에서는 이렇게 작성할 수 있다.

```js
const user = { firstName: "bae", lastName: "Cho" };
const { firstName, lastName } = user;
console.log(firstName, lastName); // bae Cho
```

객체 구조 분해 할당은 **객체의 프로퍼티 키를 기준으로 할당**된다.

만약 프로퍼티 키를 커스텀하게 설정하고 싶다면 이렇게 작성하면 되는데 약간 어색하다..

```js
const user = { firstName: "bae", lastName: "Cho" };
const { firstName: name, lastName: surname } = user;
console.log(name, surname); // bae Cho
```

뭔가 프로퍼티의 밸류로 할당하는 것 같아서 이상하지만 이렇게 꼭 작성해줘야한다.

예외적으로 중첩 객체일때 어떻게 작성해야할까?

```js
const user = {
  name: "chobae",
  address: {
    city: "seoul",
    country: "korea",
  },
};
const {
  name,
  address: { city, country },
} = user;
console.log(name, city, country); // chobae seoul korea
```

중첩 객체 내부의 프로퍼티 값에 접근하려면 위의 코드처럼 작성하면 된다.

이건 내 취향이지만 배열 디스트럭처링 할당보다 객체 디스트럭처링 할당은 뭔가 기본의 `.연산자`를 사용하는 것이 더 직관적인 것 같다.

## 📝 정리하자면..

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/227271089-ad6829a9-25d3-4ec1-b3a4-3dbaca7f66e5.png" alt="이미지 설명"/>
  <figcaption>어디로 가야하오..?</figcaption>
</figure>

_**이터러블**이란, **'순회 가능한 객체'** 를 의미하며, 여러 개의 요소를 가지고 있는 데이터 구조를 말합니다. 이터러블 객체는 ES6부터 도입된 **이터레이션 프로토콜(Iteration Protocol)을 준수**하는 객체입니다. 이터러블 객체는 **Symbol.iterator 메서드를 소유**하며, 이 메서드를 호출하면 **이터레이터를 반환**합니다. 이터레이터를 통해 순차적으로 **이터러블 객체의 요소를 탐색**할 수 있습니다. 이터러블의 예로는 **배열, 문자열, Map, Set** 등이 있습니다._

궁극적으로 이터러블이 등장하면서 데이터 자료구조는 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 `for … of` 문, `스프레드 문법`, `구조 분해 할당`의 대상으로 사용할 수 있도록 **일원화**했다고 할 수 있다.  
쉽게 말해 순회 방법을 통일하여 인터페이스 구성에 편리함을 제공했다고 하면 좋은 대답일 것 같다.

**혹시 잘못된 부분이 있다면 언제든지 댓글로 남겨주신다면 감사하겠습니다🫡**

## 🙏🏻 참고 Reference

---

- 모던 자바스트립트 Deep Dive
