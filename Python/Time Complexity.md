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
|Operation|Example|Big-O|Notes|
|---|---|---|---|
|Index|l[i]|O(1)||
|Store|l[i] = 0|O(1)||
|Length|len(l)|O(1)||
|Append|l.append(5)|O(1)||
|Pop|l.pop()|O(1)|l.pop(-1)과 같음|
|Clear|l.clear()|O(1)|l = []과 비슷함|
|Slice|l[a:b]|O(b-a)|l[:]:O(len(l)-0)=O(N)|
|Extend|l.extend(...)|O(len(...))|확장할 요소(...)의 길이에만 의존|
|Construction|list(...)|O(len(...))|요소(...)의 길이 의존, iterable 객체|
|check ==, !=|l1 == l2|O(N)||
|Insert|l[a:b] = ... |O(N)||
|Delete|del l[i]|O(N)|i에 의존, 최악의 경우 O(N)|
|Containment|x (not) in l|O(N)|선형 검색|
|Copy|l.copy()|O(N)|l[:]과 같음|
|Remove|l.remove(...)|O(N)||
|Pop|l.pop(i)|O(N)|O(N-i):l.pop(0)=O(N)|
|Extreme value|min(l)/max(l)|O(N)|특정 값을 찾기까지 선형 검색|
|Reverse|l.reverse()|O(N)||
|Iteration|for v in l:|O(N)|최악의 경우: return과 break 없는 반복문|
|Sort|l.sort()|O(N Log N)|reverse=True도 대부분 다르지 않다|
|Multiply|k*l|O(k N)|5*l=O(N), len(l)*l=O(N**2)|


### Tuple
- 튜플은 데이터를 변경하는 Operation을 제외한 모든 List의 Operation을 사용할 수 있음.


### Set

### Dictionary

# 출처
- [[알고리즘] Time Complexity (시간 복잡도)](https://hanamon.kr/알고리즘-time-complexity-시간-복잡도/)
- [파이썬 자료형 별 주요 연산자의 시간 복잡도 (Big-O)](https://wayhome25.github.io/python/2017/06/14/time-complexity/)
- [Complexity of Python Operations](https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt)