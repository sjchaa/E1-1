# 내 컴퓨터에 개발자용 '작업실' 꾸미기

## 1. 프로젝트 개요
- **목표:** 공용 PC 환경에서도 재현 가능하고 독립된 개발 워크스테이션(리눅스 CLI 터미널, Docker 컨테이너, Git/GitHub 버전 관리)을 구축하고, Docker 컨테이너 실행 및 볼륨·포트 검증 과정을 문서화하여 GitHub에 제출.

- **주요 활용 도구:** Linux CLI (Ubuntu/WSL2), Docker, Git & GitHub, VS Code.

## 2. 실행 환경
- **OS:** Ubuntu 26.04.1 LTS (WSL2 on Windows)
- **Shell:** Zsh (`/bin/zsh`)
- **Docker Version:** Docker version 29.9.3, build 5457199
- **Git Version:** git version 2.53.0

## 3. 수행 항목 체크리스트
✅ 터미널 기본 조작 및 폴더 구성 (`~/projects/myapp`)  
✅ 권한 변경 실습 (`chmod`, 소유권 및 접근 권한 `rwx` 확인)  
✅ Docker 설치 및 데몬 점검 (`docker info`)  
✅ `hello-world` 컨테이너 실행 성공  
✅ Dockerfile 작성 및 커스텀 웹 서버 이미지 빌드/실행  
✅ 포트 매핑 및 브라우저/curl 접속 검증  
✅ Docker 볼륨 영속성 및 바인드 마운트 검증  
✅ Git 설정 및 GitHub/VS Code 연동 완료  

## 4. 검증 방법 및 터미널 조작 로그
### (1) 현재 위치 확인

```
$ pwd
/home/sj_ee_6255/projects/myapp
```

### (2) 목록 확인(숨김 파일 포함) (ls -la)

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ la -la
total 24
drwxr-xr-x 3 sj_ee_6255 sj_ee_6255 4096 Nov  9 23:58 .
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:53 ..
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   27 Nov  9 23:52 .envrc
drwxr-xr-x 5 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:19 .venv
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 1220 Nov 10 00:35 README.md
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov  9 23:02 index.html
```

### (3) 디렉토리 생성 및 이동 (mkdir, cd 명령어 사용)

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ mkdir test_dir
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ cd test_dir
sj_ee_6255@GNIA-C202-B06:~/projects/myapp/test_dir$ pwd
/home/sj_ee_6255/projects/myapp/test_dir
```

### (4) 빈 파일 생성 (touch 명령어 사용)
```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ touch test.txt
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ ls -la
total 28
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:28 .
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:53 ..
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   27 Nov  9 23:52 .envrc
drwxr-xr-x 5 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:19 .venv
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 1220 Nov 10 00:35 README.md
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov  9 23:02 index.html
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255    0 Nov 10 23:28 test.txt
drwxr-xr-x 2 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:19 test_dir
```

### (5) 파일 내용 입력 및 확인 (echo, cat)

파일 내용을 입력하고 출력해 확인합니다.

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ echo "test 123456789" > test.txt
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ cat test.txt
test 123456789
```

### (6) 파일 복사 (cp)

파일을 다른 이름으로 복사합니다. 
test.txt 파일을 test1.txt로 복사합니다.


```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ cp test.txt test_1.txt
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ ls -la
total 36
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:36 .
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:53 ..
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   27 Nov  9 23:52 .envrc
drwxr-xr-x 5 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:19 .venv
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 1220 Nov 10 00:35 README.md
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov  9 23:02 index.html
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   15 Nov 10 23:34 test.txt
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   15 Nov 10 23:36 test_1.txt
drwxr-xr-x 2 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:19 test_dir
```

### (7) 파일 이동 및 이름 변경 (mv)

파일의 이름을 바꾸거나 위치를 이동합니다.  
복사한 test_1.txt 파일의 이름을 test_2.txt로 변경했습니다.

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ mv test_1.txt test_2.txt
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ ls -la
total 36
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:39 .
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:53 ..
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   27 Nov  9 23:52 .envrc
drwxr-xr-x 5 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:19 .venv
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 1220 Nov 10 00:35 README.md
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov  9 23:02 index.html
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   15 Nov 10 23:34 test.txt
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   15 Nov 10 23:36 test_2.txt
drwxr-xr-x 2 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:19 test_dir
```

### (8) 디렉토리 및 파일 삭제 (rm -rf, rm)

디렉토리와 내부 파일까지 모두 삭제합니다.  
(디렉토리 삭제는 rm -rf 명령어를 사용하여 영구 삭제 했습니다.)

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ rm -rf test_dir
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ ls -la
total 32
drwxr-xr-x 3 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:45 .
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:53 ..
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   27 Nov  9 23:52 .envrc
drwxr-xr-x 5 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:19 .venv
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 1220 Nov 10 00:35 README.md
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov  9 23:02 index.html
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   15 Nov 10 23:34 test.txt
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   15 Nov 10 23:36 test_2.txt
```

텍스트 파일을 삭제합니다. 
test.txt와 test_2.txt 파일은 test_dir 가 아닌 상위 폴더에 있었기 때문에 별도로 삭제를 진행합니다.

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ rm test.txt test_2.txt
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ ls -la
total 24
drwxr-xr-x 3 sj_ee_6255 sj_ee_6255 4096 Nov 10 23:50 .
drwxr-xr-x 4 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:53 ..
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   27 Nov  9 23:52 .envrc
drwxr-xr-x 5 sj_ee_6255 sj_ee_6255 4096 Nov  9 22:19 .venv
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 1220 Nov 10 00:35 README.md
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov  9 23:02 index.html
```

## 5. 권한 실습 및 증거 기록

### (1) 권한 확인 (ls -l)

파일과 디렉토리의 현재 권한`rmw`를 확인하고, `chmod` 명령어를 통해 권한을 변경한 뒤 변경 전/후를  비교하였습니다.


#### ▷ 파일 권한 확인 및 변경

임의로 텍스트 파일을 생성한 후 권한 확인과 변경을 진행합니다.

touch 명령어로 파일 생성  
ls -l 명령어로 권한 확인  
chmod 명령어로 권한 변경 진행


```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ touch abcdefg.txt
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ ls -l
total 12
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 6359 Nov 17 23:41 README.md
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255    0 Nov 17 23:49 abcdefg.txt
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov 17 21:55 index.html
```

chmod 755 명령어로 

소유자는 읽기/쓰기/실행  
그룹 사용자는 읽기/실행  
기타 사용자는 읽기/실행  

이 가능하도록 실행 권한을 추가했습니다.

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ chmod 755 abcdefg.txt
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ ls -l
total 12
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 6359 Nov 17 23:41 README.md
-rwxr-xr-x 1 sj_ee_6255 sj_ee_6255    0 Nov 17 23:49 abcdefg.txt
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov 17 21:55 index.html
```

#### ▷ 파일 권한 변경 전후 비교

- **변경 전 권한 (`-rw-r--r--`):** 소유자(읽기/쓰기),  
그룹(읽기 전용), 기타 사용자(읽기 전용) 상태이며 실행 권한이 없습니다.

- **변경 후 권한 (`-rwxr-xr-x`):** `chmod 755` 명령어 적용 후,  
소유자와 그룹, 기타 사용자 모두에게 실행(`x`) 권한이 추가/변경된 것을 확인할 수 있습니다.

```
# 변경 전
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255    0 Nov 17 23:49 abcdefg.txt

# 변경후
-rwxr-xr-x 1 sj_ee_6255 sj_ee_6255    0 Nov 17 23:49 abcdefg.txt
```



#### ▷ 디렉토리 권한 확인 및 변경

디렉토리를 생성하고 권한을 확인합니다.

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ mkdir abcdefg_dir
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ ls -l
total 16
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 6359 Nov 17 23:41 README.md
-rwxr-xr-x 1 sj_ee_6255 sj_ee_6255    0 Nov 17 23:49 abcdefg.txt
drwxr-xr-x 2 sj_ee_6255 sj_ee_6255 4096 Nov 18 00:05 abcdefg_dir
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov 17 21:55 index.html
```

현재 새롭게 생성한 디렉토리의 권한은

소유자는 읽기/쓰기/실행  
그룹 사용자는 읽기/실행  
기타 사용자는 읽기/실행

이 가능했으며, 소유주가 아닌 그룹 사용자와 기타 사용자의 모든 권한을 해제했습니다.

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ chmod 700 abcdefg_dir
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ ls -l
total 16
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255 6359 Nov 17 23:41 README.md
-rwxr-xr-x 1 sj_ee_6255 sj_ee_6255    0 Nov 17 23:49 abcdefg.txt
drwx------ 2 sj_ee_6255 sj_ee_6255 4096 Nov 18 00:05 abcdefg_dir
-rw-r--r-- 1 sj_ee_6255 sj_ee_6255   76 Nov 17 21:55 index.html
```

#### ▷ 디렉토리 권한 변경 전후 비교

소유자 외의 모든 접근을 차단하는 `chmod 700` 명령어를 수행하여 변경 전후를 비교하였습니다.

- **변경 전 권한 (`drwxr-xr-x`):** 소유자는 모든 권한(읽기/쓰기/실행), 그룹 및 기타 사용자는 읽기 및 실행(조회)이 가능한 상태입니다.
- **변경 후 권한 (`drwx------`):** `chmod 700` 적용 후, 오직 소유자만 접근 및 조회가 가능하도록 보안이 강화되었습니다.

```
# 변경 전
drwxr-xr-x 2 sj_ee_6255 sj_ee_6255 4096 Nov 18 00:05 abcdefg_dir

# 변경후
drwx------ 2 sj_ee_6255 sj_ee_6255 4096 Nov 18 00:05 abcdefg_dir 
```

## 6. Docker 설치 및 기본 점검

### 6-1. Docker 버전 확인 및 결과

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ docker --version
Docker version 29.9.3, build 5457199
```

### 6-2. Docker 데몬 동작 여부 확인 및 결과

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ docker info
Client:
 Version:    29.9.3
 Context:    default
 Debug Mode: false
 Plugins:
  agent: Docker AI Agent Runner (Docker Inc.)
    Version:  v1.153.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-agent.exe
  ai: Docker AI Agent - Ask Gordon (Docker Inc.)
    Version:  v1.33.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-ai.exe
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-buildx.exe
  compose: Docker Compose (Docker Inc.)
    Version:  v5.7.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-compose.exe
  debug: Get a shell into any image or container (Docker Inc.)
    Version:  0.49.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-debug.exe
  desktop: Docker Desktop commands (Docker Inc.)
    Version:  4.99.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-desktop.exe
  dhi: CLI for managing Docker Hardened Images (Docker Inc.)
    Version:  v0.0.10
    Path:     C:\Program Files\Docker\cli-plugins\docker-dhi.exe
  extension: Manages Docker extensions (Docker Inc.)
    Version:  v0.2.32
    Path:     C:\Program Files\Docker\cli-plugins\docker-extension.exe
  init: Creates Docker-related starter files for your project (Docker Inc.)
    Version:  v1.4.1
    Path:     C:\Program Files\Docker\cli-plugins\docker-init.exe
  mcp: Docker MCP Plugin (Docker Inc.)
    Version:  v0.44.3
    Path:     C:\Program Files\Docker\cli-plugins\docker-mcp.exe
  model: Docker Model Runner (Docker Inc.)
    Version:  v1.2.9
    Path:     C:\Program Files\Docker\cli-plugins\docker-model.exe
  offload: Docker Offload (Docker Inc.)
    Version:  v0.7.36
    Path:     C:\Program Files\Docker\cli-plugins\docker-offload.exe
  pass: Docker Pass Secrets Manager Plugin (beta) (Docker Inc.)
    Version:  v0.3.2
    Path:     C:\Program Files\Docker\cli-plugins\docker-pass.exe
  sandbox: "docker sandbox" is deprecated, use Docker Sandboxes instead (Docker Inc.)
    Version:  v0.13.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-sandbox.exe
  scout: Docker Scout (Docker Inc.)
    Version:  v1.28.0
    Path:     C:\Program Files\Docker\cli-plugins\docker-scout.exe

Server:
failed to connect to the docker API at npipe:////./pipe/docker_engine; check if the path is correct and if the daemon is running: open //./pipe/docker_engine: The system cannot find the file specified.
```

## 7. Docker 기본 운영 명령 수행

### 7-1. 이미지 : 다운로드/목록 확인

Nginx 웹서버 이미지를 원격 저장소에서 다운로드(`pull`)하고,  
로컬에 저장된 이미지 목록을 확인(`images`)했습니다.


- **이미지 다운로드**

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker pull nginx:alpine
alpine: Pulling from library/nginx
881fc932939e: Pull complete
1abafee971b9: Pull complete
c7dfd64eeb02: Pull complete
e011f124249b: Pull complete
a746bd1dc828: Pull complete
b9c6ee6a30f8: Pull complete
69d81d552257: Pull complete
da90db506dd5: Pull complete
Digest: sha256:fed7e14be83cbd2661a75cdc5b872d936cf155d46c327084d30b8df19ae88d72
Status: Downloaded newer image for nginx:alpine
docker.io/library/nginx:alpine
```

- **이미지 목록 확인**  
```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker images
                                                                                                    i Info →   U  In Use
IMAGE          ID             DISK USAGE   CONTENT SIZE   EXTRA
nginx:alpine   7cac34a4d72b         63MB             0B
```

## 7-2. 컨테이너 : 실행/중지/목록 확인 (예: docker ps, dockr ps -a)

- **컨테이너 실행** hello-world

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete
Digest: sha256:1e4c05b897a751332ce85c7e9449da2d93514ea2dea6dbb0e2753c9e949b2bfa
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

- **Nginx 웹서버 컨테이너 실행** 

  다운로드한 `nginx:alpine` 이미지를 기반으로 백그라운드에서 동작하는 웹서버 컨테이너를 생성 및 실행하였습니다.

  -d (Detached mode): 컨테이너를 백그라운드(화면 뒤쪽)에서 실행하여 터미널을 계속 사용할 수 있게 합니다. 
  
  --name my-nginx-container: 컨테이너의 이름을 my-nginx-container로 명시적으로 지정하여 관리하기 쉽게 만듭니다. 
  
  -p 8080:80: 호스트(내 PC)의 8080 포트를 컨테이너 내부의 80 포트(Nginx 기본 포트)와 연결(포트 매핑)하여 외부에서 접속할 수 있도록 합니다.


```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker run -d --name my-nginx-container -p 8080:80 nginx:alpine
c384b6b43fc854092df79038bbb1345a1693c401c5fc42ecf4a7f5d135a9f290
```

- **컨테이너 목록 확인** 

현재 실행 중인 컨테이너 목록을 확인합니다. (docker ps)

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
c384b6b43fc8   nginx:alpine   "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-container
```

모든 컨테이너 목록 확인 (정지된 컨테이너 등 포함) (docker ps -a)  
처음 실행했던 hello-world도 함께 보여지는 것을 확인할 수 있습니다.

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                     NAMES
c384b6b43fc8   nginx:alpine   "/docker-entrypoint.…"   4 minutes ago   Up 4 minutes   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-container
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                      PORTS                                     NAMES
c384b6b43fc8   nginx:alpine   "/docker-entrypoint.…"   6 minutes ago    Up 6 minutes                0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-container
c9bc242148a5   hello-world    "/hello"                 15 minutes ago   Exited (0) 15 minutes ago                                             unruffled_merkle
```

- **컨테이너 중지** (docker stop)

docker stop 명령어를 사용하여 컨테이너를 중지합니다.  
docker ps 명령어로 현재 실행중인 컨테이너 목록을 확인하였고,  
현재 실행중인 컨테이너가 없는 것이 확인됩니다.

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker stop my-nginx-container
my-nginx-container
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 7-3. 운영 : 로그 확인 (예: docker logs), 리소스 확인 (예: docker stats) 

- **로그 확인 (docker logs)**

컨테이너의 로그를 확인하기 위해 중지된 컨테이너를 다시 실행합니다.


기존 컨테이너를 다시 실행하는 명령어는 docker start 입니다.
(docker run 명령어를 사용하면 새로운 컨테이너가 처음부터 생성되며 실행되기 때문에 docker start 명령어를 사용합니다.)

컨테이너를 다시 실행하고 (docker start),  
제대로 실행되었는지 확인한 후 (docker ps)  
로그를 확인합니다. (docker logs)

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker start my-nginx-container
my-nginx-container
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
c384b6b43fc8   nginx:alpine   "/docker-entrypoint.…"   23 minutes ago   Up 21 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my-nginx-container
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker logs my-nginx-container
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/11/17 16:04:35 [notice] 1#1: using the "epoll" event method
2026/11/17 16:04:35 [notice] 1#1: nginx/1.31.8
2026/11/17 16:04:35 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2026/11/17 16:04:35 [notice] 1#1: OS: Linux 6.18.58.1-microsoft-standard-WSL2
2026/11/17 16:04:35 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/11/17 16:04:35 [notice] 1#1: start worker processes
2026/11/17 16:04:35 [notice] 1#1: start worker process 30
2026/11/17 16:04:35 [notice] 1#1: start worker process 31
2026/11/17 16:04:35 [notice] 1#1: start worker process 32
2026/11/17 16:04:35 [notice] 1#1: start worker process 33
2026/11/17 16:04:35 [notice] 1#1: start worker process 34
2026/11/17 16:04:35 [notice] 1#1: start worker process 35
2026/11/17 16:04:35 [notice] 1#1: start worker process 36
2026/11/17 16:04:35 [notice] 1#1: start worker process 37
2026/11/17 16:04:35 [notice] 1#1: start worker process 38
2026/11/17 16:04:35 [notice] 1#1: start worker process 39
2026/11/17 16:04:35 [notice] 1#1: start worker process 40
2026/11/17 16:04:35 [notice] 1#1: start worker process 41
2026/11/17 16:04:35 [notice] 1#1: start worker process 42
2026/11/17 16:04:35 [notice] 1#1: start worker process 43
2026/11/17 16:04:35 [notice] 1#1: start worker process 44
2026/11/17 16:04:35 [notice] 1#1: start worker process 45
2026/11/17 16:04:35 [notice] 1#1: start worker process 46
2026/11/17 16:04:35 [notice] 1#1: start worker process 47
2026/11/17 16:04:35 [notice] 1#1: start worker process 48
2026/11/17 16:04:35 [notice] 1#1: start worker process 49
2026/11/17 16:04:35 [notice] 1#1: start worker process 50
2026/11/17 16:04:35 [notice] 1#1: start worker process 51
2026/11/17 16:04:35 [notice] 1#1: start worker process 52
2026/11/17 16:04:35 [notice] 1#1: start worker process 53
2026/11/17 16:19:38 [notice] 1#1: signal 3 (SIGQUIT) received, shutting down
2026/11/17 16:19:38 [notice] 31#31: gracefully shutting down
2026/11/17 16:19:38 [notice] 30#30: gracefully shutting down
2026/11/17 16:19:38 [notice] 32#32: gracefully shutting down
2026/11/17 16:19:38 [notice] 33#33: gracefully shutting down
2026/11/17 16:19:38 [notice] 32#32: exiting
2026/11/17 16:19:38 [notice] 33#33: exiting
2026/11/17 16:19:38 [notice] 31#31: exiting
2026/11/17 16:19:38 [notice] 30#30: exiting
2026/11/17 16:19:38 [notice] 46#46: gracefully shutting down
2026/11/17 16:19:38 [notice] 50#50: gracefully shutting down
2026/11/17 16:19:38 [notice] 46#46: exiting
2026/11/17 16:19:38 [notice] 50#50: exiting
2026/11/17 16:19:38 [notice] 32#32: exit
2026/11/17 16:19:38 [notice] 31#31: exit
2026/11/17 16:19:38 [notice] 33#33: exit
2026/11/17 16:19:38 [notice] 30#30: exit
2026/11/17 16:19:38 [notice] 46#46: exit
2026/11/17 16:19:38 [notice] 50#50: exit
2026/11/17 16:19:38 [notice] 51#51: gracefully shutting down
2026/11/17 16:19:38 [notice] 51#51: exiting
2026/11/17 16:19:38 [notice] 43#43: gracefully shutting down
2026/11/17 16:19:38 [notice] 52#52: gracefully shutting down
2026/11/17 16:19:38 [notice] 52#52: exiting
2026/11/17 16:19:38 [notice] 43#43: exiting
2026/11/17 16:19:38 [notice] 47#47: gracefully shutting down
2026/11/17 16:19:38 [notice] 45#45: gracefully shutting down
2026/11/17 16:19:38 [notice] 47#47: exiting
2026/11/17 16:19:38 [notice] 34#34: gracefully shutting down
2026/11/17 16:19:38 [notice] 45#45: exiting
2026/11/17 16:19:38 [notice] 34#34: exiting
2026/11/17 16:19:38 [notice] 51#51: exit
2026/11/17 16:19:38 [notice] 41#41: gracefully shutting down
2026/11/17 16:19:38 [notice] 52#52: exit
2026/11/17 16:19:38 [notice] 45#45: exit
2026/11/17 16:19:38 [notice] 43#43: exit
2026/11/17 16:19:38 [notice] 44#44: gracefully shutting down
2026/11/17 16:19:38 [notice] 41#41: exiting
2026/11/17 16:19:38 [notice] 44#44: exiting
2026/11/17 16:19:38 [notice] 40#40: gracefully shutting down
2026/11/17 16:19:38 [notice] 37#37: gracefully shutting down
2026/11/17 16:19:38 [notice] 47#47: exit
2026/11/17 16:19:38 [notice] 37#37: exiting
2026/11/17 16:19:38 [notice] 40#40: exiting
2026/11/17 16:19:38 [notice] 34#34: exit
2026/11/17 16:19:38 [notice] 42#42: gracefully shutting down
2026/11/17 16:19:38 [notice] 53#53: gracefully shutting down
2026/11/17 16:19:38 [notice] 42#42: exiting
2026/11/17 16:19:38 [notice] 53#53: exiting
2026/11/17 16:19:38 [notice] 35#35: gracefully shutting down
2026/11/17 16:19:38 [notice] 37#37: exit
2026/11/17 16:19:38 [notice] 35#35: exiting
2026/11/17 16:19:38 [notice] 40#40: exit
2026/11/17 16:19:38 [notice] 35#35: exit
2026/11/17 16:19:38 [notice] 53#53: exit
2026/11/17 16:19:38 [notice] 44#44: exit
2026/11/17 16:19:38 [notice] 41#41: exit
2026/11/17 16:19:38 [notice] 42#42: exit
2026/11/17 16:19:38 [notice] 36#36: gracefully shutting down
2026/11/17 16:19:38 [notice] 48#48: gracefully shutting down
2026/11/17 16:19:38 [notice] 49#49: gracefully shutting down
2026/11/17 16:19:38 [notice] 38#38: gracefully shutting down
2026/11/17 16:19:38 [notice] 36#36: exiting
2026/11/17 16:19:38 [notice] 48#48: exiting
2026/11/17 16:19:38 [notice] 38#38: exiting
2026/11/17 16:19:38 [notice] 49#49: exiting
2026/11/17 16:19:38 [notice] 39#39: gracefully shutting down
2026/11/17 16:19:38 [notice] 39#39: exiting
2026/11/17 16:19:38 [notice] 39#39: exit
2026/11/17 16:19:38 [notice] 49#49: exit
2026/11/17 16:19:38 [notice] 48#48: exit
2026/11/17 16:19:38 [notice] 38#38: exit
2026/11/17 16:19:38 [notice] 36#36: exit
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 34
2026/11/17 16:19:38 [notice] 1#1: worker process 34 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 44 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 37
2026/11/17 16:19:38 [notice] 1#1: worker process 30 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 31 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 37 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 38 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 40 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 41 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 45 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 46 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 50 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 51 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 53 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 51
2026/11/17 16:19:38 [notice] 1#1: worker process 33 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 32
2026/11/17 16:19:38 [notice] 1#1: worker process 32 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 35 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 47 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 35
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 43
2026/11/17 16:19:38 [notice] 1#1: worker process 43 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 36
2026/11/17 16:19:38 [notice] 1#1: worker process 36 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 48 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 52
2026/11/17 16:19:38 [notice] 1#1: worker process 49 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: worker process 52 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 42
2026/11/17 16:19:38 [notice] 1#1: worker process 42 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: signal 29 (SIGIO) received
2026/11/17 16:19:38 [notice] 1#1: signal 17 (SIGCHLD) received from 39
2026/11/17 16:19:38 [notice] 1#1: worker process 39 exited with code 0
2026/11/17 16:19:38 [notice] 1#1: exit
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: IPv6 listen already enabled
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/11/17 16:27:46 [notice] 1#1: using the "epoll" event method
2026/11/17 16:27:46 [notice] 1#1: nginx/1.31.8
2026/11/17 16:27:46 [notice] 1#1: built by gcc 15.2.0 (Alpine 15.2.0)
2026/11/17 16:27:46 [notice] 1#1: OS: Linux 6.18.58.1-microsoft-standard-WSL2
2026/11/17 16:27:46 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2026/11/17 16:27:46 [notice] 1#1: start worker processes
2026/11/17 16:27:46 [notice] 1#1: start worker process 22
2026/11/17 16:27:46 [notice] 1#1: start worker process 23
2026/11/17 16:27:46 [notice] 1#1: start worker process 24
2026/11/17 16:27:46 [notice] 1#1: start worker process 25
2026/11/17 16:27:46 [notice] 1#1: start worker process 26
2026/11/17 16:27:46 [notice] 1#1: start worker process 27
2026/11/17 16:27:46 [notice] 1#1: start worker process 28
2026/11/17 16:27:46 [notice] 1#1: start worker process 29
2026/11/17 16:27:46 [notice] 1#1: start worker process 30
2026/11/17 16:27:46 [notice] 1#1: start worker process 31
2026/11/17 16:27:46 [notice] 1#1: start worker process 32
2026/11/17 16:27:46 [notice] 1#1: start worker process 33
2026/11/17 16:27:46 [notice] 1#1: start worker process 34
2026/11/17 16:27:46 [notice] 1#1: start worker process 35
2026/11/17 16:27:46 [notice] 1#1: start worker process 36
2026/11/17 16:27:46 [notice] 1#1: start worker process 37
2026/11/17 16:27:46 [notice] 1#1: start worker process 38
2026/11/17 16:27:46 [notice] 1#1: start worker process 39
2026/11/17 16:27:46 [notice] 1#1: start worker process 40
2026/11/17 16:27:46 [notice] 1#1: start worker process 41
2026/11/17 16:27:46 [notice] 1#1: start worker process 42
2026/11/17 16:27:46 [notice] 1#1: start worker process 43
2026/11/17 16:27:46 [notice] 1#1: start worker process 44
2026/11/17 16:27:46 [notice] 1#1: start worker process 45
```

- **리소스 확인 (예: docker stats)**

docker stats 명령어만 사용해도 무관하지만
docker stats --no-stream 명령어를 사용한 이유는  

docker stats 만 쳤을 경우 실시간으로 리소스 사용량 (CPU, 메모리 등)을 계속 새로고침하며 화면에 계속 띄워주기 때문입니다.

그래서 docker stats 만 사용하지 않고 현재 상태만 한번만(no stream) 측정한 결과를 보여줍니다.


```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker stats --no-stream
CONTAINER ID   NAME                 CPU %     MEM USAGE / LIMIT     MEM %     NET I/O         BLOCK I/O        PIDS
c384b6b43fc8   my-nginx-container   0.00%     19.51MiB / 23.47GiB   0.08%     1.17kB / 126B   9.99MB / 4.1kB   25
```

## 8. 컨테이너 실행 실습

### 8-1. hello-world 실행 성공 기록

```
sj_ee_6255@GNIA-C202-D06:~/E1-1/myapp$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete
Digest: sha256:1e4c05b897a751332ce85c7e9449da2d93514ea2dea6dbb0e2753c9e949b2bfa
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

### 8-2. ubuntu 컨테이너를 실행/내부 진입 후 간단 명령 수행


