> 💡 서버와 클라이언트가 인터넷상에서 데이터를 주고 받기 위한 프로토콜, **HTTP**

## HTTP란?

HTTP는 **H**yper**T**ext **T**ransfer **P**rotocol의 약자예요. 이름만 들으면 뭔지 잘 모르겠지만, 어딘가 낯이 익지 않나요?

제 블로그 주소인 https://velog.io/@dayo____me 앞에도 https가 붙어있죠? 이렇게 우리 사용하는 웹 사이트 모든 곳에는 http가 사용되고 있습니다.

물론 https와 http는 차이점이 있지만, 그건 다음 포스팅에 조금 더 다뤄보기로 할게요.

![](https://velog.velcdn.com/images/dayo____me/post/f5947360-40d9-4c58-be36-8ceb01e9c666/image.png)


예를 들어 우리가 컴퓨터와 전화를 한다고 가정해봅시다. 원활한 대화를 위해서 지켜야 할 사항이 몇 가지 있습니다.

첫 번째로, **우리는 서로 알아들을 수 있는 말을 사용**해야 해요.

예를 들어, 컴퓨터가 우리에게 전화해서 냅다 모르는 외국어로 말을 건다면 당황스럽겠죠? 적어도 무슨 언어인지 얘기해줘야 번역기라도 돌려볼 수 있을 거에요. 우리도 컴퓨터가 당황스럽지 않게, '나 지금부터 한국어로 얘기할거야!' 라고 먼저 말해줘야 하는거죠.

두 번째로, **한쪽이 말할 때 다른 쪽은 들을 수 있어야** 해요. 둘 다 말하거나 조용히 있으면 대화가 이어질 수 없겠죠? 또, 전화 연결이 끊어지면 더 이상 대화를 이어갈 수 없을 겁니다.

이처럼 웹 브라우저와 웹 서버가 우리 앞으로 통신할 때 이렇게 하자~ 하고 정해놓은 규약이 바로 **HTTP** 입니다.

클라이언트(웹 브라우저)에서 통신을 시작할 때 '나 지금부터 HTTP로 보낼거야! 이런 이런거 보내줘' 라고 말하면, 서버에서는 전에 정해놓은 약속대로 '아 ㅇㅋㅇㅋ 알아들었어 그럼 나 이런이런 응답줄게' 라고 대답하도록요!

여러분이 인터넷을 통해 WWW(World Wide Web) 서비스를 이용한다면, HTTP를 지키면서 사용해야 합니다.

## HTTP의 작동 방식

### 서버/클라이언트 모델

-   클라이언트가 먼저 서버에게 요청을 보내고, 요청을 받은 서버는 클라이언트에 응답을 보냅니다. 
-   여기서 클라이언트는 사용자를 뜻합니다. 사용자가 웹 브라우저를 이용해 서버에 어떤 요청을 보내면, 웹 서버에서 이를    처리해서 클라이언트에 응답을 보내도록 설계되어 있습니다.

### 무상태 프로토콜

-   서버가 요청에 대한 응답이 끝나면 바로 클라이언트와의 연결을 종료해요. 불특정 다수를 대상으로 하는 서비스에는 클라이언트와 서버가 계속 연결된 형태가 아니기 때문에 클라이언트와 서버간의 최대 연결 수보다 훨씬 많은 요청과 응답을 처리할 수 있어 적합합니다.
-   그러나 연결을 끊어버리면 클라이언트의 이전 상황을 알 수 없습니다. 쇼핑 카트에 물건을 담아서 계산하러 갔는데, 하나 계산하고 종료, 하나 계산하고 종료해버리면 사용자가 뭘 샀는지, 총 얼마를 계산해야 하는지 알 수 없겠죠?
-   이런 문제를 해결하기 위해 Cookie와 같은 기술이 등장했습니다.

![](https://velog.velcdn.com/images/dayo____me/post/1d65e0e3-2122-46a6-9357-80843ec05a9f/image.png)

그럼, HTTP로 요청을 보낼 때 클라이언트는 어떤 형식을 지키는지 더 자세히 살펴봅시다.

### 요청 데이터 포맷 Request Data Format

```
GET /images/logo.gif HTTP/1.1
Host: www.just-developer.tistory.com
Accept-Language: en
```

-   헤더, 빈줄, 바디 세 부분으로 나뉩니다.
-   헤더
    -   **GET** : 요청 메서드, 서버에게 요청의 종류를 알려줌
    -   **/images/logo.gif** : 요청 URI, 요청하는 자원의 위치를 명시
    -   **HTTP/1.1** : 웹 브라우저가 사용하는 프로토콜의 버전 명시
    -   각각의 줄은 헤더명과 헤더값이 콜론으로 구분되고 각 줄은 캐리지 리턴(Carriage Return)과 라인 피드(Line Feed)로 구분
-   빈줄(empty line)
    -   빈 줄은 <CR><LF>로 구성되며 그 외 다른 화이트스페이스(whitespace)가 있어서는 안 됩니다.
-   바디
    -   GET 방식은 요청할 때 가지고 가야하는 자원 부분도 URI에 붙여서 가지고 가기 때문에 요청 바디가 존재하지 않습니다.
    -   요청 메서드가 POST, PUT을 사용하게 되었을 때 body가 들어오게 됩니다. 아래와 같은 형태로 표시될 수 있습니다.

```
field1=value1&field2=value2
```

#### 요청 메서드 종류

| 메서드명 | 설명 |
| --- | --- |
| GET | 정보를 요청하기 위해서 사용(SELECT) |
| POST | 정보를 밀어넣기 위해서 사용(INSERT) |
| PUT | 정보를 업데이트하기 위해서 사용(UPDATE) |
| DELETE | 정보를 삭제하기 위해서 사용(DELETE) |
| HEAD | (HTTP)헤더 정보만 요청 해당 자원이 존재하는지, 혹은 서버에 문제가 없는지를 확인하기 위해서 사용 |
| OPTIONS | 웹서버가 지원하는 메서드의 종류를 요청 |
| TRACE | 클라이언트의 요청을 그대로 반환 예컨데 echo 서비스로 서버 상태를 확인하기 위한 목적으로 주로 사용 |

<br><br>
### 응답 데이터 포맷 Response Data Format
```
Date: Mon, 23 May 2005 22:38:34 GMT
Content-Type: text/html; charset=UTF-8
Content-Encoding: UTF-8
Content-Length: 138
Last-Modified: Wed, 08 Jan 2003 23:11:55 GMT
Server: Apache/1.3.3.7 (Unix) (Red-Hat/Linux)
ETag: "3f80f-1b6-3e1cb03b"
Accept-Ranges: bytes
Connection: close
```

-   헤더  
    -   첫 줄은 반드시 응답 HTTP 프로토콜의 버전, 응답 코드, 응답 메시지로 이루어집니다.
    -   ex) HTTP/1.1 200 OK
    -   날짜, 웹서버 이름과 버전, 컨텐츠 타입, 캐시 제어 방식, 컨텐츠 길이 등으로 구성됩니다.
-   바디
    -   실제 응답 리소스 데이터입니다.
    -   이 HTML 소스를 응답으로 줌으로써, 우리는 요청에 해당하는 웹 페이지를 볼 수 있게 됩니다.

```
<html>
<head>
  <title>An Example Page</title>
</head>
<body>
  Hello World, this is a very simple HTML document.
</body>
</html>
```

#### 응답 코드 종류

응답 코드는 종류가 굉장히 많지만, 우리가 상대적으로 자주 접할 수 있는 코드들만 모아보았습니다.

| 코드 | 메시지 | 설명 |
| --- | --- | --- |
| 200 | OK | 오류 없이 전송 성공 |
| 400 | Bad Request | 요청 실패 |
| 404 | Not Found | 문서를 찾을 수 없음 |
| 500 | Internal Server Error | 서버 내부 오류 |
| 502 | Bad gateway | 게이트웨이 상태 나쁨 |
<br><br>
  
### URL (Uniform Resource Locator)

우리가 웹 사이트에 접속하고, 사이트에 있는 특정 버튼이나 링크를 누르면 브라우저 상단의 주소창에 있는 문자가 변하는 걸 볼 수 있습니다. 이걸 **URL**이라고 하는데요, 이는 인터넷 상의 자원의 위치를 의미합니다.

![](https://velog.velcdn.com/images/dayo____me/post/b05a0f3d-9433-41d9-9871-2814f7b289b1/image.png)

URL의 형식을 그림으로 표현하면 위와 같습니다. (출처: https://en.wikipedia.org/wiki/URL)

1\. **scheme(protocol)**
-   접근 프로토콜을 의미합니다. 리소스에 어떻게 접근할 것인지 명시해줍니다.
-   ftp, http, https, mailto 등이 들어갈 수 있습니다.
2\. **userinfo**
-   사용자의 이름과 비밀번호를 요구하는 서버의 경우, userinfo@host와 같은 형식으로 접근할 수 있습니다.
3\. **host, port**
-   host는 해당 서버가 존재하는 IP 주소, 혹은 도메인 이름을 의미합니다.
-   하나의 물리적 컴퓨터에는 여러 개의 소프트웨어 서버가 동작할 수 있습니다. 따라서 이 소프트웨어 서버를 구분하기 위해 포트값을 사용합니다. 이 포트값은 0보다 큰 숫자를 사용합니다.
4\. **path**
-   path는 문서의 경로를 의미합니다.
5. **query**
-   query는 자원을 GET 방식으로 요청할 때 필요한 데이터를 함께 넘겨 주기 위해 사용합니다. 예를 들어, /images/news/ 라는 폴더에서 타입이 'tistory'인 이미지들만 가져오고 싶다면, ?type=tistory 와 같이 보내주고 이를 서버에서 처리해 해당하는 자료만 가져오도록 설계할 수 있습니다.
6. **fragment**
-   HTML에서는 각각의 요소에 id를 부여할 수 있습니다. #fragment에 사용자의 스크롤을 배치할 요소의 id를 지정하면, 사용자가 해당 URL을 요청했을 때 그 위치에 스크롤을 배치할 수 있습니다.
예를 하나 들어보겠습니다. 제가 지금 포스팅 하고 있는 URL 위치입니다.
```
https://velog.io/write?id=851c0a73-ab93-47f7-ac7f-3431151917ed
```

저는 지금 https 프로토콜을 사용해서 접근하고 있고, velog.io 라는 도메인에서 /write 라는 경로에서 id가 851c0a73-ab93-47f7-ac7f-3431151917ed인 페이지를 불러오고 있네요.

보통 위 그림과 같은 경로를 모두 사용하는 경우는 드뭅니다.

저는 protocol://host/path 혹은 protocol://host/path/query 구조로 된 URL를 더 자주 접할 수 있던 것 같습니다.

## 참고 자료 및 출처


[웹 풀스택 - 부스트코스](https://www.boostcourse.org/web316/lecture/16661?isDesc=false)

[https://ko.wikipedia.org/wiki/HTTP](https://ko.wikipedia.org/wiki/HTTP)
  
[HTTP - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/HTTP)

  [https://developer.mozilla.org/ko/docs/Web/HTTP/Overview](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)
 
  [HTTP 개요 - HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)
  
 [URL - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/URL)