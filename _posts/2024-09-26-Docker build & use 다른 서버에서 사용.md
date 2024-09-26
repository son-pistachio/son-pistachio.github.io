---
title: Docker build & use 다른 서버에서 사용
date: 2024-09-26
categories: [Docker]
tags: [Docker, 도커]
---
  

특정 폴더 루트 예시:  /home/tmp/ml  
# Dockerfile 생성  
먼저, 도커 이미지 생성을 원하는 특정 폴더(/home/tmp/ml)에 Dockerfile을 만들어 해당 폴더를 도커 이미지에 포함.  
```
# Python 3.9을 사용하는 베이스 이미지 설정 (필요한 환경에 맞게 선택)
FROM python:3.9-slim

# 컨테이너 내 작업 디렉토리 설정
WORKDIR /app

# 로컬 폴더의 파일들을 도커 이미지로 복사
COPY . /app

# 필요한 패키지를 설치하는 명령어 (선택사항)
RUN pip install -r requirements.txt

# 컨테이너 실행 시 실행할 명령어 (선택사항)
CMD ["python", "main.py"]
```
위 필수 세가지만 수행 했음.



# 도커 이미지 빌드  
터미널에서 해당 폴더로 이동한 후, 다음 명령어로 도커 이미지 빌드.
```
cd /home/tmp/ml
docker build -t my_image .
```  
my_image는 이미지의 이름



# 도커 이미지 저장 및 전송
다음 명령어로 도커 이미지를 .tar 파일로 저장.
```
docker save -o my_image.tar my_image
```  
생성된 my_image.tar 파일을 다른 서버로 전송. 이를 위해 scp 명령어 사용.
```
scp my_image.tar user@other_server_ip:/path/to/destination
```  



# 다른 서버에서 도커 이미지 로드
다른 서버로 접속한 후, 해당 서버에서 도커 이미지 로드.
```
docker load -i /path/to/destination/my_image.tar
```
도커 이미지가 해당 서버에 로드



# 도커 컨테이너 실행
이미지 로드가 완료되면 도커 컨테이너를 실행할 수 있다. 다음 명령어로 컨테이너 실행.
```
docker run -d -p 8000:8000 my_image
```
여기서 -p 8000:8000은 호스트와 컨테이너 간의 포트 매핑을 의미. 필요에 따라 포트 변경 가능.  

아래 코드로 도커 접근

```
docker run -i -t my_image /bin/bash
```


# 전체 프로세스 요약
**Dockerfile 작성:** 도커 이미지를 만들 폴더에 Dockerfile을 작성.  
**도커 이미지 빌드:** docker build 명령어로 도커 이미지를 빌드.  
**도커 이미지 저장 및 전송:** docker save로 이미지를 파일로 저장하고 scp로 전송.  
**다른 서버에서 이미지 로드:** docker load로 다른 서버에 이미지를 로드.  
**컨테이너 실행:** docker run으로 컨테이너를 실행.  
