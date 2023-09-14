---
layout: post
title:  "풀스택 웹개발(1) - API와 REST API"
date:   2023-09-09 12:40:48 +0900
categories: ycTechAcademy
---


## 소개
[풀스택 웹개발] 시리즈는 Spring을 활용하여 풀스택 웹사이트를 만드는 것을 목표로 합니다.
빠르고 직관적인 이해를 돕기 위해 전문적인 용어보다는 비유적인 표현을 많이 사용했습니다.
웹사이트를 처음 만들어보는 분도 쉽게 배우고 따라하는 웹개발 설명서가 되었으면 합니다.

본 시리즈가 다루는 주제는 크게 다음과 같습니다:
> - API, REST API
> - Spring
> - RDBMS 중급 활용
> - AWS 클라우드 컴퓨팅
> - 웹서비스 배포
> - 마이크로서비스 아키텍처

다음과 같은 내용을 학습하시면 본 시리즈를 이해하는 데 도움이 됩니다:
> - 객체지향(Opject-oriented-programming)
> - Java
> - SQL

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### Table of Contents
---
1. [Introduction](#소개)
2. [API](#api)
3. [REST API](#rest-representational-state-transfer-api)

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## API

# API (Application Programming Interface)

API는 프로그램이 서로 정보를 주고받을 때 사용하는 약속된 문장입니다.

식당에 가면 손님은 직접 음식을 요리해 먹지 않습니다. 웨이터에게 주문하면 웨이터는 조리실에 주문 내역을 전달하고, 조리실은 주문에 따라 음식을 만들어 내놓죠. 여기서 식당을 웹사이트(Website), 웨이터를 API, 주문 내용을 파라미터(Parameter), 조리실을 서버(Server), 음식을 응답(Response)로 이해할 수 있습니다.

&nbsp;
![웨이터 사진](https://img.freepik.com/free-photo/we-serve-best-cakes_637285-7884.jpg?w=2000)
##### 이미지 출처: Freepik

> - 식당 = Website
> - 웨이터 = API
> - 주문 = Parameter
> - 음식 = Response (응답)

&nbsp;

주문을 할 때 웨이터는 손님에게 아무거나 물어보지 않습니다. 정해진 질문과 선택지가 있고, 손님은 이 중에서 골라서 답변을 하죠. 웨이터가 메뉴(첫 번째 parameter)를 물어봤고, 메뉴로 스테이크를 골랐다면 웨이터는 굽기 정도(두 번째 parameter)는 어느 정도로 원하는 지 물어볼 겁니다. 레어로 주문해보겠습니다. 그러면 전체 API는 다음과 같이 작성할 수 있습니다:

`https://스테이크하우스/주문?메뉴=스테이크&굽기=레어`

여기서 `https://스테이크하우스` 는 **Base URL**, `/주문` 은 **Endpoint**라고 부릅니다. 메뉴와 굽기는 **Parameter**이며, **&**로 연결합니다.

&nbsp;

이제 실제 예시를 보겠습니다. 유튜브 검색 API는 검색창에 입력한 키워드를 서버로 전송하고, 검색된 동영상의 정보를 받아오는 역할을 수행합니다. 유튜브에 그냥 접속하면 주소창에 <https://www.youtube.com>이라는 Base URL이 나오죠.

&nbsp;

![유튜브 고양이 검색 gif](https://www.wpromote.com/wp-content/uploads/2020/04/youtube-suggest-1.png)
##### 이미지 출처: Wpromote

&nbsp;

그런데 유튜브 검색창에 "고양이"를 검색해 보면 주소창이 아래처럼 자동으로 변한 것을 확인할 수 있습니다.

<https://www.youtube.com/results?search_query=고양이>

여기서 <span style="color:#0000EE">results?search_query=고양이</span>가 Base URL의 뒷부분에 추가된 게 보이죠.
하나씩 풀어보자면 다음과 같습니다.

```
results         (검색 결과를 불러오는데)
?               (무엇을 기준으로 가져오지?)
search_query    (검색어를 기준으로)
=고양이         (그리고 그 검색어는 '고양이'야)

```

즉, '고양이'라는 키워드를 검색어로 결과를 가져와 달라는 뜻입니다.
그러면 강아지를 검색하면 주소창이 어떻게 나올 지 예상이 되실 겁니다

<https://www.youtube.com/results?search_query=강아지>

&nbsp;

#### 사족 - 인터페이스
> API를 잘 이해하기 위해서는 인터페이스가 무엇인지 살펴보면 도움이 됩니다. API의 I는 Interface에서 왔는데요, 인터페이스는 서로 다른 상대가 상호작용을 할 수 있도록 연결해주는 매개체입니다. 키보드나 마우스는 사람이 입력하는 내용을 컴퓨터에게 전달해 주는 인터페이스이며, 터치스크린은 스마트폰에게 정보를 전달하는 인터페이스입니다. 사람과 컴퓨터 사이의 소통처럼 소프트웨어끼리도 정보를 전달하는 매개체가 필요합니다. 이 때 활용하는 인터페이스가 바로 API입니다. API는 웹사이트에서 서비스를 호출하고 정보를 주고받는 역할을 합니다.

&nbsp;

#### 사족 - API Documentation
> API는 서비스를 제공하는 애플리케이션마다 다릅니다. 그리고 API는 여러 사람이 사용하기 때문에 API 사용 방법을 잘 정리해 놓은 설명서, 즉 API Documentation을 자세히 작성하는 것이 매우 중요합니다. 만약 제가 집을 청소하는 로봇을 만들어서 판매한다면, 전원을 켜고 끄는 방법이나 청소 모드의 종류를 설명서에 잘 적어 놔야겠죠. 이처럼 API Documentation에는 Endpoint와 사용 예시, 응답 형식과 예시, 그리고 에러의 종류 등을 적어놓습니다.

> API Documentation 작성 방법과 예시는 따로 포스트에서 설명해드리겠습니다.

&nbsp;
&nbsp;

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

## REST (REpresentational State Transfer) API
REST API는 2000년도에 로이 필딩(Roy Fielding)이 최초로 소개한 아키텍처로, 웹(HTTP)를 잘 활용할 수 있도록 특별히 고안된 API 아키텍처(스타일)입니다. 즉, REST 형식에 맞게 작성한 API는 모두 REST API라고 부릅니다.

&nbsp;

# REST API의 구성
REST API는 `HTTP METHOD` + `URL` 형식으로 요청을 보내면, `JSON` 형식으로 응답이 오는 구조를 가지고 있습니다. `JSON`은 .pdf, .jpeg 처럼 데이터를 저장하는 파일 형식입니다.

![REST API의 구조](https://images.ctfassets.net/vwq10xzbe6iz/5sBH4Agl614xM7exeLsTo7/9e84dce01735f155911e611c42c9793f/rest-api.png)

앞선 예시에서 살펴봤던 API도 모두 REST 구조를 가지고 있습니다. 앞에 `HTTP METHOD`가 생략되어 있지만, `HTTP METHOD`까지 붙이면 유튜브 검색 API는 다음과 같이 작성할 수 있습니다.

`HTTP GET https://www.youtube.com/results?search_query=강아지`

`HTML METHOD`는 HTML에 보내는 요청의 종류를 정의합니다. 주로 사용하는 종류로는 `GET`, `POST`, `DELETE`, `PUT`이 있습니다. 이름으로부터 직관적으로 알 수 있듯이, `GET`은 서버로부터 정보를 가져오는 요청을, `POST`서버에 새로운 데이터를 추가하는 요청을, `DELETE`는 삭제하는 요청을, `PUT`은 이미 서버에 존재하는 데이터를 변경하는 데 사용합니다. 사용 예시를 볼까요?

```
HTTP GET      https://website/user?id=1       (아이디가 1인 유저를 불러오는 API)
HTTP POST     https://website/user?id=1       (아이디가 1인 유저를 새로 생성하는 API)
HTTP DELETE   https://website/user?id=1       (아이디가 1인 유저를 삭제하는 API)
```

&nbsp;

# REST API의 특징
그러면 왜 웹사이트에서는 REST API를 사용해 요청을 주고받는 걸까요? REST API는 몇 가지 특징이자 장점이 있습니다.

- **Client-Server Architecture**

  REST는 클라이언트(client, 사용자)가 서버(server)에 요청을 보내면, 서버가 클라이언트에게 응답을 보내는 정해진 구조(architecture)를 가지고 있습니다. 요청을 주고받는 방향이 명확히 정해져있기 때문에 개발이 수월합니다.

- **Statelessness**

  State는 상태를 의미합니다. REST API는 필요한 모든 정보를 포함하고 있기 때문에 (어떤 요청을 어디에 보낼 건 지, 각 파라미터는 무엇인 지 등) 따로 현재 상태를 저장할 필요가 없습니다. 때문에 세션 정보나 쿠키가 불필요해서 서버의 저장 용량을 사용하지 않고, 정보의 처리 과정도 훨씬 단순해집니다.

- **Cacheability**

  REST를 사용하면 클라이언트가 기존에 어떤 요청을 주고받았는 지 기록을 남겨놓기 때문에 똑같은 요청을 보낼 경우 훨씬 빠르게 응답을 받을 수 있습니다. 도서관에서 어떤 책을 찾을 때 처음에는 헤매지만, 같은 책을 두 번째로 찾을 때는 더 빨리 찾을 수 있는 것과 같은 원리입니다.

- **Uniform interface**

  REST는 한정된 인터페이스로 상호작용 방식을 통일했습니다. HTTP METHODS를 알고 있으면 각 API가 어떤 역할을 수행할 지 유추하기 수월합니다.

- **Layered System**

  REST는 계층(레이어)화 된 시스템을 사용합니다. 서버와 클라이언트가 소통하는 과정에서 REST API는 사용자 인증이나 로그인 정보 관리 등의 역할을 각 레이어에 할당합니다. 그렇기에 서비스 유지 보수가 수월하며, 클라이언트는 가장 근접한 레이어와만 상호작용하면 됩니다.

  요약하자면 REST API는 유연성, 확장성, 그리고 간결성을 갖춘 웹 서비스를 구축하는 것을 가능하게 해줍니다. 이러한 특성 덕분에 많은 웹 애플리케이션과 모바일 애플리케이션은 REST API를 사용하고 있습니다.

---

&nbsp;
&nbsp;
&nbsp;
&nbsp;
&nbsp;

#### 자료 출처
- YC Tech Academy 1주차
- 그림으로 배우는 프로그래밍 구조 (마스이 토시카츠 저, 김성훈 역)
- <https://www.redhat.com/en/topics/api/what-is-a-rest-api>
- <https://restfulapi.net/http-methods/>
- <https://www.scrapingbee.com/blog/six-characteristics-of-rest-api/>