# DNS 동작원리

---

## DNS란?

- www.example.com과 같이 사람이 읽을 수 있는 이름을 192.0.2.1과 같은 숫자 IP 주소로 변환하여 컴퓨터가 서로 통신할 수 있도록 한다.
- 인터넷의 DNS 시스템은 이름과 숫자 간의 매핑을 관리하여 마치 전화번호부와 같은 기능
- DNS 서버는 이름에 대한 요청을 IP 주소로 변환하여 최종 사용자가 도메인 이름을 웹 브라우저에 입력할 때 해당 사용자를 어떤 서버에 연결할 것인지를 제어한다. 이 요청을 쿼리라고 한다.

## Domain 구조

- 인터넷상에서 사용되는 도메인은 전 세계적으로 고유하게 존재하는 이름
- 정해진 규칙 및 체계에 따라야 하며, 임의로 변경되거나 생성될 수 없음.
- 인터넷상의 모든 도메인은 ".(dot)" 또는 루트(root)라 불리는 도메인 아래에 그림과 같이 나무를 거꾸로 위치시킨 역트리(Inverted tree)구조로 계층적으로 구성되어 있음
- 루트 도메인 바로 아래의 단계를 1단계 도메인 또는 최상위도메인(TLD, Top Level Domain)이라고 부르며, 그 다음 단계를 2단계 도메인(SLD,Second Level Domain)이라고 함
- 도메인은 일반최상위도메인(GTLD:Generic Top Level Domain)과 국가최상위도메인(CCTLD:Country Code Top Level Domain)로 구분할 수 있으며 여기서 일반최상위도메인은 다시 스폰서도메인(Sponsored TLD)과 언스폰서도메인(Unsponsored TLD)으로 구분됩니다.

![도메인 구조](https://t1.daumcdn.net/cfile/tistory/997DA9405BDFB7B71E)

## DNS 서비스 유형

---

### 신뢰할 수 있는 DNS

- 개발자가 퍼블릭 DNS 일므을 관리하는데 사용하는 업데이트 메커니즘을 제공하며, 이를 통해 DNS 쿼리에 응답하여 도메인 이름을 IP 주소로 변환
- 신뢰할 수 있는 DNS는 도메인에 대해 최종 권한이 있으며 재귀적 DNS 서버에 IP 주소 정보가 담긴 답을 제공할 책임이 있음

### 재귀적 DNS

- 보통 클라이언트는 신뢰할 수 있는 DNS 서비스에 직접 쿼리를 수행하지 않고, 해석기 또는 재귀적 DNS 서비스라고 알려진 다른 유형의 DNS 서비스에 연결하는 경우가 일반적임
- 재귀적 DNS 서비스는 호텔 컨시어지와 같은 역할
- DNS 레코드를 소유하고 있지 않지마 ㄴ사용자를 대신해서 DNS 정보를 가져올 수 있는 중간자의 역할
- 일정 기간 동안 캐시된 또는 저장된 DNS 레퍼런스를 가지고 있는 경우, 소스 또는 IP 정보를 제공하여 DNS 쿼리에 답을 하거나, 해당 정보를 찾기 위해 쿼리를 하나 이상의 신뢰할 수 있는 DNS 서버에 전달

## DNS 동작 원리

---

![DNS 동작 원리](https://t1.daumcdn.net/cfile/tistory/99C16C455BDFBB2A23)

1. 웹 브라우저에 www.naver.com을 입력하면 먼저 Local DNS에게 "www.naver.com"이라는 hostname"에 대한 IP 주소를 질의하여 Local DNS에 없으면 다른 DNS name 서버 정보를 받음(Root DNS 정보 전달 받음)

2. Root DNS 서버에 "www.naver.com" 질의

3. Root DNS 서버로 부터 "com 도메인"을 관리하는 TLD (Top-Level Domain) 이름 서버 정보 전달 받음

4. TLD에 "www.naver.com" 질의

5. TLD에서 "name.com" 관리하는 DNS 정보 전달

6. "naver.com" 도메인을 관리하는 DNS 서버에 "www.naver.com" 호스트네임에 대한 IP 주소 질의

7. Local DNS 서버에게 "응! www.naver.com에 대한 IP 주소는 222.122.195.6 응답

8. Local DNS는 www.naver.com에 대한 IP 주소를 캐싱을 하고 IP 주소 정보 전달

### 출처

[한량 개발자](https://ijbgo.tistory.com/27)

<div style="text-align: right">작성일 : 2021.02.10</div>
