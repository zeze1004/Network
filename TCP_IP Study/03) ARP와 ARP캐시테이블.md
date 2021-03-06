# TCP/IP

## ARP 캐시 테이블



### IP주소와 맥주소의 차이

| 구분    | 구성 체계                                      | 기능                  |
| ------- | ---------------------------------------------- | --------------------- |
| IP 주소 | 가변적인 32비트 크기의 네트워크 ID와 호스트 ID | IP 주소 기반 라우팅   |
| 맥 주소 | 고정적인 48비트 크기의 OUI와 일련번호          | 맥 주소 기반의 스위칭 |



**ARP**: Adderess Resolution Protocol

```shell
PS C:\Users\zeze> ping 8.8.8.8

Ping 8.8.8.8 32바이트 데이터 사용:
8.8.8.8의 응답: 바이트=32 시간=36ms TTL=114
8.8.8.8의 응답: 바이트=32 시간=35ms TTL=114
8.8.8.8의 응답: 바이트=32 시간=36ms TTL=114
8.8.8.8의 응답: 바이트=32 시간=37ms TTL=114

8.8.8.8에 대한 Ping 통계:
    패킷: 보냄 = 4, 받음 = 4, 손실 = 0 (0% 손실),
왕복 시간(밀리초):
    최소 = 35ms, 최대 = 37ms, 평균 = 36ms
```

`ping`: 출발지 호스트와 목적지 호스트 사이에서 회선 연결 상태나 목적지 OS 동작 여부 등 점검위해 사용하는 명령어

`ping 8.8.8.8`: 출발 IP 주소에서 목적지 IP 주소(구글에서 제공하는 DNS 서버의 IP)까지 임의의 데이터 전송, 목적지 호스트는 이를 수신해 다시금 전송

> 결과적으로 출발지, 목적지 사이의 회선과 두 호스트의 동작 상태는 정상



ARP 캐시테이블: IP주소와 맥 주소의 대응 관계를 저장한 테이블

`arp`: 연결하려는 시스템을 IP 주소 기반으로 MAC 주소를 알아와서 출력하는 커멘드

```shell
PS C:\Users\zeze> arp -a # 연결된 모든 호스트를 보여줌

인터페이스: 172.30.1.53 --- 0x6
  인터넷 주소           물리적 주소             유형
  172.30.1.254          1c-ec-72-08-f5-82     동적
  172.30.1.255          ff-ff-ff-ff-ff-ff     정적
  224.0.0.2             01-00-5e-00-00-02     정적
  224.0.0.22            01-00-5e-00-00-16     정적
  224.0.0.251           01-00-5e-00-00-fb     정적
  224.0.0.252           01-00-5e-00-00-fc     정적
  239.255.255.250       01-00-5e-7f-ff-fa     정적
  255.255.255.255       ff-ff-ff-ff-ff-ff     정적
```

출발지 IP 주소가 갖고 있는

무선공유기의 IP 주소(인터넷 주소)와 맥 주소(물리적 주소) 보여준다

카페에서 여러 와이파이를 써서 그런가 엄청 많다!0!



`arp 옵션`

-v : ARP 상태를 출력

-t type : ARP 캐시에 올라와 있는 타입을 검색 ether(Enternet), ax25(AX25 packet radio)등이 있으며, ether가 기본 타입

-a [host] : 등록된 호스트 중 지정한 호스트의 내용을 보여줌 호스트를 지정하지 않으면 등록된 모든 호스트를 출력

-d [host] : 지정한 호스트를 목록에서 삭제

-s host hardware-address : 호스트의 하드웨어 주소. 즉, 호스트 MAC 주소를 추가 이더넷 카드의 경우 6자리의 16진수로 되어 있음

-f file : 파일에 있는 목록을 추가

출처: https://gregorio78.tistory.com/258 [알아두면 쓸데있는 IT 잡학사전]



`arp -d`: 호스트 삭제

```shell
PS C:\Users\zeze> arp -d
ARP 항목을 삭제하지 못했습니다. 요청한 작업을 수행하려면 권한 상승이 필요합니다.

PS C:\Users\thwjd> start-process powershell ?verb runAs   # 파워셀 권한 상승

PS C:\WINDOWS\system32> arp -d
PS C:\WINDOWS\system32> arp -a

인터페이스: 172.30.1.53 --- 0x6
  인터넷 주소           물리적 주소           유형
  172.30.1.254          1c-ec-72-08-f5-82     동적
  224.0.0.2             01-00-5e-00-00-02     정적
  224.0.0.22            01-00-5e-00-00-16     정적
```

9줄에서 3줄로 줄었다

왤까? 차차 알아보도록 하자



## ping 8.8.8.8

위에서 ping을 보낼 때 어떤 과정이 일어나는지 알아보도록 하자

참고로 8.8.8.8이 아니라 ping zeze.kr(도메인 네임) 입력해도 도메인 네임이 목적지 ip 주소에 해당



#### ping 8.8.8.8 입력

>엔터를 누르면 명령어 입력 후 모든 처리를 os에 맡기겠다는 뜻

1. os가 자기가 사용하는 서브넷 마스크 255.255.255.0을 갖고 아래로 설정

   출발지 ip: 172.30.1.53 255.255.255.0

   목적지 ip: 8.8.8.8 255.255.255.0

   ​	네트워크 ID가 다름(172.30.1, 8.8.8) 

   - 목적지가 다른 LAN 영역에 있다는 것을 의미

   - OS는 목적지 IP주소를 라우터 IP 주소(무선공유기IP)로 변경

     | 구분    | 출발지            | 목적지       |
     | ------- | ----------------- | ------------ |
     | IP 주소 | 172.30.1.53       | 172.30.1.254 |
     | 맥 주소 | D8-F2-CA-52-33-7A |              |

     

2. 목적지의 맥 주소 알아내기

   - 같은 LAN 역역에 있으면 게이트웨이까지 스위칭 가능

   - 브로드 캐스트 방식으로 ARP 질의 전송

     :172.30.1.254번(목적지 IP)에 대응하는 맥 주소를 구하기 위해 같은 LAN영역 전체를 대상으로 모든 호스트에게 데이터 전송

   - 유니캐스트 방식으로 답장

     :172.30.1.254번을 사용하는 게이트웨이가 172.30.1.53번이 내 MAC 주소를 요청한다는 것을 알고 특정한 호스트

     (나)에게 데이터 전송

   - ARP 질의 후 ARP 캐시 테이블

     | 구분    | 출발지            | 목적지       |
     | ------- | ----------------- | ------------ |
     | IP 주소 | 172.30.1.53       | 172.30.1.254 |
     | 맥 주소 | D8-F2-CA-52-33-7A | WI-FI 맥주소 |

3. ARP 캐시 테이블 참조해 핑 데이터를 유니캐스트 방식으로 게이트웨이까지 전송
   - 이후에는 게이트웨이 IP(WIFI) 주소에 기반한 라우팅 통신을 통해 전송



### ARP 요청과 응답 과정 정리

1. OS가 출발지 서브넷 마스크 기준으로 출발지 네트워크 ID와 목적지 네트워크 ID 비교해

   목적지가 스위칭 통신인지 라우팅 통신 대상인지 판단

2. OS가 ARP 캐시 테이블에 접근해 목적지 맥 주소 검색

   - 목적지가 스위칭 통신 대상: 

     실제 목적지 IP 주소에 해당하는 맥 주소를 ARP 캐시테이블에서 검색

   - 목적지가 라우팅 통신 대상:

     라우터 IP 주소에 해당한느 맥 주소를 ARP 캐시 테이블에서 검색

3. ARP 캐시 테이블에 목적지 맥 주소의 유무에 따른 데이터 전송 방식 결정

   - 유니캐스트: ARP 캐시 테이블에 목적지 맥 주소 존재하면 OS는 곧바로 해당 맥 주소 참고해 목적지까지 유니캐스트 방식으로 전송 
   - 브로드캐스트: OS는 자신이 속한 LAN 영역 전채를 대상으로 ARP 브로드캐스트 질의 전송

4. 응답 받은 호스트(출발지와 동일한 LAN 영역)가 ARP 유니캐스트 응답을 출발지 호스트에게 전송

5. OS는 ARP 유니캐스트 응답으로 받은 MAC 주소를 ARP 캐시테이블에 반영

6. OS는 사용자가 전송하고픈 실제 데이테를 ARP 캐시 테이블에 기반해 목적지 호스트로 유니캐스트 방식으로 전송



#### ARP 영역

: ARP 요청과 응답이 일어나는 공간 

- ARP 요청과 응답은 같은 LAN 영역 내에서 일어나므로 ARP 영역이 곧 동일 LAN 영역을 의미함



#### LAN 영역 내에서 아래 개념이 사용

| 관점        | 정의                                     |
| ----------- | ---------------------------------------- |
| 네트워크 ID | 동일한 네트워크 ID를 공유하는 공간       |
| 스위칭      | 맥 주소에 기반해 내부 통신이 가능한 공간 |
| ARP 영역    | 단일한 ARP 영역을 생성하는 공간          |



















































