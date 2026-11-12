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
### 4-1. 현재 위치 확인

```
$ pwd
/home/sj_ee_6255/projects/myapp
```

### 4-2. 목록 확인(숨김 파일 포함) (ls -la)

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

### 4-3. 디렉토리 생성 및 이동 (mkdir, cd 명령어 사용)

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ mkdir test_dir
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ cd test_dir
sj_ee_6255@GNIA-C202-B06:~/projects/myapp/test_dir$ pwd
/home/sj_ee_6255/projects/myapp/test_dir
```

### 4-4.빈 파일 생성 (touch 명령어 사용)
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

### 4-5. 파일 내용 입력 및 확인 (echo, cat)

파일 내용을 입력하고 출력해 확인합니다.

```
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ echo "test 123456789" > test.txt
sj_ee_6255@GNIA-C202-B06:~/projects/myapp$ cat test.txt
test 123456789
```

### 4-6. 파일 복사 (cp)

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

### 4-7. 파일 이동 및 이름 변경 (mv)

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

### 4-8. 디렉토리 및 파일 삭제 (rm -rf, rm)

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

abcdefg

