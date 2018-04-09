---
title: 'enum 알고 사용하자'
author: imjinbro
date: 2018-04-05 14:21
tags: [java]
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---
  
## enum
* 상수값을 모아서 관리하기
* 문자열이었다면 많은 예외처리를 해야할 수 있지만, **값의 범위를 강제해버리기떄문에 안전한 프로그래밍이 가능함**
    * 예를 들어 0 ~ 6까지 전달받아야하는데 래핑시켜서 그 값들만 받게끔 할 수 있음 : 유효성 체크 메소드 따로 만들지않고도
    * 예시 2 : 방향 4가지만 줄 수 있도록 강제
  
~~~
/* example */
public enum Direction { 
    NORTH,
    SOUTH,
    EAST,
    WEST;    
}

public class Ladder {
    public void move(Direction direction) {

    }
}
~~~
  
* 클래스의 인스턴스 개수를 제한해버리고 만듦 : static 환경에서 만든다, **생성자가 private**
  * enum에 선언한 상수값들은 유일한 인스턴스 
  * 메모리 주소가 항상 같음 : 같은 값일 때, == 과 equals 동작이 같음
  * 인스턴스라는 것을 기억하기 : 여기도 객체니깐 물어봐라
  
~~~
public enum Rank {
    FIRST(6, 100000),
    SECOND(5, 10000);
    
    private int matchPoint;
    private int winnerPrize;
    
    Rank(int matchPoint) {
        this.matchPoint = matchPoint;
    }    
    
    public boolean isFirst() {
        return FIRST == this;
    }
    
    /* 상태값을 그대로 리턴하는 get을 쓰지않고 이렇게 요청하는 방식으로 똑같이 할 수 있다 */
    public int getTotalPrize(int matchCount) {
        return this.winnerPrize * matchCount;
    }
}
~~~
  
* **상태값이 변경되는 값을 enum에서 관리하면 안됨**
  * 유일한 인스턴스라서 멀티쓰레드 환경에서 위험함 : race condition 발생
  * 고정되어있는 값만 관리하자 : final로 막기
  
~~~
public enum Rank {
    private final int matchPoint;
    private final int prize;

    Rank(final int matchPoint, final int prize) {
        this.matchPoint = matchPoint;
        this.prize = prize;
    }
}
~~~
