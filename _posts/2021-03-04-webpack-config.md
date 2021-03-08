---
layout: post
title: 까먹을까봐 써놓는 웹팩 설정
tags: [webpack]
comments: true
---

## 까먹을까봐 써놓는 웹팩 설정

### css loader

웹팩이 js 밖에 해석하지 때문에 css loader가 css 파일을 string 형태로 변환해준다.

### postcss-loader

웹팩이 postcss를 사용해 css를 처리할 수 있도록 하는 웹팩 로더. postcss는 다양한 플러그인으로 css 를 전처리할 수 있다. vendor prefix를 자동으로 적용하기 위해 autoprefixer를, css 의 바벨 역할을 하는 postcss-preset-env사용했다

### url-loader

폰트나 크기가 작은 이미지 파일 등을 파일을 직접 불러오지 않고 문자열 형태로 변환해서 번들파일에 넣어준다고 한다. 나는 폰트에 사용하려고 했는데 , 어째서인지 그냥 파일형태로 저장된다..

### mini-css-extract-plugin

style loader는 css파일을 head에 style 태그안에 내용을 넣어주는 반면에 이 플러그인은 따로 css 파일을 만들고 link로 연결해줌

### html-webpack-plugin

이 플러그인을 사용하면 bundle 한 js , css 파일들을 각각 html에 link 태그와 script 태그로 추가해주는 일을 자동화할 수있다.

### clean-webpack-plugin

빌드할 때 이전 빌드의 결과물이 삭제되지 않고 남아있는 경우가 있어 이를 해결해준다.
