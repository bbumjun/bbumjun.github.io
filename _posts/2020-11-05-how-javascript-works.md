---
layout: post
title: 자바스크립트의 동작 원리 [TIL]
tags: [javascript, browser, v8]
comments: true
---

자바스크립트가 어떻게 동작하는지 정리해보았다.

## 개요

자바스크립트를 사용해 개발하는 프론트엔드 개발자라면 자바스크립트의 내부 동작 원리에 대해 이해하고 있어야 한다고 생각한다. 대표적인 자바스크립트 엔진인 V8의 동작과 런타임에서의 동작을 정리해보자.

## 자바스크립트 엔진

브라우저에서 자바스크립트의 실행을 담당한다.
![](https://images.velog.io/images/bbumjun/post/5fa7f4ca-0f82-407c-a58b-b190206da0d0/image.png)

대부분의 자바스크립트 엔진의 구성요소는 변수, 함수 등의 메모리 할당이 일어나는 **메모리힙(Memory Heap)**, 실행중인 함수에 대한 정보를 담은 스택이 쌓이는 **콜스택(Call Stack)** 이렇게 두가지 요소로 구성된다.

## V8

![](https://images.velog.io/images/bbumjun/post/41739284-04d5-43b3-9fdb-06fd91f26adb/image.png)
V8은 크롬에서 사용되는 자바스크립트 엔진이다. 작동원리를 간단히 살펴보자.

>

1. V8은 자바스크립트 코드를 가져와 파서(Parser)에게 전달한다.
2. 파서는 코드를 분석해 AST(Abstract Syntax Tree)로 변환한다.
3. Ignition 이라고 불리는 인터프리터는 AST를 컴퓨터가 이해하기 쉬운 바이트코드로 변환한다.
4. 바이트코드가 실행되면서 자바스크립트가 실제로 작동하게 된다.
5. 바이트코드가 실행되는 과정에서 자주 사용되는 로직을 감지해서 TurboFan에게 넘긴다.
6. TurboFan은 재사용되는 코드를 최적화하여 메모리 오버헤드를 줄이고 더 빠른 실행속도를 내도록 한다.

## 런타임(브라우저)

자바스크립트는 싱글스레드 언어이기 때문에 단일 콜스택을 갖고 있고, 때문에 한번에 한가지의 작업만 수행할 수 있다. 멀티스레드 환경에서 발생하는 데드락과 같은 상황을 신경쓰지 않아도 된다는 장점이 있다. 하지만 시간이 매우 오래걸리는 작업이 실행되면 유저의 인터랙션, 화면의 렌더링 등 모든 작업은 긴 시간동안 작업이 끝나길 기다리면서 아무것도 못하게 된다. 브라우저에서는 이러한 동시성을 지원하기 위해 Web API, 이벤트루프, 콜백큐도 함께 동작에 관여한다.

## 동작과정

![](https://images.velog.io/images/bbumjun/post/6fc7eaba-fe5c-4c91-a8ef-26415eed93de/image.png)

위 그림과 함께 자바스크립트의 실행 과정을 아래와 같이 간단히 정리할 수 있다.

>

- 코드를 읽어내려가면서 함수 호출이 발견되면, 콜스택에 해당 함수의 정보를 담고 해당 함수를 실행한다. 단, 해당 함수가 비동기 함수일 경우, 콜백의 전달과 함께 브라우저에게 해당 Web API의 실행을 맡기고 함수가 바로 종료되며 콜스택에서 제거된다.
- Web API는 작업을 완료했을 경우 콜백 큐에 콜백함수를 전달한다.
- 이벤트루프는 콜스택을 계속 감시하고 있다가, 콜스택이 비어있을 경우, 즉 아무 함수도 실행되고 있지 않을 때 콜백큐에 대기중인 콜백이 있는지 체크한 후 콜백큐 맨앞의 콜백함수를 콜스택에 전달해 실행시켜준다.

---

참고한 글

[https://joshua1988.github.io/web-development/translation/javascript/how-js-works-inside-engine/#%EB%93%A4%EC%96%B4%EA%B0%80%EA%B8%](https://joshua1988.github.io/web-development/translation/javascript/how-js-works-inside-engine/#%EB%93%A4%EC%96%B4%EA%B0%80%EA%B8%)

[https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EC%97%94%EC%A7%84-%EB%9F%B0%ED%83%80%EC%9E%84-%EC%BD%9C%EC%8A%A4%ED%83%9D-%EA%B0%9C%EA%B4%80-ea47917c8442](https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-%EC%97%94%EC%A7%84-%EB%9F%B0%ED%83%80%EC%9E%84-%EC%BD%9C%EC%8A%A4%ED%83%9D-%EA%B0%9C%EA%B4%80-ea47917c8442)

[https://evan-moon.github.io/2019/06/28/v8-analysis/](https://evan-moon.github.io/2019/06/28/v8-analysis/)

[https://meetup.toast.com/posts/89](https://meetup.toast.com/posts/89)
