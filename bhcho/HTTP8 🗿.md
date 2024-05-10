# HTTP  헤더2 - 캐시와 조건부 요청
---
+ 캐시 기본 동작
+ 검증 헤더와 조건부 요청
+ 캐시와 조건부 요청 헤더
+ 프록시 캐시
+ 캐시 무효화
---
## 캐시 기본 동작 📁

### 캐시가 없을 경우

![[스크린샷 2024-05-10 134001.png]](https://github.com/SpringFoward/http-fundamentals/blob/94fa695226a773ac8c27797d3956b5da26fc2820/bhcho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/2%EC%A3%BC%EC%B0%A8/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-10%20134001.png)

캐시가 없을 경우에는 내가 요청했던 게 다시 필요할 때마다 계속 서버에 요청을 해서 받아와야 한다.

**특징 (캐시가 없을 때)
+ 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
+ 인터넷 네트워크는 매우 느리고 비싸다.
+ 브라우저 로딩 속도가 느리다.
+ 느린 사용자 경험

### 캐시가 있을 때
![[스크린샷 2024-05-10 134442.png]](https://github.com/SpringFoward/http-fundamentals/blob/94fa695226a773ac8c27797d3956b5da26fc2820/bhcho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/2%EC%A3%BC%EC%B0%A8/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-10%20134442.png)

캐시가 있을 경우에는 한번 요청한 데이터를 캐시에 보관해 놓으면서 내가 다시 똑같은 데이터가 필요할 때 캐시에서 바로 꺼내올 수 있다.

**특징(캐시가 있는 경우)
+ 캐시 덕분에 캐시 가능 시간동안 네트워크를 사용하지 않아도 된다.
+ 비싼 네트워크 사용량을 줄일 수 있다.
+ 브라우저 로딩 속도가 매우 빠르다.
+ 빠른 사용자 경험
**캐시의 시간이 초과된 경우**
캐시의 시간이 초과된 경우에는 서버를 통해 데이터를 다시 조회하고, 캐시를 갱신한다. 이 때 다시 네트워크 다운로드가 발생한다.(캐시가 없는 경우보다는 훨씬 덜하다.) 
이를 해결하기 위해 필요한 것이 아래에 있는 검증 헤더와 조건부 요청으로 해결한다.

---
## 검증 헤더와 조건부 요청 🔐

### 캐시 시간 초과

+ 캐시 유효 시간이 초과해서 서버에 다시 요청하면 다음 두 가지 상황이 나타난다.
 + 서버에서 기존 데이터를 변경함 
 + 서버에서 기존 데이터를 변경하지 않음 (이 경우 최적화 가능)

#### 서버에서 기존 데이터를 변경하지 않은 경우

+ 생각해보면 데이터를 전송하는 대신에 저장해 두었던 캐시를 재사용 할 수 있다.
+ 클라이언트의 데이터와 서버의 데이터가 같다는 사실을 확인할 수 있는 방법 필요하다.

![[스크린샷 2024-05-10 140212.png]](https://github.com/SpringFoward/http-fundamentals/blob/94fa695226a773ac8c27797d3956b5da26fc2820/bhcho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/2%EC%A3%BC%EC%B0%A8/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-10%20140212.png)

이렇듯 캐시가 만료되어도 서버에서 데이터의 바디만 전달하고 이것이 캐시 데이터의 수정일과 같다면 그냥 그 캐시를 이용할 수 있다.

**특징 (검증 헤더와 조건부 요청)

+ 캐시 유효 시간이 초과해도, 서버의 데이터가 갱신되지 않으면 304 Not Modified + 헤더 메타 정보만 응답
+ 클라이언트는 서버가 보낸 응답 헤더 정보로 캐시의 메타 정보를 갱신
+ 클라이언트는 캐시에 저장되어 있는 데이터 재활용
+ 네트워크 다운로드가 발생하지만 용량이 적은 헤더 정보만 다운로드 (매우 실용적)

#### 검증헤더
**Last-Modified , ETag

#### 조건부 요청 헤더 (Last-Modified)
+ If-Modified-Since: Last-Modified 사용
+ 조건이 만족하면 200 OK
+ 조건이 만족하지 않으면 304 Not Modified

+ **예시 (Last-Modified)
 + 캐시시간 = 서버시간 : 304 Not Modified, 헤더 데이터만 전송(BODY 미포함)
 + 캐시시간 != 서버시간 : 200 OK, 모든 데이터 전송(BODY 포함)

+ **단점 (Last-Modified)
 + 1초 미만(0.x초) 단위로 캐시 조정이 불가능
 + 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우 불필요한 네트워크 손실 발생
 + 서버에서 별도의 캐시 로직을 관리하고 싶은 경우

#### 조건부 요청 헤더 (ETag)

+ 캐시용 데이터에 임의의 고유한 버전 이름을 달아둠
+ 데이터가 변경되면 이 이름을 바꾸어서 변경함(Hash를 다시 생성)
 + ex) ETag: "aaaaa" -> ETag: "bbbbb"
+ 단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받기!
+ Last-Modified의 단점 보완

+ **예시 (ETag)
 + 캐시ETag = 서버ETag : 304 Not Modified, 헤더 데이터만 전송(BODY 미포함)
 + 캐시ETag != 서버ETag : 200 OK, 모든 데이터 전송(BODY 포함)

---
## 캐시와 조건부 요청 헤더 🔑

#### 캐시 제어 헤더

+ Cache-Control: 캐시 제어
+ Pragma: 캐시 제어(하위 호환 - 잘 안씀)
+ Expires: 캐시 유효 기간(하위 호환 - 잘 안씀)

#### Cache-Control: 캐시 제어

+ Cache-Control: max-age
 + 캐시 유효 시간을 초 단위로 지정
+ Cache-Control: no-cache
 + 데이터는 캐시해도 되지만, 항상 서버에 검증하고 사용
+ Cache-Control: no-store
 + 데이터에 민감한 정보가 있으므로 저장하면 안됨 (주민번호, 카드번호 등)

#### 검증헤더와 조건부 요청 헤더

+ 검증 헤더 (Validator)
 + ETag: "v1.0", ETag: "asid93jkrh2l"
 + Last-Modified: Thu, 04 Jun 2020 07:19:24 GMT
+ 조건부 요청 헤더
 + If-Match, If-None-Match: ETag 값 사용 (None은 일반 Match와 반대 개념)
 + If-Modified-Since, If-Unmodified-Since: Last-Modified 값 사용(Unmodified는 Modified와 반대 개념)
---
## 프록시 캐시 🖥️

우리가 한국에서 미국에 있는 서버에 접속할려면 어느정도의 시간이 걸릴 것이다. 이것을 해결하기 위해 존재하는 거시 프록시 캐시 서버이다.
프록시 캐시 서버는 미국에 있는 원서버(origin)에 접근하는 것이 아니라 한국에 프록시 캐시 서버를 만들어서 접근을 좀 더 용이하게 하는 것이다.

![[스크린샷 2024-05-10 201357.png]](https://github.com/SpringFoward/http-fundamentals/blob/94fa695226a773ac8c27797d3956b5da26fc2820/bhcho/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%20%EB%B3%B4%EA%B4%80/2%EC%A3%BC%EC%B0%A8/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202024-05-10%20201357.png)

#### Cache-Control

+ Cache-Control: public
 + 응답이 public 캐시에 저장되어도 됨
+ Cache-Control: private
 + 응답이 해당 사용자만을 위한 것임, private 캐시에 저장해야 함(나의 아이디나 비밀번호가 이에 해당)
+ Cache-Control: s-maxage
 + 프록시 캐시에만 적용되는 max-age
+ Age: 60 (HTTP 헤더)
 + 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)
---
## 캐시 무효화 ❌
**확실한 캐시 무효화 응답

+ Cache-Control: no-cache, no-store, must-revalidate
+ Pragma: no-cache
 + HTTP 1.0 하위 호환
#### Cache-Control
**캐시 지시어(directives)

+ Cache-Control: no-cache
 + 데이터는 캐시해도 되지만, 항상 원 서버에 검증하고 사용(이름에 주의!)
+ Cache-Control: no-store
 + 데이터에 민감한 정보가 있으므로 저장하면 안됨
+ Cache-Control: must-revalidate
 + 캐시 만료후 최초 조회시 원 서버에 검증해야함
 + 원 서버 접근 실패시 반드시 오류가 발생해야함 - 504(Gateway Timeout)
 + must-revalidate는 캐시 유효 시간이라면 캐시를 사용함
 + Cache-Control: no-cache와 비슷하지만 더 엄격한 캐시 지시어
---

