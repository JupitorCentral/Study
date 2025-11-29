---
tags:
  - Study/Database
---

### Chapter 14 데이터베이스 프로그래밍 기초


#### 데이터베이스와 DBMS

데이터베이스 -> 데이터가 저장되는 공간
DBMS -> DataBase Management System -> 데이터베이스를 관리하는 시스템 -> Oracle, MySql, Postgresql








## RDBMS vs NoSQL

### RDBMS

데이터가 테이블 형식 - 즉 row, column 기반의 정형화된 구조로 데이터를 저장하는 DB 종류
데이터의 구조가 미리 정의되어 있고 (schema), SQL (Structured Query Language) 로 데이터를 조회 삽입 수정 삭제함
ACID 원칙 (Atomicity, Consistency, Isolation, Durability) 를 지켜 복잡한 트랜잭션 처리와 데이터 무결성을 보장

#### ACID principle

##### Atomicity


##### onsistency


##### Isolation


##### Durability


### NoSQL (Not Only SQL)

데이터를 전통적인 방식이 아닌 구조 (Document, Key-Value 형태 등)조로 데이터를 저장

메인 메모리에 데이터를 저장, 필요할떄만 디스크에 저장

#### 대표적인 구조

##### Document-based

##### key-value

##### Wide-column

##### Graph


#### 왜 NoSql 이 필요해졌는지 ?


#### Connection Pool
