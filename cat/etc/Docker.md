# Docker 실습
1. 도커 개발환경 설정

   - 도커 설치 

     - 도커는 윈도우 PRO이상 버전에서만 사용가능, 아닐 경우 TOOLBOX 설치

   - 설치 후 버전확인

     ```
     docker -v
     ```

   - 도커 컨테이너 실행 

     ```
     docker run [도커 컨테이너 명]
     ```

2. 도커 기본 명령어

   - 컨테이너 조회

     ```
     docker ps -a
     ```

   - 컨테이너 삭제

     ``` 
     docker rm [컨테이너 ID/Name]
     ```

   - 도커 이미지 조회

     ``` 
     docker images
     ```

   - Hello world 도커 이미지 삭제

     ```
     docker rmi [이미지 ID 또는 이미지명: TAG명]
     ```

3. 샘플 서비스를 이용한 도커 실습

   - Jenkins를 도커 컨테이너로 실행 및 실행중인지 확인

     ```
     docker run --name myjenkins -d -p 8080:8080 jenkins:2.60.3
     docker ps
     ```

   - Jenkins 서버 컨테이너의 bash 실행 후 컨테이너의 OS버전 확인

     ```
     docker exec -it myjenkins bash
     cat /etc/issue
     ```

   - 컨테이너 안에서 Admin 패스워드가 저장된 파일 확인 후 컨테이너 bash 종료

     ```
     cat /var/jenkins_home/secrets/initialAdminPassword
     exit
     ```

   - 컨테이너 안에 있는 패스워드 파일을 개발 PC로 복사하기

     ```
     docker cp myjenkins:/var/jenkins_home/secrets/initialAdminPassword ./ 
     dir
     ```

   - 컨테이너 삭제

     ```
     docker rm -f myjenkins
     ```

4. 기본 프로젝트 기반 도커 이미지 제작

   - Dockerfile작성(참고: https://kr.vuejs.org/v2/cookbook/dockerize-vuejs-app.html)

   - 프론트 코드용 도커 이미지 빌드

     ```
     dokcer build . -t front:0.1
     docker images
     ```

   - 이미지에 TAG추가하기

     ```
     docker tag front:0.1 front:latest 
     ```

   - 이미지에 TAG 삭제하기 

     ``` 
     docker rmi front
     ```

   - 도커로 프론트 실행 및 웹 브라우져로 접속

     ```
     docker run -it -p 8080:8080 --rm front:0.1
     ```

     
