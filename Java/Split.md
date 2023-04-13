# Split


## split() 메서드
- 구분자(delimiter)를 기준으로 문자열을 잘라 배열로 입력할 때 사용하는 메서드
- 같은 이름의 메서드이지만 param 갯수에 따라 다른 동작을 함 (Overloading)


### split(String regex)
- 구분자를 기준으로 문자열을 잘라 배열 형식으로 반환


### split(String regex, int limit)
- 구분자를 기준으로 문자열을 잘라 limit 수 만큼의 원소를 갖는 배열 형식으로 반환


### limit의 값에 따라 결과가 다르다!
- 예제 코드
  - [Split.java](https://github.com/k1m743hyun/java-exercise/src/Split.java)


#### limit이 0인 경우
- length가 0인 String은 Ignore 처리됨


#### limit이 음수인 경우
- length가 0인 String도 포함됨


#### limit이 양수인 경우
- 구분자를 기준으로 문자열을 자른 배열의 최대 개수가 limit 값보다 더 크다면 limit 수 만큼의 원소를 갖는 배열이 출력됨
- 구분자를 기준으로 문자열을 자른 배열의 최대 개수보다 limit 값이 같거나 크다면 limit이 음수인 경우와 동일하게 동작함


### 정리
- split 메서드의 default limit 값은 "0"
  - split(String regex) == split(String regex, 0)
- split 메서드의 limit 값이 0이면
- split 메서드의 limit 값이 0보다 크면(양수)
- split 메서드의 limit 값이 0보다 작으면(음수) 구분자를 기준으로 나눠진 모든 문자열(공백 포함)이 반환


# 출처
- [[JAVA] 문자열을 배열로 자르는 메서드 split에 대해서 알아봅시다](https://crazykim2.tistory.com/549)
- [[Java] split() 메서드의 limit 이용 공백유지](https://baejangho.com/entry/java-split-limit)
- [Class String](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html)