# HTTP 헤더1 - 일반 헤더
---
+ HTTP 헤더 개요
+ 표현
+ 콘텐츠 협상
+ 전송 방식
+ 일반 정보
+ 특별한 정보
+ 인증
+ 쿠키
---
## HTTP 헤더 개요 📄

+ header-field = field-name ":" OWS field-value OWS (OWS:띄어쓰기 허용)
+ field-name은 대소문자 구문 없음

#### HTTP 헤더 용도
+ HTTP 전송에 필요한 모든 부가정보
+ 표준 헤더가 너무 많음
+ 필요시 임의의 헤더 추가 가능

![[스크린샷 2024-05-10 105144.png]](https://github.com/SpringFoward/http-fundamentals/blob/0a6929c97102550fe9395a0a412fb6206d6a36cb/bhcho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/2%EC%A3%BC%EC%B0%A8/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-10%20105144.png)

이전에는 RFC2616을 표준으로 사용하였지만, 2014년 부터 RFC7230 ~ 7235를 사용한다.

#### RFC723x 변화
+ 엔티티(Entity)가 사라지고 표현(Representation)이 생김
+ 표현 = 표현 메타데이터 + 표현 데이터

---

## 표현😀
+ Content-Type: 표현 데이터의 형식
+ Content-Encoding: 표현 데이터의 압축 방식
+ Content-Language: 표현 데이터의 자연 언어
+ Content-Length: 표현 데이터의 길이
+ 표현 헤더는 전송, 응답 둘다 사용

![[스크린샷 2024-05-10 105600.png]](https://github.com/SpringFoward/http-fundamentals/blob/0a6929c97102550fe9395a0a412fb6206d6a36cb/bhcho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/2%EC%A3%BC%EC%B0%A8/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-10%20105600.png)

### Content-Type
**표현 데이터의 형식 설명

+ 미디어 타입, 문자 인코딩
+ ex)
 + text/html; charset=utf-8
 + application/json
 + image/png

### Content-Language
**표현 데이터의 자연 언어

+ 표현 데이터의 자연 언어를 표현
+ ex)
 + ko
 + en
 + en-US

### Content-Length
**표현 데이터의 길이

+ 바이트 단위
+ Transfer-Encoding(전송 코딩)을 사용하면 Content-Length를 사용하면 안됨

___

## 협상 💰
**클라이언트가 선호하는 표현 요청

+ Accept: 클라이언트가 선호하는 미디어 타입 전달
+ Accept-Charset: 클라이언트가 선호하는 문자 인코딩
+ Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
+ Accept-Language: 클라이언트가 선호하는 자연 언어


+ **협상 헤더는 요청시에만 사용함

#### Accept-Language

이를 사용하면 내가 원하는 언어 형식으로 언어를 받을 수 있다.(물론 내가 원하는 언어가 서버에 없는 경우에는 사용 불가)

#### 협상과 우선순위1
**Quality Values(q)

+ 0~1, 클수록 높은 우선순위
+ 생략하면 1
+ Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
 + ko-KR;q=1 (q생략)
 + ko;q=0.9
 + en-US;q=0.8
 + en;q=0.7
+ 이렇듯 언어 형식을 여러개 요청할 수 있음

#### 협상과 우선순위2
**Quality Values(q)

+ 구체적인 것이 우선한다.
+ Accept: text/*, text/plain, text/plain;format=flowed, */*
 + text/plain;format=flowed
 + text/plain
 + text/*
 + */*
 + 복잡한 순으로 우선순위가 높음

---
## 전송방식 📋

+ Transfer-Encoding
+ Range, Content-Range

#### 전송 방식 설명

+ 단순 전송
+ 압축 전송
+ 분할 전송
+ 범위 전송

##### 단순전송
특별한 전송 문자 없이 보내는 응답 전송

##### 압축 전송

+ Content-Encoding : gzip
+ 본문을 압축해서 보내는 방식

##### 분할 전송
+ Transfer-Encoding : chunked
+ 본문을 여러개로 나누어 보내주는 방식

##### 범위 전송
+ Content-Range : bytes (숫자 ~ 숫자 / 숫자)
+ 클라이언트가 요청한 본문의 범위 만큼을 보내주는 방식

---
## 일반 전송 ✉️

+ From: 유저 에이전트의 이메일 정보
+ Referer: 이전 웹 페이지 주소
+ User-Agent: 유저 에이전트 애플리케이션 정보
+ Server: 요청을 처리하는 오리진 서버의 소프트웨어 정보
+ Date: 메시지가 생성된 날짜

#### Form
**유저 에이전트의 이메일 정보

+ 일반적으로 잘 사용되지 않음
+ 검색 엔진 같은 곳에서, 주로 사용
+ 요청에서 사용

#### Referer
**이전 웹 페이지 주소

+ 현재 요청된 페이지의 이전 웹 페이지 주소
+ A -> B로 이동하는 경우 B를 요청할 때 Referer: A 를 포함해서 요청
+ Referer를 사용해서 유입 경로 분석 가능
+ 요청에서 사용
+ 참고: referer는 단어 referrer의 오타 (만들고 나중에 알아서 고치기 애매해짐)

#### User-Agent
**유저 에이전트 애플리케이션 정보

+ user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/ 537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
+ 클라이언트의 애플리케이션 정보(웹 브라우저 정보, 등등)
+ 어떤 종류의 브라우저에서 장애가 발생하는지 파악 가능
+ 요청에서 사용

#### Server
**요청을 처리하는 ORIGIN 서버의 소프트웨어 정보

+ Server: Apache/2.2.22 (Debian)
+ server: nginx
+ 응답에서 사용

#### Date
**메시지가 발생한 날짜와 시간

+ Date: Tue, 15 Nov 1994 08:12:31 GMT
+ 응답에서 사용

---
## 특별한 정보 📧

+ Host: 요청한 호스트 정보(도메인)
+ Location: 페이지 리다이렉션
+ Allow: 허용 가능한 HTTP 메서드
+ Retry-After: 유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

#### Host
**요청한 호스트 정보(도메인)

+ 요청에서 사용
+ 하나의 서버가 여러 도메인을 처리해야 할 때
+ 하나의 IP 주소에 여러 도메인이 적용되어 있을 때

#### Location
**페이지 리다이렉션

+ 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동 (리다이렉트)
+ 3xx (Redirection): Location 값은 요청을 자동으로 리디렉션하기 위한 대상 리소스를 가리킴

#### Allow
**허용 가능한 HTTP 메서드

+ 405 (Method Not Allowed) 에서 응답에 포함해야함
+ Allow: GET, HEAD, PUT

#### Retry-After
**유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간

+ 503 (Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있음
+ Retry-After: Fri, 31 Dec 1999 23:59:59 GMT (날짜 표기)
+ Retry-After: 120 (초단위 표기)

---
## 인증 🔓

+ Authorization: 클라이언트 인증 정보를 서버에 전달
+ WWW-Authenticate: 리소스 접근시 필요한 인증 방법 정의

#### Authorization
**클라이언트 인증 정보를 서버에 전달

+ Authorization: Basic xxxxxxxxxxxxxxxx

#### WWW-Authenticate
**리소스 접근시 필요한 인증 방법 정의

+ 401 Unauthorized 응답과 함께 사용
+ WWW-Authenticate: Newauth realm="apps", type=1, title="Login to \"apps\"", Basic realm="simple"

---
## 쿠키 🪧

+ Set-Cookie: 서버에서 클라이언트로 쿠키 전달(응답)
+ Cookie: 클라이언트가 서버에서 받은 쿠키를 저장하고, HTTP 요청시 서버로 전달
+ 쿠키를 사용하지 않으면 클라이언트가 로그인 하였을 때 그 로그인 정보를 모든 요청에 포함하여 전달해야 한다.

![[스크린샷 2024-05-10 114953.png]](https://github.com/SpringFoward/http-fundamentals/blob/94fa695226a773ac8c27797d3956b5da26fc2820/bhcho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/2%EC%A3%BC%EC%B0%A8/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-10%20114953.png)

이러한 문제는 쿠키를 사용하면 해결된다.
+ 예) set-cookie: **sessionId=abcde1234**; **expires**=Sat, 26-Dec-2020 00:00:00 GMT; **path=/; domain**=.google.com; **Secure
+ 사용처
 + 사용자 로그인 세션 관리
 + 광고 정보 트래킹
+ 쿠키 정보는 항상 서버에 전송됨
 + 네트워크 트래픽 추가 유발
 + 최소한의 정보만 사용(세션 id, 인증 토큰)
 + 서버에 전송하지 않고, 웹 브라우저 내부에 데이터를 저장하고 싶으면 웹 스토리지 (localStorage, sessionStorage) 참고
+ **주의! 사용자의 민감한 정보(카드 번호, 주민 번호 등)는 저장하면 안됨

#### 쿠키 - 생명주기
**Expires, max-age

+ Set-Cookie: expires=Sat, 26-Dec-2020 04:39:21 GMT
 + 만료일이 되면 쿠키 삭제(보안과 용량을 위해서)
+ Set-Cookie: max-age=3600 (3600초)
 + 0이나 음수를 지정하면 쿠키 삭제
+ 세션 쿠키: 만료 날짜를 생략하면 브라우저 종료시 까지만 유지
+ 영속 쿠키: 만료 날짜를 입력하면 해당 날짜까지 유지

#### 쿠키 - 경로
**Path

+ 예) path=/home
+ 이 경로를 포함한 하위 경로 페이지만 쿠키 접근
+ 일반적으로 path=/ 루트로 지정

#### 쿠키 - 보안
**Secure, HttpOnly, SameSite

+ Secure
 + 쿠키는 http, https를 구분하지 않고 전송하지만, Secure는 https에만 전송
+ HttpOnly
 + XSS 공격 방지
 + 자바스크립트에서 접근 불가(document.cookie)
 + HTTP 전송에만 사용
+ SameSite
 + XSRF 공격 방지
 + 요청 도메인과 쿠키에 설정된 도메인이 같은 경우만 쿠키 전송
 ---
 
