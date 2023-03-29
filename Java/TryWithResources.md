# Try with Resources


## Before Try with Resources
- Try with Resources 기능이 생기기 전(Java 7 이전)에는 try-catch-finally 구문을 사용하여 리소스 반납 처리를 해야했음
- [TryWithResources.java](https://github.com/k1m743hyun/java-exercise/commit/fc02995c0b8030a14afa35080605f25974568bda)
- 리소스 생성은 try 문에서, 반납은 finally 문에서 하다보니 생성은 했지만 반납을 빼먹는 경우가 발생함


## Birth of Try with Resources


### 단일 리소스
- Java 7 이후에는 Try with Resources 기능이 탄생하여 리소스를 생성하고 자동으로 반납함
- [TryWithResources.java](https://github.com/k1m743hyun/java-exercise/commit/c867586851f706749016a9540640a48cf17f1533)
- try 옆에 괄호 안에서 리소스를 생성함
- 리소스 반납하는 명령어가 없어도 자동으로 리소스가 반납됨 
- Try with Resources 기능을 사용하려면 반드시 `java.lang.AutoCloseable` 인터페이스를 구현해야 함


### 멀티 리소스
- 여러 개의 리소스를 생성하고 반납할 수도 있음
- [TryWithResources.java](https://github.com/k1m743hyun/java-exercise/commit/6f9af9f2f1ec78d01c886d71a2238fba1d90f5d3)
- try 옆 괄호 안에서 리소스를 생성할 때 `;`로 구분하여 여러 리소스를 생성할 수 있음


## Use Try with Resources with Custom Resource


### CustomResource 생성
- AutoCloseable 인터페이스를 구현하여 리소스 클래스를 직접 생성할 수 있음
- [CustomResource.java](https://github.com/k1m743hyun/java-exercise/commit/55c805da583f3abf57d768054e22020a37b95da7)
    - AutoCloseable 인터페이스에 정의된 close() 메소드를 Override하여 구현
- [TryWithResources.java](https://github.com/k1m743hyun/java-exercise/commit/3e2ed880d9cbb52533c93b2977ca756e968d0dcf)
    - 직접 만든 CustomResource 클래스를 생성
    - close()를 따로 호출하지 않았지만 자동 호출되는 것을 확인할 수 있음


### 리소스 반납 순서
- 여러 개의 리소스가 같이 선언되었을 때 가장 나중에 선언된 리소스부터 역순으로 반납 처리됨
- [CustomResource.java](https://github.com/k1m743hyun/java-exercise/commit/44d69460efcae7a293b5d9d7fd3a560bc3a58678)
- [NewCustomResource.java](https://github.com/k1m743hyun/java-exercise/commit/f54aab6eabaffc8c6fac292dcb52d6ef0a8e0edd)
- [TryWithResources.java](https://github.com/k1m743hyun/java-exercise/commit/f3cd7ec3111507fcf96e38c72daa80f45dff1b26)
    - CustomResource, NewCustomResource 순서대로 생성
    - 반납은 NewCustomResource, CustomResource 순으로 처리됨


# 출처
- [[Java] Try with resources 로 자원 반납하기](https://hianna.tistory.com/546)