---
title: 'TIL - 5월 두번째 주말'
author: imjinbro
date: 2018-05-14 00:41
tags: [TIL, spring-boot, linux, algorithm]
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 쓰면서 배우는 스프링부트
### 뷰 중복 코드 없애기
* 뷰 중 container(contents)부분만 다르고, head, nav, footer는 코드가 중복됨
  * 하나가 변경되면 모든 뷰 파일 코드 변경해야했음

* 템플릿 엔진 기능 중 부분(partial)뷰를 만들고 템플릿 문법으로 include 시키는 기능이 있음
* 아래와 같이 디렉토리를 새로 만들어서 부분 뷰를 만들어놓음
  
![](/files/2018-05-second-weekend-TIL/resources-list.png)
  
~~~
/* /partial/header */
<head>
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <meta charset="utf-8">
    <title>Colin</title>
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <link href="/css/bootstrap.min.css" rel="stylesheet">
    <!--[if lt IE 9]>
    <script src="//html5shim.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->
    <link href="/css/styles.css" rel="stylesheet">
</head>
~~~
  
* 템플릿엔진 문법을 통해서 뷰 파일에 include 시킴
   
~~~
<!DOCTYPE html>
<html lang="kr">
	{{> partial/header }}
	
	<body>
	</body>
</html>
~~~
  
### 컨트롤러 - 뷰 맵핑 설정
* 뷰 중복 코드를 없애면서 static에 있던 뷰 파일을 templates로 옮겨오게됨 
  * 그에 따라 컨트롤러 만들고 URL과 맵핑해주는 메소드를 만들어야했음
  * 즉, URL 요청 시 뷰 파일만 리턴해주는 아무런 로직없는 메소드가 여러개 만들어짐, 코드만 늘어난 격

* 이를 코드에 만들지않고 설정만 잡아줌으로서 스프링이 내부적으로 만들어줌 : 코드를 깔끔하게 관리할 수 있음
  * WebMvcConfigurer를 구현하는 클래스를 만들면 됨 : Springboot 2.0.1.RELEASE 기준(Spring5) 

~~~
@Configuration
public class MvcConfig implements WebMvcConfigurer {

	@Override
    public void addViewControllers(ViewControlleRegistry registry) {
        registry.setOrder(Ordered.HIGHEST_PRECEDENCE);
        
        registry.addViewController("/user/form").setViewName("/user/form");
    }
}


~~~

* 컨트롤러 내 URL - 액션 맵핑되어있는 URL은 할 필요없음 : 이미 만들어져있기때문에 만들면 우선순위 설정에 의해 잘못된 맵핑됨(로직X)
* ViewControllerRegistry : 컨트롤러 - 뷰 맵핑을 간편하게 해주는 객체
    
### @RequestMapping과 @GetMapping, @PostMapping
* 세가지 어노테이션 모두 URL - action을 맵핑하는 용도임
* @RequestMapping : 모든 HTTP 메소드에 대한 처리가 가능한 상태, method value를 지정해주지않으면 모두 통용됨
  * 겹치는 url이 있다면 method 지정을 꼭해줘야해서 코드가 오히려 더 많아짐
  
~~~
@Controller
public class UserController {

	@RequestMapping(value = "/users", method = "RequestMethod.GET") 
	public String showAllUser() {
		
	}

	@RequestMapping(value = "/users", method = "RequestMethod.POST") 
	public String create(User user) {

	}
}
~~~
  
* @GetMapping, @PostMapping : 특정 HTTP method에 대해서 맵핑할 때 사용
  
~~~
@Controller
public class UserController {

	@GetMapping("/users")
	public String showAllUser() {
		
	}

	@PostMapping("/users")
	public String create(User user) {

	}
}
~~~
  
### @RequestMapping의 올바른 사용
* 컨트롤러의 공통되는 URL을 맵핑해두고, 뒷부분 URL은 각자 액션(메소드)에서 맵핑하도록 하면 중복 제거를 할 수 있음

~~~
@Controller
@RequestMapping("/users")
public class UserController {
	
	@PostMapping
	public String create(User user) {
	
	}
}

~~~

### 하나의 URL은 하나의 액션(메소드)과만 맵핑되어야함
* 일종의 객체지향 원칙이랄까 : 수정할 부분이 많아지는 것과는 다른 맥락이지만...!
  * 하나의 URL이 여러 역할(액션 메소드)을  가진다면 에러가 남, 무엇과 맵핑되어야할지 모르기때문에

### form input 개수와 메소드의 파라미터 개수가 일치해야함
* 일치하지않으면 임의의 자리 값에 null이 계속 들어오는 문제가 생김
* input의 name과 bean 객체의 인스턴스를 생성할 때만 인스턴스 변수명이 같아야하고, 메소드로 받을 때는 일단 개수가 같아야함
  
### 기타 - 만들면서 현재 문제점이라고 생각되는 것
* 데이터베이스 대신 메모리(ArrayList)에 저장하고 있음 
  1. ArrayList size를 조회한 뒤 size + 1로 id값을 지정해주는데, 만약 중간 게시글이 삭제된다면?
  
  ~~~
  중간 게시글을 삭제한 상태에서 게시글이 생성되었을 때 중복 id값이 지정될 수 있음 : 중복을 피하기위해서는 삭제될 때 모든 게시글의 id값을 -1 해줘야함
  ~~~
  
  2. index에 게시글을 뿌려줄 때 최신글이 아래로 가도록 되어있음 : ArrayList에서 순차적으로 뽑아서 <li>를 생성하고 있기때문

  ~~~
  보여줄 때마다 id값을 기준으로 내림차순을 해야하는데, add 될 때마다 그 작업을 해줘야할까?
  ~~~
  
### 참고자료
* [Springboot docs - ViewControllerRegistry](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/ViewControllerRegistry.html)
  
## 리눅스
### 시그널
* 특정상황이 발생했을 때 운영체제(커널)가 프로세스에게  상황을 알리기위한 방법 : 약속해놓은 상수값
* 시그널 종류(지금까지 본 것만)
  
| 시그널 | 발생 상황 |
|---|---|
| SIGHUP | 터미널 인터페이스에 의해 연결이 단절되었을 때, 터미널과 연결된 프로세스에 전달 |
| SIGINT | 인터럽트 키(ctrl + c)가 입력되었을 때 프로세스에 전달 |
  
* 시그널 핸들러 : 특정한 시그널이 발생했을 때 적절히 처리해주는 메소드
  * 기본적으로 시그널에 대한 디폴트 처리 방법이 있음 : 프로세스를 종료시키거나 무시하거나 등

* **프로세스와 사용자는 직접 대화하지않고, 특정 인터페이스를 통해 대화를 함** : 사용자 - 인터페이스 - OS - 프로세스

### 참고자료
* 구글링, 유튜브 검색
  
## 알고리즘
* 하루 한 문제라도 꾸준하게 풀자
  
### [boj] Q1110
* 문제 읽기 연습됨(N과 num 할당시기), 자리수 문제(나누기, 나머지 연산 연습)
  
### [bog] Q4344
* int 타입 두 변수를 가지고 double 타입 결과를 구할 때 연산 전에 하나의 변수라도 double로 캐스팅하고 연산해야함
* 테스트 케이스수 말고 1번의 테스트에서 데이터 개수에 따라 연산 횟수가 몇번 이뤄지는지가 중요
  