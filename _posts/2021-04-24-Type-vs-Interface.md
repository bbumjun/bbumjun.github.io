---
layout: post
title: Type 과 Interface 의 차이
tags: [typescript]
comments: true
---

# Type 과 Interface 차이

타입스크립트에서 새로운 타입을 선언해야 할 때 `type`과 `interface`중 어떤 것을 적절히 사용해야 하는지 궁금해 검색해보았다.

`type`과 `interface`는 매우 유사하고, 둘 중 아무거나 자유롭게 사용해도 된다. 또한 `interface`의 대부분의 기능은 `type`에서도 가능하다.

가장 큰 차이점은 type은 한번 생성된 이후로 수정할 수 없지만 `interface`는 아래처럼 다시 선언해 기존의 `interface`에 필드를 추가하는 것이 가능하다.

```typescript
interface Person {
  name: string;
}

interface Person {
  age: number;
}

const person = {
  name: "james",
  age: 20,
};
```

그리고 type 또는 interface의 확장할 때의 문법이 살짝 다르다.

`interface`를 확장할 때

```typescript
interface Animal {
  name: string;
}

interface Bear extends Animal {
  honey: boolean;
}

const bear = getBear();
bear.name;
bear.honey;
```

`type`을 확장할 때

```typescript
type Animal = {
  name: string;
};

type Bear = Animal & {
  honey: Boolean;
};

const bear = getBear();
bear.name;
bear.honey;
```

> 출처: [타입스크립트 공식문서](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)
