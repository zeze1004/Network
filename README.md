# HTTP 



### IP (인터넷 프로토콜)

- IP 주소에 데이터 전달
  - 패킷 단위로 전탈
    - 출발 IP, 도착 IP 등 전달
- IP 프로토콜의 한계
  - 비연결성
    - 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
  - 비신뢰성
    - 패킷이 사라질 수 있음
    - 패킷이 순서대로 안올 수 있음
  - 프로그램 구분
    - 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상일 수 있음



### TCP(전송 제어 프로토콜)

- IP 문제점을 해결하기 위한 IP 계층 위의 프로토콜

- ### 인터넷 프로토콜 4계층

  ​	사진

- ### 프로토콜 계층

  사진

- IP 프로토콜 계층에서 출발지 IP, 목적지 IP, 메세지 등을 담은 패킷이

  TCP 계층에서 전송 제어, 순서, 검증 정보 등도 함께 담기게 됨

  + 전송 데이터도 이 때 담김
  + PORT 정보도 담김

- ### TCP 특징

  - 1. 연결지향 - TCP 3 WAY 핸드ㅅㅔ이크(가상 연결)

       - 클라이언트가 서버에게 1) 씽크(syn)
       - 서버가 받았다고 클라이언트에게 2) syn + ack
       - 마지막으로 클라이언트가 서버에게 3) ack
       - 그 후 데이터 전송

    2. 데이터 전달 보증

       - 데이터 받으면 받았다고 전송해줌

    3. 순서 보장

       - 순서가 제대로 오지 않으면 서버가 다시 보내라고 함

       

- ### UDP 특징

  사용자 데이터그램 프로토콜

  - IP와 비슷한 기능

  - 출발지 PORT, 도착지 PORT가 같은지 비교

  - TCP는 국룰 인터넷 프로토콜이므로 손 대기 힘들지만

    UDP는 최근에 기능 추가하면서 각광 받고 있음



### PORT

- 한 번에 둘 이상 연결해야 하면?

  - 클라이언트가 게임하면서 화상통화를 하고 있는데 다른 서버에 웹 브라우저 요청까지 한다면
  - 같은 IP면 포트로 구분

- IP가 아파트 이름이면 PORT는 동호수

- 대표적인 포트

  ![image-20210728200256939](C:\Users\thwjd\AppData\Roaming\Typora\typora-user-images\image-20210728200256939.png)



### DNS

​	IP는 기억하기 힘들고 바뀔 수 있음

​	DNS 서버에 도메인 명을 등록하면

​	도메인 명을 치면 DNS 서버에서 IP를 클라이언트 서버로 전송

​	클라이언트가 받은 IP로 접속

​	- IP가 바뀌어도 도메인으로 접속 가능



### URI(유니폰 리소스  아이덴티파이어)

- URI는 URL - 리소스 위치_로케이터(locater), URN - 리소스 이름_이름(Name)을 모두 포함한 개념
- URN만으로는 리소스를 찾기 힘들어서 URI와 URL이 같은 의미로 쓰임
- URL 분석
  - 



### 웹 브라우저 요청 흐름

URL 보내면 먼저 DNS 조회(도메인)

HTTP 요청 메시지 생성(GET /)

SOCKET 라이브러리를 통해 TCP/IP로 연결

TCP/IP에서 패킷 생성, HTTP 메시지 포함

인터넷에 요청 패킷 보내짐

서버가 요청 패킷 받음

서버가 TCP/IP 패킷 안의 HTTP 메세지 해석

서버가 HTTP 응답 메서지 생성하고 TCP/IP 패킷 안에 넣음

이 때 HTTP 메세지 안에 HTML도 있어서

응답 패킷을 받은 클라이언트가 HTML 렌더링해서 화면에 띄움



### HTTP란

처음에는 HTML만 보내는 전송 프로토콜로 시작했지만

지금은 이미지, 동영상, JSON, XML, 서바간 데이터 통신할 때도 HTTP 사용

HTTP로 모든 것이 통신 가능해짐! 국룰 전송 프로토콜이 됨

- HTTP/1.1이 현재 국룰 버전



### HPPT 특징

- 클라이언트(요청) - 서버(응답) 구조

  - 클라이언트는 서버에 요청을 보내고 응답을 대기
  - 클라이언트에는 UI만 집중
  - 서버에는 비즈니스 로직, 복잡한 데이터만 집중
  - 클라이언트와 서버가 분리

- 무상태프로토콜(스테이스리스)

  - 서버가 클라이언트 상태를 보존X

    - 상태유지 - Stateful

      - 서버가 전 상태를 계속 유지하는 것
      - 고객의 요청을 가지고 있는 서버가 고장나면 요청도 사라짐

    - 무상태 - Statelesee

      - 클라이언트 요청이 증가해도 서버 증설 가능

      - 서버 확장성 용이

      - 클라이언트가 요청을 기억함

      - 서버는 상태를 보관하지 않고 응답만 해줌

      - 한 서버가 고장나도 중계서버가 다른 서버에 요청 보내서 해결

      - ##### stateless 한계

        - 상태 유지 ex)로그인 를 해야할 때
          - 로그인 했다는 상태를 서버에 유지
          - 일반적으로 쿠키와 세션등을 이용해 상태 유지
          - 상태유지 stateless는 최소한만 사용할 것!

- 비연결성

  - 서버가 요청받고 응답한 후 연결 유지
    - 서버는 자원 아낄 수 있음
  - HTTP는 기본적으로 연결을 유지하지 않는 모델
  - 초 단위 이하의 빠른 속도로 응답하므로 동시간대 이용자가 많아도 실제로 요청(계속 버튼을 누르지 않음)은 별로 없어서 서버 자원을 적게 쓸 수 있음
  - 한계
    - 요청할 때 마다 TCP/IP 연결을 새로 맺어야함 - 3 악수 시간 추가
    - 웹 사이트 요청할 때마다 HTML과 CSS, JS, 이미지 등 계속 다운로드 해야함
    - HTTP 지속연결
      - 요청과 동시에 HTML, JS 를 함께 받음



### HTTP 메세지

- HTTP 요청메세지

  ​	GET(요청메세지 EX.DELETE,  PUT...) /URL 쿼리문

  ​	HOST: 도메인

- HTTP 응답메세지

  

- HTTP 헤더
  - HTTP 전송 위한 모든 부가정보가 다 있음
  - HTML등 포함

- HTTP 메세지 바디
  - 실제 전송할 데이터
  - 바이트로 표현할 수 있는 모든 데이터 들어감
- HTTP는 크게 헤더, 바디로 구분
- 단순함



### HTTP 메서드

- 요구사항

  - 회원관리 API를 만들어라

    - 회원이라는 개념 자체가 리소스

    - 리소스 식별을 어떻게할까

      - 회원이라는 리소스만 식별하면 됨

        /members/{id} => 회원 등록, 수정, 삭제 등을 어떻게 구분할 것인가

    - 리소스와 행위를 분리

      - url는리소스만 식별
      - 행위: 조회, 등록, 삭제, 변경

    - 리소스는 명사, 행위는 동사

GET: 리소스 조회

POST: 요청 데이터처리, 등록

PUT: 리소스를 대체, 해당 리소스가 없으면 생성

PATCH: 리소스 부분 변경

DELETE: 리소스 삭제



### GET

- 클라이언트가 서버에 리소스 조회 요청
- 서버에 전달하고 싶은 데이터를 GET/ 쿼리문으로 요청
  - GET/ 이거 내놔



### POST

- 메세지 전달/ 요청 데이터 처리
- POST /members/{id}
  - 이 멤버 저장

- POST 요청이 오면 요청 데이터를 어떻게 처리할지 리소스마다 따로 정해야함

- 1. 주로 새 리소스 생성(등록)

- 2. 요청 데이터 처리

     값 생성, 변경하는 것을 넘어 프로세스 상태 변경될 수도 있음

     EX) 컨트롤 URI

  3. 다른 메서드로 처리하기 애매한 경우

     애매할 때는 POST



### PUT

- 리소스 대체

  리소스가 있으면 대체(덮기) 없으면 생성

  ex. 멤버의 age만 수정하고 싶어서 put age만 했는데 username도 없어지고 age만 저장

  POST와 차이점

  - 클라이언트가 리소스 위치를 알고 URI 딱 지정



### PATCH

- PUT은 리소스를 대체하는 거고 PATCH가 수정
  - 리소스 부분 수정
  - age만 부분 변경 가능

### DELETE

- 리소스 제거
