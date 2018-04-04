---
title: '기초를 알자 - 네트워크 프로토콜'
author: imjinbro
date: 2018-04-04 18:25
tags: [network, web]
---  

## 통신
* 쌍방 간의 통신
* 요청과 응답
  
## 프로토콜
* 통신을 위해 꼭 필요함
* 통신 상대를 찾기도, 어떤 내용을 어떻게 전달할지도 약속에 의해 모든게 진행됨 : 서로 다른 하드웨어, 소프트웨어 환경
  
## TCP/IP
* 네트워크에 연결된 컴퓨터(IP Address 부여)간에 데이터(패킷 형태로 됨)를 주고 받기위한 프로토콜
* 프로토콜에서 핵심적으로 사용되는 프로토콜이 TCP(패킷 프로토콜 - 연결을 어떻게 하고, 데이터 순서 형태), IP(인터넷 환경 프로토콜 - 어느 라우터로 가야할지)라 TCP/IP로 불림
* 계층형 프로토콜 : 한번에 정보를 작성하는게 아니라 4개의 계층별(각각 프로토콜 그게 4개)로 각각이 맡고 있는 정보를 작성하는 방식(계층 독립적, 그러나 연관된 내용 - 어디로 어떤 내용을)  
![TCP/IP Stack](/files/tcp-ip-stack.jpg) 
  
* IP : 4계층 중 2번째 계층 프로토콜, 인터넷에 연결된 컴퓨터를 찾아가기위해서 어떤 라우터를 거쳐야 빠르게 갈 수 있는지를 찾고 그에 대한 내용을 담는 프로토콜
* TCP : 4계층 중 3번째 TCP/UDP 계층의 프로토콜, 찾아놓은 길(라우터 - IP 담당)에 어떤 순서, 형태로 보낼지 정함, 연결지향, 상대가 패킷 수신에 대해서 정상적으로 전송되었다고 보고 다음 패킷 전송
* HTTP : 4계층 중 4번째 어플리케이션 계층의 프로토콜, 하위 계층에서 어떤 컴퓨터, 어떻게 데이터 연결, 전송에 대해서 정했다면 클라이언트와 서버가 무슨 데이터를 전송 받을 것이고에 대한 구체적인 내용 기술
* 컴퓨터 식별자 : IP Address(실제로는 MAC Address) - 인터넷 프로토콜(IP) 계층
* 어플리케이션 식별자 : 포트번호(HTTP 기반 프로그램은 80을 사용) - 어플리케이션 프로토콜 계층
  
## HTTP
* 어플리케이션 계층 프로토콜 : 어떤 데이터를 요청, 응답하는지 등의 내용을 담는 프로토콜
* 클라이언트(웹브라우저) 호스트와 서버 호스트 간의 문서(.html) 통신 규약
* 데이터 타입 : html 뿐만 아니라 동영상, 이미지, js, css 등도 통신할 수 있도록 지원
* 요청과 응답 HTTP 메시지 예시 : 위가 요청, 아래가 응답(다른 메시지 구성)
* 스테이트리스 : 이전의 통신에 대해서 전혀 알지 못함(전혀 상관 없는 통신, 상태를 저장하지않음, 그때 뿐인 연결임, 각각 다른 연결임)
  * 장점으로는 서버가 클라이언트의 상태를 저장하기 시작하면.... 클라이언트가 많아질수록..... 또한 서버 프로그램이 1대의 컴퓨터가 아니라 여러 컴퓨터에서 돌고 있으면 해당 클라이언트는 같은 서버만 사용하거나 모두 같은 세션 상태를 동기화해야함
  * 단점을 극복하기위해 브라우저에 쿠키가 존재함 : 일정기간 상태를 저장해두는(세션 상태라고 함)

  
~~~
/* request */
GET /static/newsstand/up/2017/0424/nsd151929775.png HTTP/1.1
Host: s.pstatic.net:443
Accept: image/webp,image/apng,image/*,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
Referer: https://www.naver.com/
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36

/* response */
HTTP/1.1 200
accept-ranges: bytes
access-control-allow-origin: *
age: 487233
cache-control: max-age=604800
content-length: 16101
content-type: image/png
date: Tue, 03 Apr 2018 00:51:13 GMT
expires: Mon, 02 Apr 2018 12:34:35 GMT
last-modified: Mon, 24 Apr 2017 06:19:29 GMT
server: Testa/4.8.6
status: 304
~~~
  
* GET 요청(1) : 요청라인과 요청헤더만 존재함
  
~~~
GET /index.html HTTP/1.1
Host: localhost:8080
Connection: keep-alive
Cache-Control: max-age=0
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36
Upgrade-Insecure-Requests: 1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
~~~
  
* GET 요청(2) : 서버에 데이터를 전달할 때 쿼리스트링(문자열 데이터)을 사용함, ?를 기준으로 왼쪽은 path, 오른쪽은 쿼리스트링
  
~~~
GET /search?newwindow=1&ei=PHrEWv2SO8G60gSM1LzoCw&q=%EC%BD%94%EB%93%9C%EC%8A%A4%EC%BF%BC%EB%93%9C&oq=%EC%BD%94%EB%93%9C%EC%8A%A4%EC%BF%BC%EB%93%9C&gs_l=psy-ab.3...54628.55756.0.55939.12.10.0.0.0.0.147.689.1j5.6.0....0...1.1j4.64.psy-ab..10.2.212.0..0j35i39k1j0i20i263k1.0.zUQhgBD7ZA8 HTTP/1.1
Host: www.google.co.kr:443
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: 생략
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36
X-Client-Data: CIy2yQEIKPKAQ==
~~~
  
* POST 요청 : 요청라인, 요청헤더 그리고 요청바디가 있음(헤더와 바디 사이에 공백라인이 있어 파싱할 때 라인을 기준으로 나눔)
  
~~~
POST /nidlogin.login HTTP/1.1
Host: nid.naver.com:443
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate, br
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
Content-Type: application/x-www-form-urlencoded
Cookie: 생략
Origin: https://www.naver.com
Referer: https://www.naver.com/
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36

parameter=value&also=another
~~~
* GET과 POST 차이 : POST는 url에 전달 데이터를 드러내지않고 HTTP content에 내용을 적는 형태
* 동기형 프로토콜 : 요청에 대한 응답이 돌아올 때까지 응답 대기상태(timeout까지)
  
## 기타 
* DNS : 도메인 네임을 IP 주소로 변경해줌
* 패킷 : IP 기반 통신에서 호스트끼리 데이터를 주고 받을 때의 단위, 전화선처럼 한 회선을 점유한 상태에서 통신하지않고 데이터를 여러개(패킷)로 조각내어 통신함
  
## reference
* [TCP/IP 프로토콜 스택](https://www.youtube.com/watch?v=S55YV8fWbpU)
* [웹을 지탱하는 기술](http://www.yes24.com/24/goods/5170353)
* [HTTP 헤더 옵저버 확장프로그램](https://chrome.google.com/webstore/search/http?hl=ko&_category=extensions)

