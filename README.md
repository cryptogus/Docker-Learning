# Dokcer-Learning
The recommended book is "Docker in Action ed2"

**Test enviroment**
- OS: Ubuntu 22.04.2 LTS
- CPU: 11th Gen Intel Core i7-1165G

How to check computer information in Ubuntu
```bash
$ sudo apt install inxi
$ sudo inxi -Fz
```
or
```bash
$ cat /proc/cpuinfo
```
## Docker Desktop for Mac or Windows or Linux
https://www.docker.com/get-started/ \
윈도우에서는 Docker Desktop 프로그램을 켜두면 터미널(PowerShell)에서 docker 명령어를 사용가능하며 요즘은 WSL2와 연동도 되어 WSL2 기반의 Ubuntu에서도 윈도우에서 프로그램만 켜두면 따로 apt로 패키지 설치하지 않더라고 docker 명령어를 사용가능하다.
## Docker install guide
https://docs.docker.com/engine/install \
https://docs.docker.com/compose/install/ \
I used the following URL\
https://docs.docker.com/engine/install/ubuntu/ \
https://docs.docker.com/compose/install/linux/
## Docker update script in Linux
리눅스 배포판에서 제공되는 도커 패키지가 있지만 최근에는 도커가 별도의 인스톨러를 통해 제공되기 때문에 패키지가 구 버전일 경우가 있다. 따라서 새 버전이 나올 때마다 Dokcer update script를 사용하는 방법이 있다.\
https://get.docker.com/
## docker without sudo

https://docs.docker.com/engine/install/linux-postinstall/ \
도커는 기본적으로 `sudo` 권한을 요구한다. 하지만 매번 `sudo`를 붙여주긴 귀찮다면 아래와 같이 한다면 `sudo` 명령어 없이 docker 실행이 가능하다.

```bash
# user 변수 확인
$ echo $USER

# 현재 사용중인 사용자를 docker 그룹에 등록
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
newgrp docker
```
socket permission denied 발생 시
```bash
$ sudo chmod 666 /var/run/docker.sock
```

## Execute a command in a running container

https://docs.docker.com/engine/reference/commandline/exec/ \
For example in Ubuntu
```bash
# 실행 중인 CONTAINER ID 확인
$ docker ps

# 컨테이너 내부에 접속해 터미널 조작
$ docker exec -it <CONTAINER ID or NAMES> /bin/bash

# 컨테이너에서 나가는 명령어
$ exit
```
`--interactive` 플래그는 컨테이너에 접속된 상태를 의미하고 `--tty`는 터미널 세션을 통해 컨테이너를 조작하겠다는 플래그이다.  
위에서 `-it` 옵션은 이 둘을 합친 것이다.
```bat
# 윈도우라면 명령 프롬프트가 뜨고 리눅스라면 bash가 뜰 것이다.
# 도커의 특징인 운영체제 공유때문!
PS> docker container run --interactive --tty <REPOSITORY>
```
## Initializing the Docker environment
```bash
# 실행 중인 docker 조회
$ docker ps
# 모든 docker 조회
$ docker ps -a
# 모든 image 조회
$ docker images
# 대상 컨테이너의 상세 정보 확인
$ docker inspect [OPTIONS] NAME|ID [NAME|ID...]
```
모든 docker 삭제 command
```bash
# -q 옵션은 ID 만 표시 되도록 하는 옵션
# 리눅스 백쿼터에 대한 지식 필요
$ docker rm -f `docker ps -aq`
$ docker rmi -f `docker images -q`
```
윈도우, 리눅스 환경 둘 다 사용가능한 command
```bat
PS> docker rm -f $(docker ps -aq)
PS> docker rmi -f $(docker images -q)
```
특정 docker 지정 삭제 command
```bash
$ docker rm -f <CONTAINER ID>
$ docker rmi -f <IMAGE ID>
```
Quantum-tomcat/Dokcerfile 실행   
reference → https://docs.docker.com/engine/reference/commandline/build/
```bash
$ docker build -t oqs_tom:v1 . # --network <nat 등> 원하는 option 추가
$ docker run -d -p 8080:8080 oqs_tom:v1
# tomcat tls or tomcat https 인증서 적용법 등 검색
# server.xml 수정으로 기억
```

### TroubleShooting
```bash
E: Failed to fetch http://deb.debian.org/debian/pool/main/libt/libtool/libtool_2.4.6-15_all.deb  Error reading from server - read (104: Connection reset by peer) [IP: 146.75.50.132 80]
E: Unable to fetch some archives, maybe run apt-get update or try with --fix-missing?
```
**solution?**  
https://lxadm.com/reason-error-reading-from-remote-server/
