---
layout: post
title:  "Express 01 - 소개"
date:   2020-09-13 22:15:00 +0900
categories: Express(Node.js)
---

# Express

## Express 란?

- 공식 사이트 첫 페이지의 내용입니다.
- Node.js를 위한 빠르고 개방적이고 간결한 웹 프레임워크
- [웹 애플리케이션] Express는 웹 및 모바일 애플리케이션을 위한 일련의 강력한 기능을 제공하는 간결하고 유연한 Node.js 웹 애플리케이션 프레임워크입니다.
- [API] 자유롭게 활용할 수 있는 수많은 HTTP 유틸리티 메소드 및 미들웨어를 통해 쉽고 빠르게 강력한 API를 작성할 수 있습니다.
- [성능] Express는 기본적인 웹 애플리케이션 기능으로 구성된 얇은 계층을 제공하며, 여러분이 알고 있고 선호하는 Node.js 기능을 모호하게 만들지 않습니다.
- [Frameworks] 많은 유명한 프레임워크들이 Express를 기반으로 하고 있습니다.

## Exrpess 설치

- npm install express —save

## Hello world 예제

- myapp 디렉토리에 app.js라는 이름의 파일을 작성한 후 다음과 같은 코드를 추가하세요.

```jsx
const express = require('express');
const app = express();
const port = 3000

app.get('/', (req, res) => {
	res.send('Hello World');
});

app.listen(port, () => {
	console.log(`Example app listening at http://localhost:${port}`);
});
```

- 실행

```bash
node app.js
```

## Express Generator

- 공식 문서에 애플리케이션의 골격을 신속하게 작성하려면 애플리케이션 생성기 도구인 express generator를 사용하라고 나와있습니다.
- 생성기에 의해 작성된 앱 구조는 Express 앱을 구조화하는 여러 방법 중 하나에 불과하다고 합니다. 이러한구조를 사용하거나 사용자의 요구사항에 가장 적합하도록 구조를 수정하라고 합니다.

```bash
npm install express-generator -g

express --view=pug myapp
```

- 다음과 같은 디렉토리 구조를 갖습니다.

![DirectoryStructure](/public/img/Nodejs-express-01-1.jpg)

- **app.js**는 서버의 설정을 담아놓은 파일이라고 생각하면 됩니다. 웹 애플리케이션 서버에 설정할 모듈을 포함시키면 됩니다.
- **public**은 서버가 public으로 제공할 정적 파일들, 예를 들어 js 파일, css 파일, 이미지 파일 등을 저장해놓는 디렉토리입니다.
- **routes**는 서버가 라우팅할 url path에 대한 로직들을 저장해놓는 파일입니다.
- **views**는 서버가 렌더링하는 템플릿들을 저장해놓는 디렉토리입니다. pug로 설정했기 때문에 pug파일들이 보입니다.

## 출처

- express 공식 문서
- 링크 : [https://expressjs.com/](https://expressjs.com/)
- 라봉이의 개발 블로그
- 링크 : [https://psyhm.tistory.com/2?category=654716](https://psyhm.tistory.com/2?category=654716)