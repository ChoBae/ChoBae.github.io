---
layout: post
title: 하이라이터즈 JS -> TS로 마이그레이션 01
date: 2023-03-28 23:00:00 +0900
categories: [JavaScript, TypeScript, Hightlighters]
tags: [JavaScript, TypeScript, Hightlighters]
description: 자바스크립트, 타입스크립트, 마이그레이션, 하이라이터즈
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

![image](https://user-images.githubusercontent.com/101175828/233026951-cbc98fea-128b-4941-8c3a-69e1c0cddc9f.png)

> 업보 청산의 시간이다. 대공사인 JS -> TS 마이그레이션을 시작한다.

# 🌈 TS로 마이그레이션을 시작하며

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233029727-c766970e-b346-40f5-9bcb-5dce6e1c9233.png"/>
  <figcaption>끔찍하다..</figcaption>
</figure>

프로젝트를 진행하면서 욕심이 많았고, 당연히 타입 스크립트를 써야지 하고 당당하게 프로젝트를 셋업 했다.  
결과는 수많은 any의 악수 요청 발생했다.. 기능을 구현하는데 급급했고, 첫 단추로 any를 끼우면서 걷잡을 수 없는 늪에 빠져버렸다.  
프로젝트가 끝난 지금, 그 늪에서 빠져나오고자 한다.

# ﹖ 타입스크립트를 쓰는 이유

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233838274-15d45b67-7e42-468e-8f59-4b22a181fa1b.png"/>
  <figcaption>응 안돼~</figcaption>
</figure>

1. **정적 타입 지원:** 타입스크립트는 변수, 함수 매개변수, 반환값 등에 명시적인 타입을 지정할 수 있어, 개발자의 의도를 명확하게 표현하고 버그를 줄여준다.
2. **에러 감지**: 정적 타입 체크를 통해 코드 작성 시점에 잠재적인 오류를 찾아내고 수정할 수 있어, 런타임 에러를 줄일 수 있다.
3. **개발 도구 지원**: 타입스크립트와 호환되는 통합 개발 환경(IDE)과 텍스트 에디터는 자동 완성, 리팩토링, 타입 정보 표시 등의 기능을 제공해 개발 생산성을 높여준다.
4. **코드 가독성 및 유지보수**: 명시적 타입 정보를 포함한 코드는 이해하기 쉽고, 기능 변경이나 버그 수정 시 코드를 안전하게 수정할 수 있도록 도와준다.
5. **객체지향 프로그래밍 지원**: 클래스, 인터페이스, 상속 등 객체지향 프로그래밍의 주요 요소를 사용할 수 있어, 코드의 재사용성과 모듈성을 높여준다.

더 많은 이유들도 있지만 그 중 중요한 부분만 간추려봤다.  
뭔가 많지만 결국에는 **'개발자의 생산성'** 을 높이기 위함이라고 생각한다.  
특히 프로젝트가 커질수록 타입스크립트의 필요성도 커진다고 생각된다.
직접 적용해보면서 필요성에 대해 더욱 인지하게 됐다.  
나처럼 뒤늦게 타입스크립트를 적용하는게 아니라 당장은 힘들더라도 배우면서 프로젝트를 진행했으면 좋겠다🥲

# 🛠️ 견적짜기

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233839169-40822e77-275c-436d-97d6-f00657c081a0.png"/>
  <figcaption>138개..?</figcaption>
</figure>

이제부터 138개의 any무리들을 제거할 것이다.  
한 두개의 컴포넌트를 마이그레이션 하면서 느낀 것은 연관된 컴포넌트를 잘 파악하고 해야 한다는 것이다.  
**중복으로 자주 사용되는 인터페이스는 types 폴더를 만들어서 관리**하고,

<figure style="text-align:center">
  <img  src="https://user-images.githubusercontent.com/101175828/233839541-75f80145-aa2a-47f9-8a94-8e60fb0a302d.png"/>
  <figcaption>루트 디렉토리 위치에 생성했다</figcaption>
</figure>

자주 사용하지 않고 **단독으로 사용되는 인터페이스는 컴포넌트 내부에서 선언**해줬다.

<figure style="text-align:center">
  <img  src="https://user-images.githubusercontent.com/101175828/233839659-9f8b7101-acbb-49b6-ada7-8cde5dce297d.png"/>
  <figcaption>사실 작명이 제일 어렵다..</figcaption>
</figure>

이정도의 규칙을 가지고 마이그레이션을 진행했고, 투자할 시간이 많이 없다보니 138개를 모두 지우는데 얼마나 걸릴지 모르겠지만 가보자 🔥

# 🎯 Feed

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233839902-861c42a6-0691-41a7-8b94-9ec22a005346.png"/>
  <figcaption>처음부터 월척이다..🎣</figcaption>
</figure>

피드 컴포넌트는 우리 프로젝트에서 가장 많이 재사용하는 컴포넌트이자 가장 복잡한 컴포넌트이다.  
정확한 타입 선언을 위해 서버에서 데이터를 가져와서 타입을 선언해줬다.

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233841360-d5859552-44eb-4c1f-9e8c-d68de2cf19e5.png"/>
  <figcaption>너~~무 복잡하다..</figcaption>
</figure>

앞 서 말했듯이 **피드 파입은 자주 재사용하기 때문에 types 폴더에 'Feed.ts' 파일을 만들어서 관리**했다.

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233841729-ebfaecac-f948-4133-838b-9795adc2196d.png"/>
  <figcaption>자세히 보면..</figcaption>
</figure>

내부적으로 댓글, 하이라이트, og 데이터, 태그, 유저 정보 등 다양한 데이터를 사용한다.  
처음에는 데이터의 타입을 파일을 각각 나누어 작성해야하나 고민했지만 모두 **피드에 깊은 의존성이 있다고 생각해서 `Feed.ts` 하나의 파일에 작성**했다.

```ts
import { UserInfo } from "./user";
import { TagInfo } from "./tag";
export interface FeedInfo {
  id: number;
  title: string;
  og: OgInfo;
  url: string;
  comment: Comment[];
  highlight: Highlight[];
  createdAt: string;
  tag: TagInfo[];
  user: UserInfo;
  bookmark: number[];
  summary: string;
}
export interface OgInfo {
  title: string;
  description: string;
  image: string;
  id: number;
}

export interface Highlight {
  color: string;
  contents: string;
  createdAt: string;
  feed_id: number;
  group_id: number;
  id: number;
  type: number;
  user: UserInfo;
}

export interface Comment {
  contents: string;
  createdAt: string;
  feed_id: number;
  id: number;
  user_email: string;
}
```

유저, 태그 타입은 임포트해서 사용했다.  
하나의 파일에서 관리하면서도 **타입을 잘 분리해서 관리**할 수 있었다.

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233841971-b0e0dca4-98f1-4965-ae75-7a95e04ca53c.png"/>
  <figcaption></figcaption>
</figure>

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233842396-a24fc06d-cb8b-4265-a842-3482b83a1c88.png"/>
  <figcaption>편안</figcaption>
</figure>

피드 관련 타입 선언을 해주고 나니 순식간의 11개의 any를 처리해줄 수 있었다.  
사실 어느 정도 완성된 프로젝트를 마이그레이션 하는 것이여서 아쉬움이 크다.  
만약 개발 중이었다면 타입스크립트의 장점을 더 이용해먹을 수 있었을 것 같아서 아쉽다.

# 🚨 마무리

---

<figure style="text-align:center">
  <img src="https://user-images.githubusercontent.com/101175828/233842726-adc1bb43-185e-4cbe-8f9c-471bb6dc1c9e.png"/>
  <figcaption>검거 완료</figcaption>
</figure>

오늘은 많은 데이터가 연관되어 있는 피드를 처리해봤는데 생각보다 오래 걸렸다.  
피드 내부의 데이터가 피드에 깊은 의존성이 있어서 한 파일에 묶어뒀는데 comment의 경우에는 충분히 재사용이 가능하다고 생각하기 때문에 분리해두는 것도 좋았을 것 같다.  
다음에 수정하다보면 각이 보이지 않을까 싶다.  
악의 any무리 126/138 to be continued..💫
