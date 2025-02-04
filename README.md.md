## Ubuntu 20.04.x

```
# Ubuntu 이미지 설치
docker pull ubuntu:latest

# 컨테이너 생성
docker create -it --name ubuntu_container -p 33000:3000 -p 33306:3306 -p 38080:8080 ubuntu
## -it = 컨테이너 내부로 진입 (attach 가능, i = 상호 입출력, t = tty를 활성화하여 bash 쉘을 사용)
### -it 옵션을 설정 하지 않으면 `docker start ubuntu_container` 후에 자동 종료 된다.
## -d = 데몬 서비스 (detached 모드, exec으로 터미널 가능)
## -p = 포트 바인딩

# 컨테이너 실행
docker start ubuntu_container

# 컨테이너 Shell 접속 (root로 접속됨)
docker attach ubuntu_container
```

### Shell 접속



```
# 버전 보기
cat /etc/os-release
# 사용자 보기
cat /etc/passwd
# 현재 사용중인 SHELL 보기
echo $SHELL

# apt update를 하지 않으면 sudo, net-tools 등을 설치 할 수 없다.
apt update

# sudo 설치
apt install sudo
# 사용자 생성
sudo adduser [사용자]
# root 비밀번호 변경
sudo passwd root
# 생성한 사용자에게 sudo 권한 주기
sudo usermod -aG sudo [사용자]
# 해당 사용자로 로그인
su - [사용자]
## 이제 `docker exec -it -u [사용자] ubuntu_container /bin/bash` 접속도 가능하다.

# 네트워크 설치
sudo apt install net-tools
sudo apt install iputils-ping
ifconfig
ping [호스트 ip]
sudo apt install traceroute
sudo traceroute -T -p 80 google.com
## tracert google.com

# 포트포워드 설정에서 내부 네트워크에서 외부 IP 포트로 접속 안 되는 이유? (ChatGPT)
## 통신사의 공유기를 사용하는 경우 NAT Loopback 미지원하면 접속 할 수 없다.
## 1. 공유기 변경
## 2. 내부 컴퓨터의 /etc/hosts (127.0.0.1 도메인) 추가, 테스트 컴퓨터의 /etc/hosts (외부IP 도메인) 추가

# MariaDB
sudo apt install mariadb-server
sudo service --status-all
sudo service mariadb status
```

### Shell 접속



```bash
# 버전 보기
cat /etc/os-release
# 사용자 보기
cat /etc/passwd
# 현재 사용중인 SHELL 보기
echo $SHELL

# apt update를 하지 않으면 sudo, net-tools 등을 설치 할 수 없다.
apt update

# sudo 설치
apt install sudo
# 사용자 생성
sudo adduser [사용자]
# root 비밀번호 변경
sudo passwd root
# 생성한 사용자에게 sudo 권한 주기
sudo usermod -aG sudo [사용자]
# 해당 사용자로 로그인
su - [사용자]
## 이제 `docker exec -it -u [사용자] ubuntu_container /bin/bash` 접속도 가능하다.

# 네트워크 설치
sudo apt install net-tools
sudo apt install iputils-ping
ifconfig
ping [호스트 ip]
sudo apt install traceroute
sudo traceroute -T -p 80 google.com
## tracert google.com

# 포트포워드 설정에서 내부 네트워크에서 외부 IP 포트로 접속 안 되는 이유? (ChatGPT)
## 통신사의 공유기를 사용하는 경우 NAT Loopback 미지원하면 접속 할 수 없다.
## 1. 공유기 변경
## 2. 내부 컴퓨터의 /etc/hosts (127.0.0.1 도메인) 추가, 테스트 컴퓨터의 /etc/hosts (외부IP 도메인) 추가

# MariaDB
sudo apt install mariadb-server
sudo service --status-all
sudo service mariadb status
```

### Shell 접속

```bash
# 버전 보기
cat /etc/os-release
# 사용자 보기
cat /etc/passwd
# 현재 사용중인 SHELL 보기
echo $SHELL

# apt update를 하지 않으면 sudo, net-tools 등을 설치 할 수 없다.
apt update

# sudo 설치
apt install sudo
# 사용자 생성
sudo adduser [사용자]
# root 비밀번호 변경
sudo passwd root
# 생성한 사용자에게 sudo 권한 주기
sudo usermod -aG sudo [사용자]
# 해당 사용자로 로그인
su - [사용자]
## 이제 `docker exec -it -u [사용자] ubuntu_container /bin/bash` 접속도 가능하다.

# 네트워크 설치
sudo apt install net-tools
sudo apt install iputils-ping
ifconfig
ping [호스트 ip]
sudo apt install traceroute
sudo traceroute -T -p 80 google.com
## tracert google.com

# 포트포워드 설정에서 내부 네트워크에서 외부 IP 포트로 접속 안 되는 이유? (ChatGPT)
## 통신사의 공유기를 사용하는 경우 NAT Loopback 미지원하면 접속 할 수 없다.
## 1. 공유기 변경
## 2. 내부 컴퓨터의 /etc/hosts (127.0.0.1 도메인) 추가, 테스트 컴퓨터의 /etc/hosts (외부IP 도메인) 추가

# MariaDB
sudo apt install mariadb-server
sudo service --status-all
sudo service mariadb status
sudo service mariadb start
sudo mysql -u root
```

- [MariaDB - 권한](https://github.com/ovdncids/mysql-curriculum/blob/master/GrantDump.md)
- [Raspberry Pi - Java](https://github.com/ovdncids/raspberrypi-curriculum#java)

## 컨테이너 생성시 포트를 설정하지 않은 경우

- https://stackoverflow.com/questions/19335444/how-do-i-assign-a-port-mapping-to-an-existing-docker-container
- [컨테이너 파일의 위치](https://yooloo.tistory.com/188)
- [chroot 설명](https://www.44bits.io/ko/post/change-root-directory-by-using-chroot)

```
# 윈도우 Docker 설정 파일 위치
C:\Users\[사용자]\AppData\Local\Docker\wsl\data\ext4.vhdx

docker info
## Docker Root Dir: /var/lib/docker
## 해당 경로를 ubuntu에서 접근해야 한다.
docker inspect ubuntu_container
## 설정 정보를 확인
docker run -v/:/data -it --name docker_ubuntu ubuntu /bin/bash
## -v = 볼륨, /:/data = `Docker Root Dir`에 접근 가능하게 해준다.

# `Docker Root Dir`에 접근
chroot /data

# 컨테이너 inspect 설정 경로 이동
cd /var/lib/docker/containers/[컨테이너 해시값]
mv hostconfig.json hostconfig.json.ori
cat hostconfig.json.ori
mv config.v2.json config.v2.json.ori
cat config.v2.json.ori

# hostconfig.json 파일 새로 생성 ("ExposedPorts" 부분에 포트 추가)
cat << EOF > hostconfig.json
{hostconfig.json.ori 파일을 수정해서 넣음}
EOF
# config.v2.json 파일 새로 생성 ("PortBindings" 부분에 포트 추가)
cat << EOF > config.v2.json
{config.v2.json.ori 파일을 수정해서 넣음}
EOF
```

### telnet 접속 불가시

<img src="./image-20241127230138859.png" alt="telnet" style="zoom:25%;" />