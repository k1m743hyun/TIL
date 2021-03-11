# 도커 네트워크(Docker Network)
&nbsp;
## 1. 도커 네트워크 구조

- 도커는 컨테이너에 내부 IP를 순차적으로 할당하며, 이 IP는 컨테이너를 재시작할 때마다 변경될 수 있습니다.


- 도커가 설치된 호스트에서 `ifconfig`나 `ip addr`과 같은 명령어로 네트워크 인터페이스를 확인하면 실행 중인 컨테이너 수만큼 `veth`로 시작하는 인터페이스가 생성된 것을 알 수 있습니다.


- 컨테이너의 `eth0` 인터페이스는 호스트의 `veth…`라는 네트워크 인터페이스와 연결되어 있으며 `veth` 인터페이스는 `docker0` 브리지와 바인딩되어 외부와 통신할 수 있습니다.
&nbsp;
&nbsp;
## 2. 도커 네트워크 기능

- 컨테이너를 생성하면 기본적으로 `docker0` 브리지를 통해 외부와 통신할 수 있는 환경을 사용할 수 있지만, 사용자의 선택에 따라 여러 네트워크 드라이버를 사용할 수 있습니다.


- 대표적인 네트워크 드라이버로는 브리지(bridge), 호스트(host), 논(none), 컨테이너(container), 오버레이(overlay)가 있습니다.


- 서드파티(third-party) 플러그인 솔루션으로는 weave, flannel, openswitch 등이 있으며, 더 확장된 네트워크 구성을 위해 활용됩니다.


- `docker network ls` 명령어로 네트워크 목록을 확인하면, 이미 브리지, 호스트, 논 네트워크가 있음을 알 수 있습니다.


- 아무런 설정을 하지 않고 컨테이너를 생성하면 컨테이너는 자동으로 `docker0` 브리지를 사용합니다.
&nbsp;
### (1) 브리지 네트워크(Bridge Network)
> `docker0`이 아닌 사용자 정의 브리지를 새로 생성해 각 컨테이너에 연결하는 네트워크 구조

&nbsp;
- 기본적으로 존재하는 `docker0`을 사용하는 브리지 네트워크가 아닌 새로운 브리지 타입의 네트워크를 생성할 수 있습니다.
```bash
$ docker network create --driver bridge mybridge
```
&nbsp;

- `docker run` 또는 `docker create` 명령어에 `--net` 옵션의 값을 설정하면 컨테이너가 이 네트워크를 사용하도록 설정할 수 있습니다.
```bash
$ docker run -i -t --name mycontainer --net mybridge ubuntu:14.04
```
&nbsp;

- 이렇게 생성된 사용자 정의 네트워크는 `docker network connect`와 `docker network disconnect`를 통해 컨테이너에 유동적으로 붙이고 뗄 수 있습니다.
```bash
$ docker network connect mybridge mycontainer
$ docker network disconnect mybridge mycontainer
```
&nbsp;

- 네트워크의 서브넷, 게이트웨이, IP 할당 범위 등을 임의로 설정하려면 네트워크를 생성할 때 아래와 같이 `--subnet`, `--ip-range`, `--gateway` 옵션을 추가합니다.
단, `--subnet`, `--ip-range`는 같은 대역이어야 합니다.
```bash
$ docker network create --driver=bridge --subnet=172.72.0.0/16 --ip-range=172.72.0.0/24 --gateway=172.72.0.1 my_custom_network
```
&nbsp;
&nbsp;
### (2) 호스트 네트워크(Host Network)

- 네트워크를 호스트로 연결하면 호스트의 네트워크 환경을 그대로 쓸 수 있습니다.
```bash
$ docker run -i -t --name network_host --net host ubuntu:14.04
```
&nbsp;

- `--net` 옵션을 입력해 호스트를 설정한 컨테이너 내부에서 네트워크 환경을 확인하면 호스트와 같은 것을 알 수 있습니다.


- 호스트 머신에서 설정한 호스트 이름도 컨테이너가 물려받기 때문에 컨테이너의 호스트 이름도 무작위 16진수가 아니라 도커 엔진이 설치된 호스트 머신의 호스트 이름으로 설정됩니다.


- 컨테이너의 네트워크를 호스트 모드로 설정하면 컨테이너 내부의 애플리케이션을 별도의 포트 포워딩 없이 바로 서비스할 수 있습니다.
이는 마치 실제 호스트에서 애플리케이션을 외부에 노출하는 것과 같습니다.
&nbsp;
&nbsp;
### (3) 논 네트워크(None Network)
> 말 그대로 아무런 네트워크를 사용하지 않는 것

```bash
$ docker run -i -t --name network_none --net none ubuntu:14.04
```
&nbsp;
&nbsp;
### (4) 컨테이너 네트워크(Container Network)

- `--net` 옵션으로 `container`를 입력하면 다른 컨테이너의 네트워크 네임스페이스 환경을 공유할 수 있습니다.
공유되는 속성을 **내부 IP**, 네트워크 인터페이스의 **맥(MAC) 주소** 등입니다.
```bash
$ docker run -i -t -d --name network_container_1 ubuntu:14.04
```
&nbsp;

- `--net` 옵션의 값으로 `container:[다른 컨테이너의 ID]`와 같이 입력합니다.
```bash
$ docker run -i -t -d --name network_container_2 --net container:network_container_1 ubuntu:14.04
```
&nbsp;

- 위와 같이 다른 컨테이너의 네트워크 환경을 공유하면 내부 IP를 새로 할당받지 않으며 호스트에 `veth`로 시작하는 가상 네트워크 인터페이스도 생성되지 않습니다.
`network_container_2` 컨테이너의 네트워크와 관련된 사항은 전부 network_container_1과 같게 설정됩니다.
&nbsp;
&nbsp;
### (5) 브리지 네트워크와 `--net-alias`

- 브리지 타입의 네트워크와 `run` 명령어의 `--net-alias` 옵션을 함께 쓰면 특정 호스트 이름으로 컨테이너 여러 개에 접근할 수 있습니다.


- 호스트 이름으로 접근하면 별도의 알고리즘이 아닌 라운드 로빈(round-robin) 방식을 이용해 컨테이너의 IP 리스트를 반환합니다.
&nbsp;
&nbsp;
### (6) MacVLAN 네트워크

- 호스트의 네트워크 인터페이스 카드를 가상화해 물리 네트워크 환경을 컨테이너에게 동일하게 제공합니다.


- MacVLAN을 사용하면 컨테이너는 물리 네트워크 상에서 가상의 맥(MAC) 주소를 가지며, 해당 네트워크에 연결된 다른 장치와의 통신이 가능해집니다.


- MacVLAN을 사용하는 컨테이너들과 동일한 IP 대역을 사용하는 서버 및 컨테이너들은 서로 통신이 가능합니다.
하지만, MacVLAN 네트워크를 사용하는 컨테이너는 기본적으로 호스트와 통신이 불가능합니다.


- MacVLAN을 사용하려면 적어도 1개의 네트워크 장비와 서버가 필요합니다.
그래서 두 서버에 MacVLAN 네트워크를 생성합니다.
```bash
root@node01$ docker network create -d macvlan --subnet=192.168.0.0/24 --ip-range=192.168.0.64/28 --gateway=192.168.0.1 -o macvlan_mode=bridge -o parent=eth0 my_macvlan
```
&nbsp;

```bash
root@node02$ docker network create -d macvlan --subnet=192.168.0.0/24 --ip-range=192.168.0.128/28 --gateway=192.168.0.1 -o macvlan_mode=bridge -o parent=eth0 my_macvlan
```
&nbsp;

- 브리지 네트워크를 생성했을 때와 동일하게 `docker network create` 명령어를 사용했지만, 이번에 여러가지 옵션들과 함께 사용됐습니다.
  &nbsp;
  - `-d`: 네트워크 드라이버로 `macvlan`을 사용한다는 것을 명시합니다. `--driver`와 같습니다.
  &nbsp;
  - `--subnet`: 컨테이너가 사용할 네트워크 정보를 입력합니다. 여기서는 네트워크 장비의 IP 대역 기본 설정을 그대로 따릅니다.
  &nbsp;
  - `--ip-range`: MacVLAN을 생성하는 호스트에서 사용할 컨테이너의 IP 범위를 입력합니다. node01과 node02의 IP 범위가 겹쳐 동일한 IP의 컨테이너가 각각 생성된다면 컨테이너 네트워크가 정상적으로 동작하지 않을 수 있으므로 반드시 겹치지 않게 설정해야 합니다.  
  &nbsp;
  - `--gateway`: 네트워크에 설정된 게이트웨이를 입력합니다. 여기서는 네트워크 장비의 기본 설정을 그대로 따릅니다.
  &nbsp;
  - `-o`: 네트워크의 추가적인 옵션을 설정합니다. 위 예시에서는 `macvlan_mode=bridge`값을 통해서 MacVLAN을 `bridge` 모드로, `parent=eth0`값을 통해 MacVLAN으로 생성될 컨테이너 네트워크 인터페이스의 부모(parent) 인터페이스를 `eth0`으로 지정합니다. `eth0`은 공유기에 랜선으로 연결되어 192.168.0.0/24 대역의 IP를 할당받은 네트워크 인터페이스입니다.


- MacVLAN 네트워크를 사용하는 컨테이너를 생성해보겠습니다.
```bash
root@node01$ docker run -it --name c1 --hostname c1 --network my_macvlan ubuntu:14.04
```
&nbsp;

```bash
root@node02$ docker run -it --name c2 --hostname c2 --network my_macvlan ubuntu:14.04
```
&nbsp;
&nbsp;
# 참고 자료
- [시작하세요! 도커/쿠버네티스 - YES24](http://www.yes24.com/Product/Goods/84927385)


- [도커 네트워크 요약 (Docker Networking)](https://jonnung.dev/docker/2020/02/16/docker_network/)