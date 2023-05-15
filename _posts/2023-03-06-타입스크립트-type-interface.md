---
layout: post
title: 타입스크립트 type? interface?
date: 2023-02-25 15:11:33 +0900
categories: [Typescript]
tags: [Typescript, type, interface]
description: 타입스크립트 type? interface?
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
![image](https://user-images.githubusercontent.com/101175828/223529211-11456a01-5359-4ff3-b993-dd2f994d3d0c.png)

### 서론
---
이전에 진행했던 프로젝트의 기획단계에서 '기왕하는거 타입스크립트 쓰자!'라고 말했던 바보 멍청이는 프로젝트 내내 any를 난사하여 작성하였고,
이제는 프로젝트 코드 대부분을 변경해야하는 대공사를 시작하려한다.
그렇게 시작하자마자 타입을 정의하는 **`type`**과 **`interface`**라는 두갈래길이 있었다. 
프로젝트때처럼 일단 해보자라는 것보다 **이제는 뭐든 확실하게 알고 넘어가고 싶어서 정리**한다..!

### type
---
- 기존 타입에 별칭을 지어줄 수 있음 -> 인터페이스도 가능함.
- 다양한 타입을 결합하거나 조건부 타입을 만드는데 유용
- TS 1.4ver에 추가되었고, 기존 JS 문법과 호환성을 유지하면서 TS의 타입 기능 강화에 사용됨

```tsx

type MyConditionalType<T> = T extends number ? "number" : "not a number";


type Point = {
  x: number;
  y: number;
};
 // Point 타입을 pt라는 별칭을 이용해서 사용함
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });

// 객체 뿐만아니라 모든 타입에도 가능함. 아래 같이 유니언 타입에 대해서도 별칭 사용
type MyType = number | string;

```

### interface
---
- TS 도입때부터 사용
- 객체의 구조를 정의하는 데 주로 사용됨
- 객체의 프로퍼티 이름, 타입, 선택 여부 등을 정의
- TS docs에서는 **객체 타입을 만드는 또 다른 ‘방법'**이라고 정의함

```tsx
interface Point {
  x: number;
  y: number;
}
 
function printCoord(pt: Point) {
  console.log("The coordinate's x value is " + pt.x);
  console.log("The coordinate's y value is " + pt.y);
}
 
printCoord({ x: 100, y: 100 });

// 함수의 타입도 정의 가능함
interface MyFunction {
  (x: number, y: number): number;
}

```

### 차이점
---
- 가장 큰 차이점은 `타입 확장성`과 `호환성`
- 둘은 사실 매우 유사하고, `interface`가 가지는 대부분의 기능은 **`type`**에서도 동일하게 사용 가능
- **`type`**은 기존 타입에 별칭을 지어주는 것이기 때문에, 기존타입을 확장하거나 조작하는 데는 불편함
- `interface`는 기존 타입을 상속하거나 확장하는데 유용. 다른 인터페이스나 클래스와 상속을 통해 확장 가능

#### 1.확장 방식
- `interface`는 class처럼 **extends**를 통해 확장함

```tsx
// interface
interface Animal {
  name: string
}
// class처럼 extends로 상속을 통해 확장
interface Bear extends Animal {
  honey: boolean
}

const bear = getBear()
bear.name
bear.honey
```

- **`type`**은 **`&`**을 이용해서 확장함

```tsx
// type
type Animal = {
  name: string
}
// &을 이용해서 상속을 통해 확장
type Bear = Animal & {
  honey: Boolean
}

const bear = getBear();
bear.name;
bear.honey;
```
#### 2.필드 추가 방식
- **`interface`**는 기존의 인터페이스에 **새 필드를 추가가 가능함**

```tsx
interface Window {
  title: string
}
// 동일한 인터페이스 추가 가능
interface Window {
  ts: TypeScriptAPI
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});
```

- **`type`**은 생성된 뒤에는 **변경이 불가함**

```tsx
type Window = {
  title: string
}
// 동일한 인터페이스 추가시 아래의 에러 메세지 발생 
type Window = {
  ts: TypeScriptAPI
}

 // Error: Duplicate identifier 'Window'.
```

### 결론
---
![](https://velog.velcdn.com/images/chobae/post/086d1320-ca61-4961-b5da-27ae3bf11b49/image.png)

실제 TS Docs의 내용이다. ~~이게 진짜네?~~
결과적으로 나는 **`interface`**를 통한 타입선언을 할 생각이고, 이중 타입 선언 같은 유용한 기능을 사용하기에 **`type`**을 사용할 것이다.