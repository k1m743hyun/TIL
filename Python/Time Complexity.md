# 시간 복잡도 (Time Complexity)


## 시간 복잡도란?
> 입력 값의 변화에 따라 연산을 실행할 때, 연산 횟수에 비해 시간이 얼만큼 걸리는가?
- 효율적인 알고리즘이란, *입력 값이 커짐에 따라 증가하는 시간의 비율을 최소화한 알고리즘*을 뜻함


### 시간 복잡도를 표기하는 세가지 방법


#### 빅-오(Big-O)
- **상한 점근**
- *최악*의 경우를 고려함


#### 빅-오메가(Big-Ω)
- **하한 점근**
- *최선*의 경우를 고려함


#### 빅-세타(Big-θ)
- **그 둘의 평균**
- *중간(평균)*의 경우를 고려함


하지만 *최악*의 경우를 고려하여 대비하는 것이 바람직하기 때문에, 다른 표기법보다 **빅-오 표기법**을 많이 사용함


## Python 자료구조의 시간 복잡도


### List
|연산자|설명|예시|시간 복잡도|비고|
|---|---|---|---|---|
|Index|n번째 element 접근|l[i]|O(1)||
|Store|n번째 element 할당|l[i] = 0|O(1)||
|Length|List 길이|len(l)|O(1)||
|Append|List 맨뒤 element 추가|l.append(5)|O(1)||
|Pop|List 맨뒤 element 삭제|l.pop()|O(1)|l.pop(-1)과 같음|
|Clear|List 전체 element 삭제|l.clear()|O(1)|l = list(), l = []과 비슷함|
|Slice|List 일부 element 접근|l[a:b]|O(b-a)|l[:]:O(len(l)-0)=O(N)|
|Extend|List 맨뒤 다른 List 연결|l.extend(...)|O(len(...))|확장할 요소(...)의 길이에만 의존|
|Construction|List 객체 생성|list(...)|O(len(...))|요소(...)의 길이 의존, iterable 객체|
|check ==, !=|두 List 일치 확인|l1 == l2|O(N)||
|Insert|element n번째 삽입|l[a:b] = ... |O(N)||
|Delete|element n번째 삭제|del l[i]|O(N)|i에 의존, 최악의 경우 O(N)|
|Containment|List 내 element 존재 여부 확인|x (not) in l|O(N)|선형 검색|
|Copy|List 복사|l.copy()|O(N)|l[:]과 같음|
|Remove|element 값 삭제|l.remove(...)|O(N)||
|Pop|n번째 element 삭제|l.pop(i)|O(N)|O(N-i):l.pop(0)=O(N)|
|Extreme value|min / max 값 탐색|min(l)/max(l)|O(N)|특정 값을 찾기까지 선형 검색|
|Reverse|List 역순 정렬|l.reverse()|O(N)||
|Iteration|List 전체 순회|for v in l:|O(N)|최악의 경우: return과 break 없는 반복문|
|Sort|List 정렬|l.sort()|O(N Log N)|reverse=True도 대부분 다르지 않다|
|Multiply|element가 k번 있는 List 생성|k*l|O(k N)|5*l=O(N), len(l)*l=O(N**2)|


### Tuple
- 튜플은 데이터를 변경하는 Operation을 제외한 모든 List의 Operation을 사용할 수 있음.


### Set
|연산자|설명|예시|시간 복잡도|비고|
|---|---|---|---|---|
|Length||len(s)|O(1)||
|Add||s.add(5)|O(1)||
|Containment||x (not) in s|O(1)|compare to list/tuple - O(N)|


### Dictionary
|연산자|설명|예시|시간 복잡도|비고|
|---|---|---|---|---|


# 출처
- [[알고리즘] Time Complexity (시간 복잡도)](https://hanamon.kr/알고리즘-time-complexity-시간-복잡도/)
- [파이썬 자료형 별 주요 연산자의 시간 복잡도 (Big-O)](https://wayhome25.github.io/python/2017/06/14/time-complexity/)
- [Complexity of Python Operations](https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt)
- [파이썬 기본 연산자들의 시간 복잡도(Big-O) 정리](https://dev.plusblog.co.kr/42)