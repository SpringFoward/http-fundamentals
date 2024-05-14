
# HTTP 메서드 활용📜
---
+ 클라이언트에서 서버로 데이터 전송
+ HTTP API 설계 예시
---
## 클라이언트에서 서버로 데이터 전송📨
__데이터의 전달 방식은 크게 2가지로 나눌 수 있다.__

+ 쿼리 파라미터를 통한 데이터 전송
 + GET
 + 주로 정렬 필터(검색) 등
 
+ 메세지 바디를 통한 데이터 전송
 + POST,PUT,PATCH
 + 회원가입, 상품주문, 리소스 등록, 리소스 변경 등
 
데이터 전달 방식으로 나눈다면 2가지로 나눌 수 있지만 **4가지의 상황**으로도 나눌 수 있다.

### 정적 데이터 조회 (쿼리 파라미터 미사용)
 + 이미지, 정적 텍스트 문서
![[스크린샷 2024-05-07 173022.png]]
클라이언트에서 서버로 별이라는 이미지를 전달할 때는 서버에 URI만 넣어주면 된다. 이런 정적 데이터는 GET을 사용하고 일반적으로 쿼리 파라미터 없이 리소스 경로로 단순하게 조회가 가능하다.

### 동적 데이터 조회 (쿼리 파라미터 사용)
 + 검색,게시판 목록 정렬 필터(검색어)
![[스크린샷 2024-05-07 200004.png]]
동적 데이터를 조회할 때는 검색어와 같은 추가 데이터를 전달해야 하는데 이럴 때는 쿼리 파라미터를 이용하여 데이터를 전달하고 서버가 그 데이터를 응답해준다. 주로 검색을하거나 게시판 목록에서 정렬할 때 사용하며, 조회 조건을 줄여주는 필터나 조회결과를 정렬하는 정렬 조건에 주로 사용한다. 
동적 데이터는 정적 데이터 조회와 똑같이 GET을 사용하고, 다른점은 쿼리 파라미터를 사용하여 데이터를 추가로 전달한다는 점이 다르다.

### HTML Form을 통한 데이터 전송
__POST 전송 - 저장__
![[스크린샷 2024-05-07 200527.png]]
우리가 서버를 만들고 싶을 때 HTML Form을 사용할 것인데 HTML Form은 위와 같다. 클라이언트가 이러한 Form을 만들고 서버에 전송을 하면 서버는 이를 해석하여 클라이언트가 요청한 응답을 보내주는 형식이다. POST말고 GET을 사용할 수도 있지만, **데이터 수정**이 일어나선 안된다.

__multipart/form-data__
![[스크린샷 2024-05-07 201516.png]]
이 HTML Form 형식을 보면 POST자리에 multipart/form-data가 들어가있다. 이는  boundary를 이용하여 각각의 입력마다 잘라서 넣어준다. (멀티파트) 주로 binary데이터를 전송할 때 사용한다.

### HTML Form 정리

+ HTML Form submit시 POST 전송
 + ex) 회원가입, 상품 주문, 데이터 변경
+ Content-Type: application/x-www-form-urlencorded사용
 + form의 내용을 메세지 바디를 통해 전송(key=value 형식, 쿼리 파라미터 형식)
 + 전송 데이터를 url encoding 처리
 + ex) abc김 -> abc%EA%B9%80
+ HTML Form은 GET 전송도 가능 (하지만 save는 못함!! 조회만 가능)
+ Content-Type: multipart/form-data
 + 파일 업로드 같은 바이너리 데이터 전송시 사용
 + 다른 종류의 여러 파일과 폼의 내용 함께 전송 가능(multipart)


### HTTP API를 통한 데이터 전송
 ![[스크린샷 2024-05-07 203449.png]]
 이는 안드로이드나 아이폰 애플리케이션에서 바로 서버로 데이터를 전송할 때 사용한다. 위와 같이 내가 다 만들고 전송하면 된다.


### HTTP API 정리
 + 서버와 서버가 서로 통신할 때 주로 사용함(서버 to 서버)
  + 백엔드 시스템 통신
+ 앱 클라이언트에서도 사용
 + 아이폰, 안드로이드
+ 웹 클라이언트
 + HTML에서 Form 전송 대신 자바 스크립트를 통한 통신에 사용한다.(AJAX)
+ POST,PUT,PATCH : 메세지 바디를 통해서 데이터를 전송한다.
+ GET : 조회 , 쿼리 파라미터로 데이터를 전달한다.
+ Content-Type : application/json을 주로 사용한다. (사실상 표준)
 + TEXT,XML,JSON 등
 ---
## HTTP API 설계 예시🏷️

### 회원 관리 시스템
+ **회원** 목록 /members -> GET
+ **회원** 등록 /members -> POST
+ **회원** 조회 /members/{id} -> GET
+ **회원** 수정 /members/{id} -> PATCH,PUT,POST
+ **회원** 삭제 /members/{id} -> DELETE

### POST - 신규 자원 등록 특징 
+  클라이언트는 등록될 리소스의 URI를 모른다. 
 +  회원 등록 /members -> POST  
 + POST **/members** (서버가 만들어줌)
+  서버가 새로 등록된 리소스 URI를 생성해준다. 
 + HTTP/1.1 201 Created 
 + Location: /members/100 
+ 컬렉션(Collection) 
 +  서버가 관리하는 리소스 디렉토리 
 +  서버가 리소스의 URI를 생성하고 관리 
 +  여기서 컬렉션은 /members

### 파일관리 시스템
**PUT기반 등록

+ **파일** 목록 /files -> GET
+ **파일** 조회 /files/{filename} -> GET
+ **파일** 등록 /files/{filename} -> PUT
+ **파일** 삭제 /files/{filename} -> DELETE
+ **파일** 대량 등록 /files -> POST


### PUT - 신규 자원 등록 특징

+ 클라이언트가 리소스 URI를 알고 있어야 한다.
 + 파일 등록 /files/{filename} -> PUT 
 + PUT **/files/star.jpg**  (다른점이 존재함 : 클라이언트가 직접 지정)
+  클라이언트가 직접 리소스의 URI를 지정한다. 
+ 스토어(Store) 
 + 클라이언트가 관리하는 리소스 저장소 
 + 클라이언트가 리소스의 URI를 알고 관리 
 + 여기서 스토어는 /files

이렇게 두 POST와 PUT으로 나뉘어 지지만, 대부분 POST기반의 신규 자원등록을 사용한다


### HTML FORM 사용

+ HTML FORM은 **GET,POST만 지원**
+ AJAX 같은 기술을 사용해서 해결 가능 -> 회원 API 참고
+ 여기서는 순수 HTML, HTML FORM을 이야기함
+ GET과 POST만 지원하기 때문에 제약이 존재

### 회원 관리 시스템
+ **회원** 목록 /members -> GET
+ **회원** 등록 폼 /members/new -> GET
+ **회원** 등록 /members/new(권장), /members (둘중 하나 사용 가능) -> POST
+ **회원** 조회 /members/{id} -> GET
+ **회원** 수정 폼 /members/{id}/edit -> GET
+ **회원** 수정 /members/{id}/edit(권장), members/{id} (둘중 하나 사용 가능) -> POST
+ **회원** 삭제 /members/{id}/delete -> POST

이런 식으로 HTML FORM은 GET ,POST 만 지원하여 불편함이 존재한다. 그래서 **컨트롤 URI**(/new, /edit, /delete)를 사용하여 PATCH,PUT,DELETE를 대신하여 사용한다. 

##### https://restfulapi.net/resource-naming  참고

---
