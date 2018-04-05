---
title: '상속을 extends답게'
author: imjinbro
date: 2018-03-27 20:58
tags: [oop,java]
sitemap :
  changefreq : daily
  priority : 1.0
---

## 상속은 inheritance가 아니라 확장(extend)
  
```
public class Car {

}

public class Sonata extends Car {

}
```
* 상속을 한다는 것은 타입을 세분화(하위 타입 생성)시키겠다는 것
* 하위클래스는 상위타입의 한 종류이다 : **is a kind of**
  
## 하위클래스와 상위클래스의 관계
![부분집합](/files/extends.png)
* 상-하위 관계를 설계해두면? 중복 코드를 줄이면서도, 다형성을 적극 활용할 수 있음
  
## 추상클래스(abstract class)
  
```
public abstract class Car implements Movable {
    
    public abstract void method();
}

public class Sonata extends Car {

    @Override
    public void method() {
              
    }
}
```

* 자바에서는 상위클래스에서 method를 구현하지않고, 상위클래스 extends하는 하위클래스에서 method를 각각 구현하도록 하는 abstract class도 있다.
* *interface와 abstract class은 언제 사용할까에 대해서 나의 의견은 다음 포스팅에 써볼 생각*      
  
## 일관성 있게 extends 하자! 리스코프 치환 원칙
  
```
Car car = new Sonata();
car.run();
```
* 상위 타입 변수에 하위 타입 인스턴스의 주소값이 저장해도 되도록 설계하자, 일관성 있게 extends 하자
* 다형성에 의해 run()은 Sonata에서 오버라이딩된 메소드가 동작할 것 
* 상속을 상속답게 하자  
  * 비행기가 활주로에서 달려서 Car의 run()과 같은 메소드가 필요하다, 그렇다고해서 Car의 run()을 사용하기위해 Car를 상속받지말자
  
## 참고
* [스프링 입문을 위한 자바 객체지향의 원리와 이해](http://wikibook.co.kr/java-oop-for-spring/)  