# docker-test

도커 연습과정 기록

## 도커설치

> <https://hub.docker.com/>

- 현재 os(윈도우)에 도커설치
- 예제 따라하기
- 첫번째 이미지 빌드 후 도커허브에 push

## 간단하게 시작하기

> <https://docs.docker.com/get-started/>

- `docker image ls` : 로컬에 다운로드한 이미지 확인
- `docker container ls --all` : 로컬에 생성된 컨테이너 전체목록
  - `--all` 옵션을 빼면 현재 실행중인 컨테이너만 표시됨
- 도커파일 작성

### 도커파일

파일명 : `DockerFile`

```txt
# 부모 이미지
FROM node:current-slim

# 최초 작업디렉토리 지정(이미지의 파일시스템, 호스트의 파일시스템이 아님)
WORKDIR /usr/src/app

# 호스트에서 이미지내의 현재위치로 파일을 복사함 (이 경우, /usr/src/app/package.json)
COPY package.json .

# npm 설치명령 실행
RUN npm install

# 도커 컨테이너가 실행될때, 8080 포트 리스닝한다는 뜻인듯..
EXPOSE 8080

# 컨테이너가 실행되고 자동으로 실행되는 초기 실행 명령인듯..
# docker run 으로 컨테이너 실행할때, 추가인자를 전달하면 여기서 정의한 값은 무시되고, 추가인자로 전달한 명령이 실행된다고 함
CMD [ "npm", "start" ]

# 호스트에서 이미지로 나머지 소스파일 전부 카피
COPY . .
```

- `CMD` 와 `ENTRYPOINT` 의 차이 : <https://bluese05.tistory.com/77>

### 이미지 빌드 / 컨테이너 실행

- `docker image build -t bulletinboard:1.0 .` : 이미지빌드
- `docker container run --publish 8000:8080 --detach --name bb bulletinboard:1.0` : 컨테이너 실행
  - `--publish` : 호스트의 8000포트를 이미지의 8080으로 연결
  - `detach` : 컨테이너를 백그라운드 실행
  - `name` : 컨테이너 이름지정
- `docker container rm --force bb` : 컨테이너 삭제
  - `--force` : 실행중인 컨테이너 제거

### 제작된 이미지 도커허브로 공유

> <https://docs.docker.com/get-started/part3/>

- 도커허브에 공유된 이미지는, 컨테이너실행시, 이미지를 미리 다운받지 않아도, 자동으로 이미지를 다운 후 실행해줌
