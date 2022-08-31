# SQL Injection


## SQL Injection이란?
- 악의적인 사용자가 보안 상의 취약점을 이용하여, 임의의 SQL문을 주입(Injection)하고 실행되게 하여 데이터베이스가 비정상적인 동작을 하도록 조작하는 행위
- OWASP Top 10 중 첫 번째에 속해 있으며, 공격이 비교적 쉬운 편이고 공격에 성공할 경우 큰 피해를 입힐 수 있는 공격


## 공격 기법


### 1) Error based SQL Injection
- 논리적 Error를 이용한 SQL Injection
- 가장 많이 쓰이고 대중적인 공격 기법
- 정상적인 로그인 Query
    ```
    SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2';
    ```
- SQL Injection 시도 Query
    ```
    SELECT * FROM Users WHERE id='' OR 1 = 1 -- ' AND password = 'INPUT2';
    ```
    - Injection된 Query는 `' OR 1 = 1 --`로 `WHERE`절에 있는 싱글 쿼터를 닫아주기 위한 싱글 쿼터와 `OR 1 = 1`라는 구문을 이용해 `WHERE`절을 모주 참으로 만들고, `--`를 넣어줌으로 뒤의 구문을 모두 주석처리해준다.
    - 결론적으로 Users 테이블에 있는 모든 정보를 조회하게 됨으로써 가장 먼저 만들어진 계정으로 로그인하게 된다.
        - 보통 관리자 계정을 맨 처음 만들기 때문에 관리자 계정으로 로그인할 수 있게 된다.
  

### 2) Union based SQL Injection
- Union 명령어를 이용한 SQL Injection
- `Union`은 두 개의 Query문에 대한 결과를 통합해서 하나의 테이블로 보여주게 하는 SQL 집합 연산자
- 정상적인 Query문에 `Union` 키워드를 사용하여 Injection에 성공하면, 원하는 Query문을 실행할 수 있게 된다.
- Union Injection을 성공하기 위해서는 두 가지 조건이 있다.
    - Union하는 두 테이블의 Column 수가 동일하다.
    - 두 테이블의 Data Type이 같다.
-  정상적인 Query
    ```
    SELECT * FROM Board WHERE title LIKE '%INPUT1%' OR contents LIKE '%INPUT2%';
    ```
- SQL Injection 시도 Query
    ```
    SELECT * FROM Board WHERE title LIKE '%' UNION SELECT null, id, password FROM Users -- %' OR contents LIKE '%INPUT2%';
    ```
    - Injection된 Query는 `' UNION SELECT null, id, password FROM Users --`로 사용자의 id와 password를 요청하는 Query문
        - `UNION` 키워드와 함께 Column 수를 맞춰서 `SELECT` 구문을 넣어주게 되면 두 Query문이 합쳐져서 하나의 테이블로 보여지게 된다.
    - Injection이 성공하게 되면, 사용자의 개인정보가 게시글과 함께 화면에 보여지게 된다.


### 3) Blind SQL Injection
- 데이터베이스로부터 특정한 값이나 데이터로 응답받지 않고, 대신에 단순히 참 혹은 거짓의 응답을 통해서 데이터베이스의 정보를 유추하는 기법


#### [1] Boolean based SQL Injection
- 로그인 Form에 SQL Injection이 가능하다고 가정했을 때, 서버가 응답하는 로그인 성공과 실패 메세지를 이용하여, DB 테이블 정보 등을 추출해낼 수 있다.
-  정상적인 Query
    ```
    SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2';
    ```
- SQL Injection 시도 Query
    ```
    SELECT * FROM Users WHERE id = 'admin' and ASCII(SUBSTR(SELECT name FROM information_schema.tables WHERE table_type = 'base table' LIMIT 0, 1), 1, 1) > 100 -- ' AND password = 'INPUT2';
    ```
    - 해당 구문은 MySQL에서 테이블 명을 조회하는 구문으로 LIMIT 키워드를 통해 하나의 테이블만 조회하고, SUBSTR 함수로 첫 글자만, 그리고 마지막으로 ASCII를 통해서 ascii 값으로 변환해준다.
        - 조회되는 테이블 명이 `Users`라면 `'U'`가 ascii 값으로 조회가 될 것이고, 뒤의 100이라는 숫자 값과 비교하게 된다.
        - 거짓이면 로그인 실패가 될 것이고, 참이 될 때까지 뒤의 100이라는 숫자를 변경해 가면서 테이블 명을 알아낼 수 있다.
        - 공격자는 위 과정을 자동화 스크립트를 통하여 단시간 내에 테이블 명을 알아낼 수 있다.


#### [2] Time based SQL Injection
- MySQL 기준으로 SLEEP과 BENCHMARK 함수를 이용하여 데이터베이스의 정보를 유추할 수 있다.
- 정상적인 Query
    ```
    SELECT * FROM Users WHERE id = 'INPUT1' AND password = 'INPUT2';
    ```
- SQL Injection 시도 Query
    ```
    SELECT * FROM Users WHERE id = 'admin' OR (LENGTH(DATABASE()) = 1 AND SLEEP(2)) -- ' AND password = 'INPUT2';
    ```
    - 현재 사용하고 있는 데이터베이스의 길이를 알아내는 방법
        - 주입된 구문에서 LENGTH(DATABASE()) = 1이 참이면 SLEEP(2)가 동작하고, 거짓이면 동작하지 않는다.
        - 이를 통해서 숫자 1 부분을 조작하여 데이터베이스의 길이를 알아낼 수 있다.


### 4) Stored Procedure SQL Injection
- 저장된 Procedure에서의 SQL Injection
- 공격에 사용되는 대표적인 저장 Procedure는 MS-SQL에 있는 xp_cmdshell로 윈도우 명령어를 사용할 수 있게 됩니다.
- 단, 공격자가 시스템 권한을 획득해야 하므로 공격 난이도가 높으나 공격에 성공한다면 서버에 직접적인 피해를 입힐 수 있는 공격이다.


### 5) Mass SQL Injection
- 다량의 SQL Injection 공격
- 한 번의 공격으로 다량의 데이터베이스가 조작되어 큰 피해를 입히는 것을 의미
- 보통 MS-SQL을 사용하는 ASP 기반 웹 애플리케이션에서 많이 사용되며, Query문은 HEX 인코딩 방식으로 인코딩하여 공격한다.


# 참고문헌
- [SQL Injection 이란? (SQL 삽입 공격)](https://noirstar.tistory.com/264)