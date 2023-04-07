# Difference between == and equals


## `==` 연산자
- primitive type에 대해서는 값을 비교(Call by Value)
- reference type에 대해서는 메모리 주소 값을 비교(Call by Reference)


## `equals()` 메서드
- 최상위 클래스인 Object에 포함되어 있기 때문에 모든 하위 클래스에서 재정의해서 사용할 수 있음
- String 클래스에서 문자열 내용이 같으면 `true`를 반환


## 예제
- [DifferenceBetweenCallByValueAndCallByReference.java](https://github.com/k1m743hyun/java-exercise/tree/main/src/DifferenceBetweenCallByValueAndCallByReference.java)


## 정리
> `==` 연산자는 주소값을 비교(Call by Reference)할 때 사용하는 연산자이고, equals() 메서드는 내용(값) 자체를 비교(Call by Value)할 때 사용하는 메서드이다.


# 출처
- [[Java]==와 equals()의 차이](https://velog.io/@dab2in/Javaequals)