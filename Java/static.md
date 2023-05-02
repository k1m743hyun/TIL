# static


## static 이란?
- 메모리에 한 번 할당되어 프로그램이 종료될 때 해제되는 것


### 참고) Java의 메모리 관리
- Class는 Static 영역에 생성
- new를 통해 생성한 객체는 Heap 영역에 생성
- Heap 영역의 메모리는 Garbage Collector를 통해 수시로 관리 받음
- Static 영역에 할당된 데이터는 모든 객체가 공유하는 장점이 있지만, Garbage Collector의 관리 영역 밖에 존재하므로 Static을 자주 사용하게 되면 프로그램이 종료될 때까지 할당된 채로 존재하게 되어 시스템 퍼포먼스에 영향을 주게 됨


## static 변수
- 메모리에 한 번 할당되어, 프로그램이 종료될 때 해제되는 변수
- 클래스 변수
- 여러 객체에 공유 가능
- 객체를 생성하지 않고 접근이 가능


### 예제
- [Static.java](https://github.com/k1m743hyun/java-exercise/blob/main/src/Static.java)
- static을 선언하지 않으면 객체를 생성할 때마다 객체 변수 name을 저장하기 위한 메모리가 별도로 할당됨
- static 키워드를 붙이면 Java는 메모리 할당을 딱 한 번만 하게 되어 메모리 사용에 이점이 있음
- static으로 설정하면 같은 곳의 메모리 주소를 


## static 메서드
- 


# 출처
- [[Java] 자바 static의 의미와 사용법](https://coding-factory.tistory.com/524)
- [07-03 스태틱(static)](https://wikidocs.net/228)
- [[java/자바] Static 이란? Static 정리](https://mi-nya.tistory.com/251)