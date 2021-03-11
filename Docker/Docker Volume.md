# 도커 볼륨(Docker Volume)

- 도커 이미지로 컨테이너를 생성하면 이미지는 읽기 전용이 되고, 컨테이너에서 변경된 부분만 별도로 저장해서 컨테이너 내부에 정보를 저장합니다.


- 도커 컨테이너가 삭제 또는 재시작이 되면 컨테이너 내부에 저장된 데이터가 함께 삭제됩니다.


- 컨테이너 내부의 데이터를 영속적(Persistent)으로 보존하기 위해 다음과 같은 방법을 사용합니다.
&nbsp;
&nbsp;
## 1. 호스트 볼륨 공유
- 컨테이너를 생성할 때 `-v` 옵션을 사용하여 호스트의 디렉토리와 컨테이너 디렉토리를 공유합니다.
&nbsp;
```bash
$ docker run -d --name wordpressdb_hostvolume -v /home/wordpress_db:/var/lib/mysql mysql:5.7
```
&nbsp;

- `-v` 옵션으로 공유된 디렉터리는 컨테이너가 삭제되더라도 호스트의 디렉터리는 삭제되지 않기 때문에 데이터를 보전할 수 있습니다.


- 디렉터리 단위의 공유뿐만 아니라 단일 파일 단위의 공유도 가능하며, 동시에 여러 개의 `-v` 옵션을 사용하여 동시에 여러 볼륨 공유도 가능합니다.


- 마운트하려는 볼륨이 호스트에 존재하지 않으면, 호스트에 해당 디렉터리가 생성되면서 컨테이너의 파일이 호스트로 복사됩니다.


- 호스트에 이미 디렉터리가 존재하고 컨테이너에도 이미 존재할 때, 두 디렉터리를 공유하게 되면 호스트 디렉터리가 컨테이너 디렉터리를 덮어쓰게 됩니다.
&nbsp;
&nbsp;
## 2. 컨테이너 볼륨 공유
- 컨테이너를 생성할 때 `--volumes-from` 옵션을 설정하면, 직접 볼륨을 공유하는 것이 아니라 `-v` 또는 `--volume` 옵션을 적용한 컨테이너를 통해서 볼륨 디렉터리가 공유됩니다.


- 볼륨 컨테이너로 사용할 컨테이너에 호스트 볼륨을 공유합니다.
&nbsp;
```bash
$ docker run --name volumes_container -v /tmp/sharetest:/var/sharetest sqlmvp/get-started:part2
```
&nbsp;

- 볼륨 컨테이너에 연결할 컨테이너를 실행합니다.
&nbsp;
```bash
$ docker run --name volumes_share1 --volumes-from volumes_container sqlmvp/get-started:part2
```
&nbsp;

- 여러 개의 컨테이너가 동일한 컨테이너에 `--volumes-from` 옵션을 사용한으로써 볼륨을 공유해 사용할 수도 있습니다.
&nbsp;
```bash
$ docker run --name volumes_share2 --volumes-from volumes_container sqlmvp/get-started:part2
```
&nbsp;

- 이러한 구조를 활용하면 호스트에서 볼륨만 공유하고 별도의 역할을 담당하지 않는 일명 **볼륨 컨테이너**로서 활용하는 것도 가능합니다.


- 다시 말하자면, 볼륨을 사용하려는 컨테이너에 `-v` 옵션 대신에 `--volumes-from` 옵션을 사용함으로써 볼륨 컨테이너에 연결해 데이터를 간접적으로 공유받는 방식입니다.
&nbsp;
&nbsp;
## 3. 도커 볼륨
- 도커 자체에서 제공하는 볼륨 기능을 활용해 데이터를 보존할 수 있습니다.


- 도커 볼륨을 생성하는 명령어는 `docker volume create`를 사용합니다.
&nbsp;
```bash
$ docker volume create -name myVolume
```
&nbsp;

-  생성된 볼륨을 확인하는 방법은 `docker volume ls`입니다.
&nbsp;
```bash
$ docker volume ls
```
&nbsp;

- 도커 볼륨이 실제 호스트의 어느 경로에 만들어졌는지 확인하는 방법은 `docker volume inspect` 명령을 사용합니다.
&nbsp;
```bash
$ docker volume inspect myVol
```
&nbsp;

- 도커 볼륨을 삭제하는 명령어는 `docker volume rm`입니다.
&nbsp;
```bash
$ docker volume rm myVol
```
&nbsp;

- 사용되지 않는 모든 볼륨을 삭제하려면 아래 명령을 실행합니다.
&nbsp;
```bash
$ docker volume prune
```
&nbsp;

- 도커 볼륨을 사용하는 컨테이너를 생성하기 위해서는 `[볼륨이름]:[컨테이너 공유 디렉토리]`순으로 명령을 입력합니다.
&nbsp;
```bash
$ docker run -d --name devtest -v myVol:/app nginx:latest
```
&nbsp;

- 여러 컨테이너가 있고 여러 볼륨이 있을 때 해당 컨테이너가 어떤 볼륨을 사용하는지 확인하는 명령은 `docker container inspect`를 사용합니다.
&nbsp;
```bash
$ docker run -d --name devtest -v myVol:/app nginx:latest
```
&nbsp;

- 컨테이너가 아닌 외부에 데이터를 저장하고 컨테이너는 그 데이터로 동작하도록 설계하는 것을 스테이트리스(stateless)라고 하며, 컨테이너 내부에 데이터를 저장하고 상태가 있는 경우 스테이트풀(stateful)하다고 합니다.

&nbsp;
# 참고 자료
- [시작하세요! 도커/쿠버네티스 - YES24](http://www.yes24.com/Product/Goods/84927385)


- [Docker Volume (호스트 볼륨 공유) -	컨테이너 데이터를 호스트 디스크에 저장하기](https://blog.naver.com/PostView.nhn?blogId=jevida&logNo=221449590567&categoryNo=131&parentCategoryNo=0&viewDate=&currentPage=2&postListTopCurrentPage=1&from=postList)


- [Docker Volume (컨테이너 볼륨 공유) - 컨테이너 볼륨을 다른 컨테이너와 공유하기](https://blog.naver.com/PostView.nhn?blogId=jevida&logNo=221451346970&categoryNo=131&parentCategoryNo=0&viewDate=&currentPage=2&postListTopCurrentPage=1&from=postList)


- [Docker Volume (도커 볼륨) - 도커 볼륨을 이용해서 데이터 공유하기](https://blog.naver.com/PostView.nhn?blogId=jevida&logNo=221457594486&categoryNo=131&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postList)