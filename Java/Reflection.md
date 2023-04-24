# Java Reflection


## Java Reflection이란?
- 구체적인 Class 타입을 알지 못 하더라도 해당 Class의 Method, Type, Variable들에 접근할 수 있도록 해줌
- 컴파일된 Byte 코드를 통해 Runtime에 동적으로 특정 Class의 정보를 추출할 수 있는 프로그래밍 기법
- JVM 힙 메모리에 저장된 `Class` 타입 객체가 마치 거울에 투영된 모습과 같음
  - 자세한 내용은 [JVM ClassLoader](https://github.com/k1m743hyun/TIL/blob/main/Java/JVM%20ClassLoader.md) 참고
- Reflection을 사용하면 접근 제어자와 무관하게 인스턴스 필드와 클래스를 직접 호출할 수 있음


## 정적 바인딩 vs 동적 바인딩
- 바인딩이란, 프로그램에 사용된 구성 요소의 실제 값 또는 프로퍼티를 결정짓는 행위를 뜻함
  - 프로그램에서 사용되는 변수나 메서드 등 모든 것들이 결정되도록 연결해주는 것을 뜻함
- 바인딩을 결정 짓는 시점에 따라 *정적 바인딩*과 *동적 바인딩*으로 나뉨
- *동적 바인딩*은 런타임에 바인딩이 되다보니 *정적 바인딩*보다는 성능 상 오버헤드가 있지만, *동적 바인딩*을 통해 상속과 다형성 등 다양한 기능을 사용할 수 있는 장점이 있음


### 정적 바인딩 (Static Binding)
- 컴파일 시간(Compiletime)에 결정
- 프로그램 실행 중에 변하지 않음
- 오버로딩(Overloading)
  - 컴파일 과정에서 어떤 메서드를 호출할 지 결정하기 때문에 코드 작성 단계에서 어떤 메서드를 사용할 지 구분할 수 있고, 잘 못 사용했을 경우 컴파일 에러를 발생시킴
- private, final, static이 붙은 메서드


### 동적 바인딩 (Dynamic Binding)
- 실행 시간(Runtime)에 결정
- 늦은 바인딩(Late Binding)
- 오버라이딩(Overriding)
  - 상속을 이용한 부모 - 자식 관계일 경우 부모에서 정의한 메서드를 자식이 오버라이딩했다고 가정했을 때, 해당 메서드를 이용할 경우 런타임에서 어떤 메서드를 호출할 지가 정해짐
- 다형성, 상속이 가능한 이유


## Reflection 사용 이유
- **동적으로 클래스를 사용해야할 경우**
  - 코드 작성 시점에서는 어떤 클래스를 사용해야할 지 모르지만, 런타임에 클래스를 가져와서 실행해야 하는 경우
  - 예) Spring Annotation
- **Test Code 작성**
  - private 변수를 변경하고 싶거나 private 메서드를 테스트할 경우
- **자동 Mapping 기능 구현**
  - IDE 사용 시 일부만 입력해도 이와 관련된 클래스 혹은 메서드 목록들을 IDE가 먼저 확인하고 사용자에게 제공함
- **Jackson, GSON 등의 JSON Serialization Library**
- **정적 분석 Tool**


## Reflection 사용 방법
- Heap 영역의 Load된 `Class` 타입 객체를 먼저 가져와야 함


### `Class` 타입 객체 가져오는 방법
- 1) `클래스.class`
- 2) `인스턴스.getClass()`
- 3) `Class.forName("클래스 명")`


#### `getXXX()` vs `getDeclaredXXX()`
- `Class` 객체 메서드 중에서 클래스에 정의된 필드, 메서드, 어노테이션 목록을 가져오기 위해 사용되는 메서드
  - `getXXX()` 형식의 메서드
    - 상속 받은 클래스와 인터페이스를 포함하여 모든 public 요소를 가져옴
    - `getFields()`, `getMethods()`, `getAnnotations()`
  - `getDeclaredXXX()` 형식의 메서드
    - 상속 받은 클래스와 인터페이스를 제외한 접근 제어자 상관 없이 직접 정의된 요소만 가져옴
    - `getDeclaredFields()`, `getDeclaredMethods()`, `getDeclaredAnnotations()`


### 인스턴스 생성
- 생성자 가져오기
  - `getConstructor()` 메서드 사용
  - `getDeclaredConstructor()` 메서드 사용
  - 생성자에 파라미터가 있다면 메서드에 생성자 파라미터 타입을 전달
- `newInstance()` 메서드를 사용하여 인스턴스를 동적으로 생성할 수 있음
  - 생성자 파라미터가 있다면 `newInstance()` 메서드에 생성자 사용하듯이 전달하면 됨
  - 참고) `newInstance()` 메서드는 deprecated 되었음


### 인스턴스 필드 접근
- `getDeclaredField()` 메서드에 필드 명을 전달하면 해당 필드를 `Field` 타입으로 획득
- `set()` 메서드를 사용하여 `Setter` 없이도 강제로 필드 값을 변경할 수 있음
- 모든 필드를 획득하려면 `getDeclaredFields()`를 사용


### 인스턴스 메서드 접근
- `getDeclaredMethod()` 메서드에 메서드 명을 전달하면 해당 메서드를 `Method` 타입으로 획득
- 접근 제어자 상관 없이 메서드에 직접 접근할 수 있음
- `invoke()`를 사용하여 획득한 메서드 직접 호출 가능


## Reflection 장단점


### 단점
- Reflection은 Runtime에 클래스를 분석하므로 속도가 느림
  - 일반적으로는 컴파일 시 클래스를 분석함
- 타입 체크가 컴파일 시 불가능
- 객체의 추상화가 깨짐


# 출처
- [자바 리플렉션 (Reflection) 기초](https://hudi.blog/java-reflection/)
- [[Java] Reflection은 무엇이고 언제/어떻게 사용하는 것이 좋을까?](https://velog.io/@alsgus92/Java-Reflection%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%96%B8%EC%A0%9C%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B4-%EC%A2%8B%EC%9D%84%EA%B9%8C)
- [[Java] Reflection 개념 및 사용 방법](https://steady-coding.tistory.com/609)