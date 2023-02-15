# sqlacodegen


## sqlacodegen 이란?
- 존재하는 DB 테이블을 읽어서 Model로 만들어주는 Library


## sqlacodegen 사용법


### sqlacodegen 설치
```
$ pip install sqlacodegen
```

### sqlacodegen 실행
```
$ sqlacodegen mysql+pymysql://{USER}:{PASSWORD}@{HOST}:{PORT}/{DB_NAME} > model.py
```

# 출처
- [[Sqlacodegen] 기존에 만들어진 DB를 모델로 만들기](https://velog.io/@lsh9672/Sqlacodegen-%EA%B8%B0%EC%A1%B4%EC%97%90-%EB%A7%8C%EB%93%A4%EC%96%B4%EC%A7%84-DB%EB%A5%BC-%EB%AA%A8%EB%8D%B8%EB%A1%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0)