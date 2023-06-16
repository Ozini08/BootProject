# BootProject
springboot + react

First Settings

* React reference Docs
https://ko.legacy.reactjs.org/docs/getting-started.html
https://create-react-app.dev/docs/getting-started

1. 'start.spring.io' 에서 프로젝트 생성
  참고
	id 'java'
	id 'org.springframework.boot' version '2.7.12'
	id 'io.spring.dependency-management' version '1.0.15.RELEASE'

2. React 설치
  iterm / terminal 에서 코드 실행
  cd src/main
  npx create-react-app frontend	# npx create-reeact 프로젝트명

3. 실행해서 확인
  cd frontend	# cd 프로젝트명
  npm start

4. frontend 폴더에 모듈 설치
  npm install http-proxy-middleware --save

5. src/main/frontend/src 폴더에서 setupProxy.js 파일을 생성 (아래 코드 기입)
  // src/main/frontend/src/setupProxy.js

const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
  app.use(
    '/api',
    createProxyMiddleware({
      target: 'http://localhost:8080',	# 서버 URL or localhost:설정한포트번호
      changeOrigin: true,
    })
  );
};

6. frontend 폴더에 axios 설치
npm install axios --save

7. axios 통신 테스트를 위한 App.js 수정
  // src/main/frontend/src/App.js

import React, {useEffect, useState} from 'react';
import axios from 'axios';

function App() {
   const [hello, setHello] = useState('')

    useEffect(() => {
        axios.get('/api/hello')
        .then(response => setHello(response.data))
        .catch(error => console.log(error))
    }, []);

    return (
        <div>
            백엔드에서 가져온 데이터입니다 : {hello}
        </div>
    );
}

export default App;

8. axios 통신 테스트를 위한 Controller 작성
  // src/main/java/com.demogroup.demoweb/Controller/HelloWorldController.java

package com.demogroup.demoweb.Controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorldController {

    @GetMapping("/api/hello")
    public String test() {
        return "Hello, world!";
    }
}

본 글은 https://velog.io/@u-nij/Spring-Boot-React.js-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EC%84%B8%ED%8C%85 블로그 참조하였음
