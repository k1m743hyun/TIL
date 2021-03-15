# 도커 이미지 추출(Docker Image Export)
&nbsp;
## 1. 방법
&nbsp;
### (1) `save` and `load`
&nbsp;
#### [1] `save`
&nbsp;
- container를 tar파일로 저장한다.
&nbsp;
```bash
$ docker export (컨테이너명 or 컨테이너 ID) >> (컨테이너).tar
```
&nbsp;
#### [2] `load`
&nbsp;
- tar 파일을 다시 docker 이미지로 생성한다. export의 반대이다.
&nbsp;
```bash
$ docker import (파일 또는 URL)
```
&nbsp;
&nbsp;
### (2) `export` and `import`
&nbsp;
#### [1] `export`
&nbsp;
- docker 이미지를 tar 파일로 저장한다.
&nbsp;
```bash
$ docker save -o rabbitmq_managment.tar rabbitmq:managment
```
&nbsp;
#### [2] `import`
&nbsp;
- tar파일 이미지를 docker 이미지로 저장한다.
&nbsp;
```bash
$ docker load -i rabbitmq_managment.tar
```
&nbsp;
# 참고 자료
- [시작하세요! 도커/쿠버네티스 - YES24](http://www.yes24.com/Product/Goods/84927385)


- [도커 export 및 import 커맨드와 save 및 load 커맨드간의 차이](https://knight76.tistory.com/entry/도커-export-및-import-커맨드와-save-및-load-커맨드간의-차이)