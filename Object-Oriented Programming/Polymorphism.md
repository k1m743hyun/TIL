# Polymorphism


## 다형성(Polymorphism)이란?
- 하나의 객체에 여러가지 타입을 대입할 수 있음


## 다형성을 구현하는 방법
- 1) 오버로딩(Overloading)
- 2) 오버라이딩(Overriding)
- 3) 함수형 인터페이스


### 오버로딩(Overloading)
- 아래 규칙을 지킨다면 동일 메서드 명을 가진 여러 메서드를 정의할 수 있음
  - 메서드 이름이 같아야 함
  - 매개변수의 개수 또는 타입이 달라야 함
  - 매개변수는 같고 리턴 타입이 다를 때는 성립하지 않음
  - 오버로딩된 메서드들은 매개변수로만 구분될 수 있음
- [Overloading.java](https://github.com/k1m743hyun/java-exercise/blob/main/src/Overloading.java)


### 오버라이딩(Overriding)
- 상위 클래스의 메서드를 재정의하여 사용하는 것
- 인터페이스 상속이나 클래스 상속 시 사용할 수 있음
- [Overriding.java](https://github.com/k1m743hyun/java-exercise/blob/main/src/Overriding.java)


### 함수형 인터페이스
- 한 개의 추상 메서드를 가지고 있는 인터페이스


# 출처
- [[OOP] 다형성(Polymorphism)이란?](https://steady-coding.tistory.com/446)