---
layout: post
title: UseSwr 사용시 데이터 요청이 무한정 발생하는 오류
tags: [swr]
comments: true
---

검색어 value 가 바뀔 때마다 데이터를 가져오도록 아래와 같이 작성했다.

```javascript
const { data, error } = useSwr(
  value.trim() ? ["search/multi", { query: value }] : null,
  fetcherWithParams
);
```

![](https://images.velog.io/images/bbumjun/post/1fa3a013-c44b-4689-b59f-e8d91e8acf54/image.png)

위처럼 입력값이 바뀌지 않았음에도 계속해서 요청하는 문제가 생겼다...
컴포넌트도 input 값이 바뀔 때에만 리렌더링되었기 때문에 이유를 알수 없었다.

[구글링](https://github.com/vercel/swr/issues/113) 하면서 문제의 이유를 깨달았다. useSwr의 매개변수는 hook의 dependancy와 같은 방식으로 동작하기 때문에 `{query: value}` 와 같이 인라인으로 생성한 객체를 넘기면 매번 새로운 객체로 판단해 hook을 무한히 실행하게 된다. 아래와 같이 해결할 수 있다.

```javascript
const { data } = useSwr(
  value.trim() ? ["search/multi", value] : null,
  (url, value) => fetcherWithParams(url, { query: value })
);
```
