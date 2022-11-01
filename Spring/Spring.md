# Spring


## 1. Spring이란?


### (1) Spring의 개념
- Java 기반으로 웹 어플리케이션을 만들 수 있는 프레임워크
- Modern Java 기반의 Enterprise 어플리케이션을 위한 프로그래밍과 Configuration Model을 제공


### (2) Spring의 구조

![Spring Architect](./images/spring_architect.png)


### (3) Spring의 특징
- Spring은 Java 객체와 라이브러리들을 관리해주며, Tomcat과 같은 WAS가 내장되어 있어 Java 웹 어플리케이션을 구동할 수 있음
- Spring은 경량 컨테이너로 Java 객체를 직접 Spring 안에서 관리함
    - 객체의 생성 및 소멸과 같은 생명 주기(Life Cycle)을 관리하며, Spring 컨테이너에서 필요한 객체를 가져와 사용함
- Spring의 가장 큰 특징으로 IoC와 DI가 있음
    - 제어의 역전 (IoC, Inversion of Control)
        - 사용자가 객체를 직접 생성하지 않고, 객체의 생명 주기를 컨트롤하는 주체는 다른 주체에게 위임하는 것
    - 의존성 주입 (DI, Dependency Injection)
        - 객체를 외부에서 생성해서 사용하려는 주체 객체에 주입시켜주는 방식
    - Spring에서는 모든 의존성 객체를 Spring이 실행될 때 만들어주고 필요한 곳에 주입해줌
        - Spring에세 제어를 위임하여 Spring이 만든 객체를 주입 -> 의존성 객체의 메소드 호출


## 2. Spring Boot란?

### (1) Spring Boot의 개념
- Spring을 더 쉽게 이용하기 위한 도구
- Spring 프로젝트를 매우 쉽게 설정할 수 있게 하여, Spring 개발을 조금 더 쉽게 만들어주는 역할
- 비즈니스 로직을 만드는데 더 집중할 수 있도록 Spring 프로젝트 설정의 많은 부분을 자동화해줌


### (2) Spring Boot의 구조

![Spring Boot Architect](./images/spring_boot_architect.png)


# 출처
- [스프링(Spring), 스프링 부트(Spring Boot)란? 개념 정리](https://melonicedlatte.com/2021/07/11/174700.html)