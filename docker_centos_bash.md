Ubuntu Linux 에서 CentOS의 Bash 셸 사용하기
===========================================
```
1. 관련 환경 설치
2. 도커에서 배포하는 GPG(Gun Privacy Guard) 키 설치
3. 저장소 추가
4. 패키지 업데이트
5. 패키지 설치
6. Docker 서비스 등록
7. 설치 되었는지 확인
8. docker centos 셸 추가
9. docker에서 centos 셸 실행
10. centos 버전 확인
11. 확인 및 삭제 명령어
```

1.&nbsp;관련 환경 설치
--------------------------
```
root@server-b:~# apt -y install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
ca-certificates is already the newest version (20211016ubuntu0.22.04.1).
ca-certificates set to manually installed.
curl is already the newest version (7.81.0-1ubuntu1.10).
curl set to manually installed.

~~~~~~~~~~~~~~~~~생략~~~~~~~~~~~~~~~~~~~~~~~

Scanning processes...
Scanning candidates...
Scanning linux images...

Running kernel seems to be up-to-date.
```
2.&nbsp;도커에서 배포하는 GPG(Gun Privacy Guard) 키 설치
--------------------------
```
root@server-b:~# curl -fsSl https://download.docker.com/linux/ubuntu/gpg | apt-key add -
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
OK
```
3.&nbsp;저장소 추가
--------------------------
```
root@server-b:~# add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
Repository: 'deb [arch=amd64] https://download.docker.com/linux/ubuntu jammy stable'
Description:
Archive for codename: jammy components: stable
More info: https://download.docker.com/linux/ubuntu
Adding repository.
Press [ENTER] to continue or Ctrl-c to cancel.
Adding deb entry to /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-jammy.list
Adding disabled deb-src entry to /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-jammy.list
Hit:1 http://mirror.kakao.com/ubuntu jammy InRelease
Get:2 http://mirror.kakao.com/ubuntu jammy-updates InRelease [119 kB]
Get:3 http://mirror.kakao.com/ubuntu jammy-backports InRelease [107 kB]
Get:4 https://download.docker.com/linux/ubuntu jammy InRelease [48.9 kB]
Hit:5 http://kr.archive.ubuntu.com/ubuntu jammy InRelease
Get:6 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:7 http://kr.archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:8 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages [13.8 kB]
Get:9 http://kr.archive.ubuntu.com/ubuntu jammy-backports InRelease [107 kB]
Get:10 http://kr.archive.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:11 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [693 kB]
Get:12 http://kr.archive.ubuntu.com/ubuntu jammy-security/main amd64 Packages [693 kB]
Get:13 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [714 kB]
Get:14 http://kr.archive.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [714 kB]
Fetched 3,547 kB in 3s (1,043 kB/s)
Reading package lists... Done
W: https://download.docker.com/linux/ubuntu/dists/jammy/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
```
4.&nbsp;패키지 업데이트
--------------------------
```
root@server-b:~# apt update
Hit:1 http://mirror.kakao.com/ubuntu jammy InRelease
Hit:2 http://mirror.kakao.com/ubuntu jammy-updates InRelease
Hit:3 http://mirror.kakao.com/ubuntu jammy-backports InRelease
Hit:4 https://download.docker.com/linux/ubuntu jammy InRelease
Hit:5 http://kr.archive.ubuntu.com/ubuntu jammy InRelease
Hit:6 http://security.ubuntu.com/ubuntu jammy-security InRelease
Hit:7 http://kr.archive.ubuntu.com/ubuntu jammy-updates InRelease
Hit:8 http://kr.archive.ubuntu.com/ubuntu jammy-backports InRelease
Hit:9 http://kr.archive.ubuntu.com/ubuntu jammy-security InRelease
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
69 packages can be upgraded. Run 'apt list --upgradable' to see them.
W: https://download.docker.com/linux/ubuntu/dists/jammy/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
```

5.&nbsp;패키지 설치
--------------------------
```
root@server-b:~# apt -y install docker-ce docker-ce-cli containerd.io
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  docker-buildx-plugin docker-ce-rootless-extras docker-compose-plugin docker-scan-plugin libltdl7 libslirp0 pigz slirp4netns
Suggested packages:
  aufs-tools cgroupfs-mount | cgroup-lite
The following NEW packages will be installed:
  containerd.io docker-buildx-plugin docker-ce docker-ce-cli docker-ce-rootless-extras docker-compose-plugin docker-scan-plugin
  libltdl7 libslirp0 pigz slirp4netns
0 upgraded, 11 newly installed, 0 to remove and 69 not upgraded.
Need to get 112 MB of archives.
After this operation, 401 MB of additional disk space will be used.
Get:1 http://mirror.kakao.com/ubuntu jammy/universe amd64 pigz amd64 2.6-1 [63.6 kB]
Get:2 http://mirror.kakao.com/ubuntu jammy/main amd64 libltdl7 amd64 2.4.6-15build2 [39.6 kB]
Get:3 http://mirror.kakao.com/ubuntu jammy/main amd64 libslirp0 amd64 4.6.1-1build1 [61.5 kB]
Get:4 http://mirror.kakao.com/ubuntu jammy/universe amd64 slirp4netns amd64 1.0.1-2 [28.2 kB]
Get:5 https://download.docker.com/linux/ubuntu jammy/stable amd64 containerd.io amd64 1.6.19-1 [28.2 MB]
Get:6 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-buildx-plugin amd64 0.10.2-1~ubuntu.22.04~jammy [25.9 MB]
Get:7 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-ce-cli amd64 5:23.0.1-1~ubuntu.22.04~jammy [13.2 MB]
Get:8 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-ce amd64 5:23.0.1-1~ubuntu.22.04~jammy [22.0 MB]
Get:9 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-ce-rootless-extras amd64 5:23.0.1-1~ubuntu.22.04~jammy [8,760 kB]
Get:10 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-compose-plugin amd64 2.16.0-1~ubuntu.22.04~jammy [10.2 MB]
Get:11 https://download.docker.com/linux/ubuntu jammy/stable amd64 docker-scan-plugin amd64 0.23.0~ubuntu-jammy [3,623 kB]
Fetched 112 MB in 5s (20.7 MB/s)

~~~~~~~~~~~~~~~~~생략~~~~~~~~~~~~~~~~~~~~~~~

Scanning processes...
Scanning candidates...
Scanning linux images...

Running kernel seems to be up-to-date.
```

6.&nbsp;Docker 서비스 등록
--------------------------
```
systemctl start docker
systemctl enable docker
systemctl status docker

root@server-b:~# systemctl start docker
root@server-b:~# systemctl enable docker
Synchronizing state of docker.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable docker
root@server-b:~# systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-03-27 07:00:38 UTC; 36min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 18564 (dockerd)
      Tasks: 11
     Memory: 276.9M
        CPU: 21.237s
     CGroup: /system.slice/docker.service
             └─18564 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Mar 27 07:00:37 server-b dockerd[18564]: time="2023-03-27T07:00:37.237476890Z" level=info msg="Loading containers: start."
Mar 27 07:00:37 server-b dockerd[18564]: time="2023-03-27T07:00:37.640383097Z" level=info msg="Default bridge (docker0) is assigne>
Mar 27 07:00:37 server-b dockerd[18564]: time="2023-03-27T07:00:37.902384063Z" level=info msg="Loading containers: done."
Mar 27 07:00:37 server-b dockerd[18564]: time="2023-03-27T07:00:37.955511757Z" level=info msg="Docker daemon" commit=bc3805a graph>
Mar 27 07:00:37 server-b dockerd[18564]: time="2023-03-27T07:00:37.956580150Z" level=info msg="Daemon has completed initialization"
Mar 27 07:00:38 server-b dockerd[18564]: time="2023-03-27T07:00:38.002459362Z" level=info msg="[core] [Server #7] Server created" >
Mar 27 07:00:38 server-b systemd[1]: Started Docker Application Container Engine.
Mar 27 07:00:38 server-b dockerd[18564]: time="2023-03-27T07:00:38.019628964Z" level=info msg="API listen on /run/docker.sock"
Mar 27 07:01:51 server-b dockerd[18564]: time="2023-03-27T07:01:51.188184932Z" level=info msg="ignoring event" container=781db22ca>
Mar 27 07:11:13 server-b dockerd[18564]: time="2023-03-27T07:11:13.342302681Z" level=info msg="ignoring event" container=abf309898>
lines 1-22/22 (END)

```

7.&nbsp;Docker 서비스 등록
--------------------------
```

root@server-b:~# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:ffb13da98453e0f04d33a6eee5bb8e46ee50d08ebe17735fc0779d0349e889e9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

8.&nbsp;Docker 서비스 등록
--------------------------
```
root@server-b:~# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
```
9.&nbsp;Docker 서비스 등록
--------------------------
```
root@server-b:~# docker run -it centos bash
[root@abf309898f78 /]#
```
10.&nbsp;Docker 서비스 등록
--------------------------
```
[root@abf309898f78 /]# cat /etc/system-release
CentOS Linux release 8.4.2105
```

11.&nbsp;확인 및 삭제 명령어
--------------------------
```
도커 정보 확인 : docker images
root@server-b:~# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    feb5d9fea6a5   18 months ago   13.3kB
centos        latest    5d0da3dc9764   18 months ago   231MB


상세확인 : docker container ls -a
root@server-b:~# docker container ls -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                      PORTS     NAMES
abf309898f78   centos        "bash"     5 minutes ago   Exited (0) 21 seconds ago             romantic_snyder
781db22ca25c   hello-world   "/hello"   9 minutes ago   Exited (0) 9 minutes ago              hungry_swanson

컨테이너 삭제 : docker rm 컨테이너ID
root@server-b:~# docker rm 781db22ca25c
781db22ca25c
root@server-b:~# docker container ls -a
CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS                          PORTS     NAMES
abf309898f78   centos    "bash"    6 minutes ago   Exited (0) About a minute ago             romantic_snyder
```
