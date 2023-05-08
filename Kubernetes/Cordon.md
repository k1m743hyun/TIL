# Cordon


## Cordon 이란?
- 특정 Node를 스케줄러에서 제외시켜 Pod가 할당되지 않도록 함
- 기존 Node에 배포된 Pod는 그대로 남아 있음


## 사용 방법
- Cordon 적용
```
$ kubectl cordon {NodeName}
```

- Cordon 해제
```
$ kubectl uncordon {NodeName}
```


# 출처
- [쿠버네티스 커든(Cordon) 및 드레인(Drain) 개념과 설정](https://velog.io/@_zero_/%EC%BF%A0%EB%B2%84%EB%84%A4%ED%8B%B0%EC%8A%A4-%EC%BB%A4%EB%93%A0Cordon-%EB%B0%8F-%EB%93%9C%EB%A0%88%EC%9D%B8Drain-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%84%A4%EC%A0%95)