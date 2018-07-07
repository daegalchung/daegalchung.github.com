---
layout: post
title:  "User Service API 서버-1"
date:   2018-07-07 14:00:00 +000
categories: Contents
tags: user service, api server
---

# User Service API Server

> - Language : Java 8
> - Framework : Spring (Boot 2.0)
> - Database : MySQL 5.7 (docker)
> - O/S : macOS High Sierra (10.13.5)
> - Author : 한대호

## 오늘의 목표 
1. Docker로 Mysql 설치
2. JPA 연동

### 1. Docker로 Mysql 설치

#### 1. MySql Docker 이미지 내려받기
우선 docker 이미지를 받기 위해 아래와 같이 실행합니다.
```
docker pull mysql:{version}
``` 
{version} 부분에 원하는 버전을 넣어주면 됩니다. 가장 최신 버전을 설치하고 싶으면 lastest를 넣어주면 됩니다. 최신 버전은 작성일 기준(18.7.7) 8.0이 받아집니다.
저는 5.7버전을 설치하기 위해 아래와 같이 입력하여 이미지를 내려받았습니다.

```
docker pull mysql:5.7
```

#### 2. MySql Docker 이미지 확인 
docker 이미지가 다운로드 되면 아래의 명령어로 제대로 받아졌는지 확인해봅니다.
```
docker images
```
![images 확인](https://daegalchung.github.io/images/docker/docker_image.png)

#### 3. MySql Container 생성
```
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD={password} --name {container_name} mysql:5.7
```
위의 명령어는 내려받은 이미지를 통해 컨테이너를 생성하고 동작시킵니다.

옵션에 대한 설명은 아래와 같습니다.
>* _-p 3306:3306_ : 호스트의 3306 port와 컨테이너의 3306포트를 연결합니다. 
>* _-e MYSQL_ROOT_PASSWORD={password}_ : 컨테이너를 생성하면서 환경변수를 지정한다. 이를 통해 mysql root 비밀번호를 설정할 수 있다. 
>* _-name {container name}_ :  컨테이너 이름을 지정할 수 있다.

아래 명령을 입력하여 생성된 mysql 컨테이너가 동작하고 있는지를 확인 할 수 있습니다.
```
docker ps -a
```
![container 동작 확인](https://daegalchung.github.io/images/docker/docker_ps.png)


#### 4.Container 접속

```
docker exec -i -t mysql_dgc bash
```
위의 명령어로 Container의 bash쉘에 접속할 수 있습니다.

#### 5. Database 계정 생성
mysql에 접속한 뒤 개발을 위해 다음과 같이 mysql 계정을 생성하습니다.

```
create database dgc;
grant all privileges on dgc.* to 'dgc'@'%' identified by '{password}';
```