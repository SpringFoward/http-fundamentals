# URI와 웹 브라우저 요청 흐름 🕫
---
+ URI
+ 웹 브라우저 요청 흐름
---
## URI (Uniform Resource Identifier)  🌐
URI는 자원이 어디에 있는지 식별하는 방법이다. URL방법의 종류로는 2가지가 존재하는데 첫번째는 __URL(Uniform Resource Locator)__ 이고 두번째는 __URN(Uniform Resource Name)__  이다.

URL의 경우 자원의 위치를 특정해서 찾는 방법이고, URN은 이름 그 자체를 의미한다.
![[스크린샷 2024-05-06 140635.png]](https://github.com/SpringFoward/http-fundamentals/blob/5c7b59f52135791d5f53d9a0339e3d35aff63c0a/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-06%20140635.png)
보통은 URL을 주로 많이 사용하고 URL,URN 전부 URI이다.

### URI 단어의 뜻 
---
+ Uniform : 리소스를 식별하는 통일된 방식
+ Resource : 자원, URI로 식별가능한 모든 것(제한 없음)
+ Idenrifier: 다른 항목과 구분하는데 필요한 정보

+ URL : Uniform Resource Locator
+ URN : Uniform Resource Name
---
### URL, URN 단어의 뜻
---
+ URL - Locator : 리소스가 있는 위치를 지정 ex) :https://www.google.com/search?q=hello&hl=ko
+ URN - Name : 리소스에 이름을 부여
+ 위치는 변할 수 있지만, 이름은 변하지 않는다.
+ URN 이름만으로 실제 리소스를  찾을 수 있는 방법은 보편화 되지 않음
---
URL의 예시를 보고 전체 문법이 어떻게 사용되는지 알아보겠다.
https://www.google.com:443/search?q=hello&hl=ko

+ 프로토콜(https) : 어떤 방식을 자원에 접근할 것인가 하는 약속 규칙
+ 호스트명(www.google.com) :  도메인 명 또는 IP주소를 직접 사용가
+ 포트 번호(443) : 포트 번호는 생략 할 수 있음 프로토콜에 따라 자동으로 입력
+ 패스(/search) : 리소스 경로, 계층적 구조
+ 쿼리 파라미터(q=hello&hl=ko)  : key=value 형태, ?로 시작, &로 추가가능

## 웹브라우저 요청 흐름 🔄
웹 브라우저와 서버를 연결할 때 웹 브라우저가 DNS 서버를 조회하고 HTTP 요청 메세지를 생성한다. HTTP 요청 메세지는 이렇게 생겼다.![[스크린샷 2024-05-06 143132.png]]

이 메세지를 생성한 이후 SOCKET라이브러리를 통해서 전달하는데, TCP,IP로 연결한 뒤에 데이터를 전달한다. 이 때  IP패킷에 있는 전송 데이터가 위에 있는 HTTP 요청메세지 이다. 이를 받은 서버는 HTTP 응답 메세지를 만드는데 응답 메세지는 아래와 같이 생겼다.![[스크린샷 2024-05-06 143614.png]]

이후 서버도 역시 TCP,IP 패킷에 이 응답메세지를 전송데이터로 올려놓고 웹 브라우저에 다시 전달한다. 그러면 웹 브라우저가 응답메세지를 보고 HTML 렌더링을 하면서 화면을 보여주는 형식으로 사용이 된다.
.
