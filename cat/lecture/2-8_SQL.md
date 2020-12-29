# 웹 프로그래밍 - SQL

### SQL(Structured Query Language)

- 데이터를 보다 쉽게 검색하고 추가, 삭제, 수정 같은 조작을 할 수 있도록 고안된 컴퓨터 언어
- 관계형 데이터베이스에서 데이터를 조작하고 쿼리하는 표준 수단
- DML (Data Manipulation Language): 데이터를 조작하기 위해 사용
  - INSERT, UPDATE, DELETE, SELECT 등
- DDL (Data Definition Language): 데이터베이스의 스키마를 정의하거나 조작하기 위해 사용
  - CREATE, DROP, ALTER 등
- DCL (Data Control Language) : 데이터를 제어하는 언어, 권한을 관리하고, 테이터의 보안, 무결성 등을 정의
  - GRANT, REVOKE 등



### Database 생성하기

> mysql -uroot -p //root 계정으로 데이터베이스 관리 시스템에 접속하겠다

- 이때, Can't connect to MySQL server on 'localhost' (10061)라는 에러가 뜬다면 MySQL이 실행되고 있지 않은것, 검색 - 서비스 - MySQL찾아서 시작해준다.

>  create database DB이름;//데이터베이스 생성



### Database 사용자 생성과 권한 주기

- 해당 데이터베이스를 사용하는 계정을 생성
- db이름 뒤의 * 는 모든 권한을 의미, @’%’는 어떤 클라이언트에서든 접근 가능하다는 의미이고, @’localhost’는 해당 컴퓨터에서만 접근 가능하다는 의미

```mysql
create user 'connectuser'@'localhost' identified by 'connect123!@#';
create user 'connectuser'@'%' identified by 'connect123!@#';
grant all privileges on connectdb.* to 'connectuser'@'localhost';
grant all privileges on connectdb.* to 'connectuser'@'%';
flush privileges; //DBMS에게 적용을 하라는 의미로, 반드시 실행해줘야 된다.
```

- db 사용하기

> use connectdb;