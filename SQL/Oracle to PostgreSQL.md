# Oracle to PostgreSQL


## 1. `DUAL`
- Oracle에서 사용하는 `DUAL`은 PostgreSQL에서는 제외
```
SELECT 1 FROM DUAL => SELECT 1
```

## 2. `SYSDATE`
- `NOW()` 함수나 `CURRENT_TIMESTAMP`로 대체
```
TO_CHAR(SYSDATE, 'YYYYMMDD') => TO_CHAR(CURRENT_TIMESTAMP, 'YYYYMMDD') 
```

## 3. `NVL`
- `COALESCE` 함수로 대체
```
NVL(USER_ID, 0) => COALESCE(USER_ID, 0)
```

## 4. `SEQUENCE`
- Oracle `SEQUENCE` 문법은 `SEQUENCE명.NEXTVAL`
- PostgreSQL `SEQUENCE` 문법은 `NEXTVAL(SEQUENCE명)`
```
SEQ_USER_ID.NEXTVAL() => NEXTVAL(SEQ_USER_ID)
```

## 5. `ROWNUM`
- Oracle에서 사용하는 `ROWNUM`을 PostgreSQL에서 사용하려면 `ROW_NUMBER() OVER()` 함수 사용
```
SELECT ROWNUM, A.* FROM USER A  => SELECT ROW_NUMBER() OVER() AS RNUM, A.* FROM USER AS A
```

## 6. `DECODE`
- PostgreSQL에서는 `DECODE` 함수를 제공하지 않음
- `CASE` 문을 사용하여 전환
```
DECODE(USER_ID, NULL, 0, USER_ID)

|
V

CASE
WHEN USER_ID IS NOT DISTINCT FROM NULL
THEN 0
ELSE USER_ID
END
```

## 7. 데이터 형 변환
- Column 또는 값에 `::[DataType]`을 붙여서 변환
- `CAST` 함수를 사용하여 형 변환도 가능
```
SELECT '1'::int
SELECT CAST('1' AS INTEGER)
```

## 8. `OUTER JOIN`
- PostgreSQL에서는 ANSI SQL 표준을 사용
- Oracle에서도 정상 동작
```
SELECT D.DNAME, E.EMP_NO
FROM DEPT D, EMP E
WHERE D.DEPT_NO = E.DEPT_NO (+)

|
V

SELECT D.DNAME, E.EMP_NO
FROM DEPT D LEFT OUTER JOIN EMP E
ON D.DEPT_NO = E.DEPT_NO
```

## 9. `MERGE INTO`
- Query를 분리하여 처리
- 변경 방법
  - 조건이 맞는지(`MATCHED`)에 대한 `SELECT`문을 실행시켜서 결과값이 있으면 `UPDATE`문을 실행시켜 주고 결과값이 없으면 `INSERT`문을 실행
  - `UPDATE`문을 실행해서 `UPDATE`가 일어나면 그대로 가고 `UPDATE`가 일어나지 않으면 `INSERT`문을 실행 

## 10. `START WITH`문과 `CONNECT BY`문
- Oracle의 `START WITH`문과 `CONNECT BY`문은 PostgreSQL에서 `WITH`를 사용한 `RECURSIVE` Query로 변경