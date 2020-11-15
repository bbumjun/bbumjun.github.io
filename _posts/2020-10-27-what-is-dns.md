---
layout: post
title: DNS가 뭘까
subtitle: DNS가 어떻게 동작하는지 알아보자
tags: [DNS]
comments: true
---

태어나서 처음으로 봤던 직무면접에서 가장 먼저 들어왔던 질문은 이것이었다.

> 브라우저 주소창에 "www.naver.com"을 입력했을 때 DNS 서버에서의 처리 과정과 브라우저의 동작과정을 통틀어서 설명해주세요.

제대로 대답하지 못했기 때문에 다음에는 완벽하게 대답할 수 있도록 **DNS가 무엇이고 DNS의 동작 과정**부터 정리해보려고 한다.

### DNS란?

`Domain Name System` 의 줄임말로, 사람이 이해하기 쉬운 `www.naver.com`과 같은 **Domain name** 을 사용해 요청했을 때, 호스트의 **IP주소** `ex) 66.249.66.60` 로 변환하거나 그 반대의 변환을 수행하기 위해서 개발된 시스템이다.

### Domain name의 구조

모든 Domain name은 아래의 그림과 같은 역트리(inverted tree) 구조에 따라서 지정해서 등록하게 된다. 1단계부터 차례대로 TLD(Top Level Domain), SLD(Second Level Domain), SubDomain이라고 한다.
![](https://images.velog.io/images/bbumjun/post/3595c2ac-5cb9-4d60-8e40-c87c771df939/image.png){: .mx-auto.d-block :}

등록된 Domain 은 `Subdomain.SLD.TLD` 또는 `Subdomain.TLD`의 구조를 갖게 된다.

### DNS 동작 원리

DNS는 아래 그림의 순서대로 동작한다.
![](https://images.velog.io/images/bbumjun/post/7fb1b222-644f-4401-b9ea-511818f5b678/image.png){: .mx-auto.d-block :}

1. 사용자가 브라우저에 `www.naver.com` 을 입력하는 순간 클라이언트에 저장된 Local DNS 서버 IP에게 `www.naver.com` 의 ip 주소를 물어본다.

2. Local DNS 서버는 우선 자신의 캐시에 해당 Domain의 ip 주소가 저장되어있는지 확인하고, 없다면 Root DNS 서버에 `www.naver.com`의 ip를 물어본다.
   Root DNS 서버는 TLD 서버들의 IP주소 테이블을 참조해, 이 중 com 네임서버의 ip주소를 Local DNS 서버에 보낸다.

3. Local DNS는 응답받은 com 네임서버로 `www.naver.com`을 보낸다. 응답받은 com 네임서버도 마찬가지로 com 네임서버를 사용하는 도메인들의 ip테이블을 참조해 `naver.com` 네임서버의 ip주소를 보낸다.

4. 마지막으로 `naver.com` 네임서버에 `www.naver.com` 요청을 보낸다. `www.naver.com` , `d2.naver.com` 등 `naver.com`을 사용하는 모든 도메인 중 `www.naver.com`에 대한 ip 주소를 찾아 Local DNS 서버에게 응답한다.

5. Local DNS는 응답받은 ip주소를 캐시에 저장하고 클라이언트에게 보낸다.

---

참고한 글

- [DNS서버(네임서버)의 이해](https://webdir.tistory.com/161)
- [로컬 DNS서버](https://yang1650.tistory.com/21)
