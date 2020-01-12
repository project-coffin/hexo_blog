---
title: [PWA-TUTORIAL#1] pwa란 / manifest 파일 생성
date: 2020-01-12 16:49:41
author: Roseline Song
categories: [Daily-Study, PWA]
tags: 기록 공부
---

### 레퍼런스

- [NET-NINZA PWA TUTORIAL](https://youtu.be/AlEdGOLhuM8?list=PL4cUxeGkcC9gTxqJBcDmoi5Q2pzDusSL7)
- [Google Developer Site](https://developers.google.com/web/fundamentals/web-app-manifest/)


### [What is Progressive Web App?](https://youtu.be/4XT23X0Fjfk?list=PL4cUxeGkcC9gTxqJBcDmoi5Q2pzDusSL7)

하나의 코드로 모든 플랫폼을 커버한다.

1. 모바일 홈스크린에 설치 가능
2. 오프라인일 때도 앱에 접근 가능
3. 푸시 알림 가능

🧚‍♀️html, css, javascript로 만들 수 있다.

🧚‍♀️modern web browser(크롬 같은) 사용할 것을 추천.


### manifest 파일 생성

1. manifest file
    - manifest file: 브라우저에게 내 앱에 대해 간단하게 알려주는 파일
    - 그리고 사용자의 데스크탑이나 모바일에 설치됐을 때 어떤 행동을 취해야 하는지 알려줌
    - 홈스크린 프롬프트에 앱을 띄우기 위해서는 manifest file을 만드는 것이 선행되어야 한다.

2. manifest.json

```json
    {
      "short_name": "Maps",
      "name": "Google Maps",
      "icons": [
        {
          "src": "/images/icons-192.png",
          "type": "image/png",
          "sizes": "192x192"
        },
        {
          "src": "/images/icons-512.png",
          "type": "image/png",
          "sizes": "512x512"
        }
      ],
      "start_url": "/maps/?source=pwa",
      "background_color": "#3367D6",
      "display": "standalone",
      "scope": "/maps/",
      "theme_color": "#3367D6"
    }
```

- *short_name: 홈스크린에서 보여질 앱 이름
- *name: 앱 인스톨 프롬프트에서 보여진다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e011ede1-e779-43e9-9727-93de2b08da14/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e011ede1-e779-43e9-9727-93de2b08da14/Untitled.png)

- icons: 실행할 때, 홈 스크린에서 보여질 때, 스플래시 화면에서 보여질 아이콘들
    - src: 이미지 경로
    - type: 파일 확장자
    - sizes: 이미지 사이즈
- start_url: 실행될 때 처음 시작하는 url
- backgroud_color: 스플래시 화면 배경색
- display: 브라우저 ui를 정해서 보여줄 수 있다.
    - standalone: 네이티브 앱처럼 보이는 ui, 주소창 같이 브라우저처럼 보이는 ui 는 제외하고 보여줌
    - [그 외는 페이지의 display 항목 참고](https://developers.google.com/web/fundamentals/web-app-manifest/)
- orientation: 방향
- scope: 브라우저가 내 앱에 있다고 여겨지는 url 집합들. scope에 정의된 url 이외의 url에 접근하면, 앱을 떠난 것으로 인식할 수 있다. scope를 통해서, entry, exit point를 컨트롤할 수 있다. start_url 역시 scope안에 있어야 한다.
    - 예시

        "scope": "/maps/"
        "start_url": "/maps/?source=pwa"

- theme_color: 툴바 색깔

2. 브라우저에 내 manifest 알려주기: manifest.json 파일을 만든 후, 존재하는 html 파일에 알리기

- index.html, about.html, contact.html, ...

    <head>
    	...생략...
      <link rel="manifest" href="/manifest.json">
    </head>

3. 내 manifest 확인

- 브라우저 - 개발자 도구 - application에서 manifest.json 파일 확인 가능
- 
![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7ed86252-c930-4417-b97d-b0549760ebd0/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7ed86252-c930-4417-b97d-b0549760ebd0/Untitled.png)
