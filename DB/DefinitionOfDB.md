# DB의 정의

## DB (Database)
    데이터: 컴퓨터 안에 기록되어있는 숫자
    데이터베이스: 비휘발성 저장장치에 저장되는 영속된 데이터의 집합
- 데이터베이스의 데이터는 비휘발성 저장장치(하드디스크나 플래시메모리(SSD) 등)에 저장

## DBMS (Database Management System)
    데이터베이스 관리 시스템

### 클라이언트/서버 모델
    사용자의 조작에 따라 요청을 전달하는 '클라이언트'
    해당 요청을 받아 처리하는 '서버'로 이루어진 모델

## 1) 사용 목적
### (1) 생산성
- 데이터 검색, 추가, 삭제, 갱신등 기본기능 제공

### (2) 기능성
- 복수의 유저의 요청에 대응 및 대용량의 데이터를 저장 등 다양한 기능 제공

### (3) 신뢰성
- 많은 요청에 대응할 수 있도록 하드웨어를 여러 대로 구성 가능
- 클러스터 구성 또는 스케일 아웃 : 컴퓨터 여러대 두고 소프트웨어를 통해 확장성과 부하분산을 구현

## 2) 종류
### (1) 관계형 데이터베이스
- 행과 열을 가지는 표 형식 데이터(2차원)를 정하는 방식
- SQL로 데이터를 다루는 데이터베이스

### (2) 계층형 데이터베이스
- 폴더와 파일 등의 계층 구조로 데이터를 저장하는 방식

### (3) XML 데이터베이스
- 태그를 이용해 마크업 문서를 작성할 수 있게 정의
- 해당 DBMS에서는 SQL명령 사용불가

### (4) 키-밸류 스토어(KVS)
- 키와 그에 대응하는 값(밸류)로 단순한 형태의 데이터를 저장하는 방식
- NoSQL(Not only SQL)에서 생겨난 데이터베이스

## SQL
    관계형 데이터베이스에서 사용하는 언어
    - RDBMS에 따라 방언이 있다 -> 표준 SQL를 사용하는 편이 좋음 

### 1) 종류
#### (1) DML (Data Manipulation Language)
- 데이터 조작 명령어
- 데이터 추가 및 삭제, 갱신 등

#### (2) DDL (Data Definition Language)
- 데이터를 정의하는 명령어

#### (3) DCL (Data Control Language)
- 데이터를 제어하는 명령어
- 트랜잭션 제어, 데이터 접근권한을 제어하는 명령어 등

<br>

## 참조
[SQL 첫걸음 - 아사이 아츠시](https://product.kyobobook.co.kr/detail/S000001057649)

2023-07-14