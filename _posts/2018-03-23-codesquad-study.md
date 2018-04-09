---
title: '객체지향 프로그래밍할 때 기억하기'
author: imjinbro
date: 2018-03-25 19:26
tags: [oop,java]
comments: true
sitemap :
  changefreq : daily
  priority : 1.0
---

## 시작하기
코드스쿼드에 들어와서 프로그래밍을 하고 피드백 받는 과정에서 그동안 어렴풋한 개념으로 알고 있었던 것들을 확실하게 알게됨 그 내용들을 두고두고 참고하기위해서 정리를 시작함 많은 내용이 있지만 먼저 정리한 것부터 조금씩 조금씩 글을 쓸 예정  
  
## 객체 간의 요청 - 응답으로 프로그램이 완성시키기
  
```
// CoordinateMain.java

List<Point> points = Input.getPoints("좌표를 입력해주세요.");
Figure triangle = new Triangle(points);
Viewer.viewAtCoordinate(triangle);
```
* 객체에 역할을 할당했다면, 역할에 해당하는 일을 싸그리 맡기기  
* 역할을 맡은 객체가 어떻게 처리하는지는 중요하지않고, 응답 결과만 얻어오면됨  
  
## 보기 좋은 코드 만들기
  
```
for (int i = 0; i < points.length; i++) {
    for(int j = 0; j < points[i].length; j++) {
    	if(points[i][j]) {
        	
        }
    }
}
```
* for 문이 중첩되어있는 코드를 짜놓으면 편하다, 그렇지만 이후 유지보수나 다른 개발자가 코드를 봤을 때 가독성이 떨어진다(depth 증가)  
* 아래처럼 객체화하면 이중 배열로 복잡하게 생각하지않아도되고, 메소드로 정의할 수 있으므로 가독성을 높일 수 있음. 또한 역할 위임이 이뤄지기때문에 코드 관리도 편함
  
```
// Coordinate.java

private List<Line> points = new ArrayList<>();  

public boolean isDrawPosition(int position) {  
    Point searchPoint = points.get(position);  
    return searchPoint.isDrawPosition();  
}  

public boolean isMaxPosition(int position) {  
    int lastPosition = points.size() - 1;  
    return position == lastPosition;  
}


// Point.java

private int position;
private boolean isDraw;  

public boolean isDrawPosition() {  
    return isDraw;  
}  
```
  
## 상태값에 대한 get메소드로 상태값을 받아와서 가공하려하지말고 일하게 만들자
  
```
List<Car> cars = getCars();
List<Car> arrivedCar = new ArrayList<>();
```
* 위의 코드로 짜면 다른 곳(객체)에서 상태값을 그대로 받아와서 다른 곳에서 판단하는 꼴  
* 아래 코드처럼 바꿀 수 있음 : 판단은 거기서, 만드는건 만드는 역할을 맡은 곳에서
  
```
// Controller.java
List<Car> arrivedCars = cars.getArrivedCars(10);


// Cars
List<Car> cars = new ArrayList<>();

public List<Car> getArrivedCars(int arrivePosition) {
	
}

public boolean isArrived(int arrivePosition) {
    
}
```
  
## 가장 중요한건....
* 자바는 객체지향 프로그래밍이다.