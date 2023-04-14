# Java Reflection


## Java Reflection이란?
- JVM 힙 메모리에 ClassLoader를 통해서 Class 정보가 저장이 됨
  - 자세한 내용은 [JVM ClassLoader](./JVM\ ClassLoader.md) 참고
- JVM에 저장된 Class 정보가 마치 거울에 투영된 모습과 같음
- Reflection을 사용하면 접근 제어자와 무관하게 클래스에 대한 정보를 자세하게 알 수 있고 직접 호출도 할 수 있음
- Reflection의 대표적인 예시는 프레임워크에서 사용되는 어노테이션임
- Reflection을 사용하기에 클래스나 메소드에 어떤 어노테이션이 붙어있는지 확인할 수 있음
- 덕분에 Spring 어노테이션을 선언함으로 Spring의 여러 기능을 쉽게 사용할 수 있게 됨


### `Class` 클래스
- Reflection의 핵심
- `Class` 클래스에는 public 생성자가 없으므로 직접 `Class` 객체를 생성할 수 없고, JVM이 자동으로 `Class` 객체를 생성해줌


#### Class 객체 획득 방법
- 1) 클래스의 `class` 프로퍼티를 통해 획득
- 2) 인스턴스의 `getClass()` 메서드를 사용하여 획득
- 3) `Class` 클래스의 `forName()` 정적 메서드에 *FQCN(Fully Qualified Class Name)*을 전달하여 `Class` 클래스의 인스턴스 획득


#### `getXXX()` vs `getDeclaredXXX()`
- `Class` 객체 메서드 중에서 클래스에 정의된 필드, 메서드, 어노테이션 목록을 가져오기 위해 사용되는 메서드
  - `getXXX()` 형식의 메서드
    - 상속 받은 클래스와 인터페이스를 포함하여 모든 public 요소를 가져옴
    - `getFields()`, `getMethods()`, `getAnnotations()`
  - `getDeclaredXXX()` 형식의 메서드
    - 상속 받은 클래스와 인터페이스를 제외한 접근 제어자 상관 없이 직접 정의된 요소만 가져옴
    - `getDeclaredFields()`, `getDeclaredMethods()`, `getDeclaredAnnotations()`


### Constructor 가져오기
- `getDeclaredConstructor()` 메서드를 사용하여 `Constructor` 타입으로 획득
- 생성자에 파라미터가 있다면 `getDeclaredConstructor()` 메서드에 생성자 파라미터 타입을 전달하면 됨
- `newInstance()` 메서드를 사용하여 객체를 직접 생성할 수 있음
- 생성자 파라미터가 있다면 `newInstance()` 메서드에 생성자 사용하듯이 전달하면 됨
- 타입 캐스팅을 하지 않으면 `Object` 타입으로 인스턴스가 반환됨
- 참고) `newInstance()` 메서드는 deprecated 되었음
- private 접근 제어자가 붙은 생성자를 사용하려면 `setAccessible(true)`를 사용하여야 접근이 가능함
  - `setAccessible(true)` 없이 사용하면 `java.lang.IllegalAccessException` 예외가 발생함


### Field 가져오기
- `getDeclaredField()` 메서드에 필드 명을 전달하면 해당 필드를 `Field` 타입으로 획득
- `set()` 메서드를 사용하여 `Setter` 없이도 강제로 필드 값을 변경할 수 있음
- 모든 필드를 획득하려면 `getDeclaredFields()`를 사용


### Method 가져오기
- `getDeclaredMethod()` 메서드에 메서드 명을 전달하면 해당 메서드를 `Method` 타입으로 획득
- 접근 제어자 상관 없이 메서드에 직접 접근할 수 있음
- `invoke()`를 사용하여 획득한 메서드 직접 호출 가능


### Annotation 가져오기
- `getAnnotation()` 메서드에 직접 어노테이션 타입을 넣어줌
- 어노테이션이 가지고 있는 필드에도 접근할 수 있음


### Reflection 단점
- Reflection은 Runtime에 클래스를 분석하므로 속도가 느림
  - 일반적으로는 컴파일 시 클래스를 분석함
- 타입 체크가 컴파일 시 불가능
- 객체의 추상화가 깨짐


# 출처
- [자바 리플렉션 (Reflection) 기초](https://hudi.blog/java-reflection/)
- [[Java] Reflection 개념 및 사용 방법](https://steady-coding.tistory.com/609)