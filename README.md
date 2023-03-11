# Dokcer-Learning
The recommended book is "Docker in Action ed2"

**Test enviroment**
- OS: Ubuntu 22.04.2 LTS
- CPU: 11th Gen Intel Core i7-1165G\

Not using WSL (WSL2를 이용한 ubuntu 환경자체가 도커라서 도커 실행이 안된다.)

How to check computer information in Ubuntu
```
sudo apt install inxi
sudo inxi -Fz
```
or
```
cat /proc/cpuinfo
```
## Docker Desktop for Mac or Windows or Linux
https://www.docker.com/get-started/

## Docker install guide
https://docs.docker.com/engine/install \
https://docs.docker.com/compose/install/ \
I used the following URL\
https://docs.docker.com/engine/install/ubuntu/ \
https://docs.docker.com/compose/install/linux/
## Docker update script in Linux
리눅스 배포판에서 제공되는 도커 패키지가 있지만 최근에는 도커가 별도의 인스톨러를 통해 제공되기 때문에 패키지가 구 버전일 경우가 있다. 따라서 새 버전이 나올 때마다 Dokcer update script를 사용하는 방법이 있다.\
https://get.docker.com/
