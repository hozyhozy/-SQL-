## SQL 기본 명령어
* 데이터 정의어
* 데이터 조작어
* 데이터 제어어
* 트랜젝션 제어어


![스크린샷 2023-08-13 오후 7 42 20](https://github.com/hozyhozy/-SQL-/assets/123252821/cf02179b-ec5b-4f8a-bd76-2d358f04ce1e)



![스크린샷 2023-08-13 오후 7 42 39](https://github.com/hozyhozy/-SQL-/assets/123252821/2b229889-ca1e-4840-b589-2d53b670d4fe)



## 데이터 정의어 (DDL)
- 데이터 타입 : 각 테이블은 각 열마다 반드시 1가지 데이터 타입으로 정의되어야함
- 제약조건
  - PK (Primary Key): 중복 X, NOT NULL
  - NOT NULL : NULL 값을 허용하지 않음


![image](https://github.com/hozyhozy/-SQL-/assets/123252821/19284d85-0bde-4a26-90cd-c1834de00d94)
*데이터를 효율적으로 사용하기 위해 타입을 미리 정해놓는 것으로 이해하면 됨*


![스크린샷 2023-08-13 오후 7 44 37](https://github.com/hozyhozy/-SQL-/assets/123252821/e967464c-80d5-400a-b7f8-25093bf205fb)
*제약조건*



![스크린샷 2023-08-13 오후 7 44 51](https://github.com/hozyhozy/-SQL-/assets/123252821/4f441857-27f9-4d68-b78e-6ee1033a43cc)



``` sql
/* Practice 이름으로 데이터베이스 생성*/
CREATE DATABASE Practice;

/* Practice 데이터베이스 사용*/
USE Practice;


/***************테이블 생성(Create)***************/
/* 회원테이블 생성 */ -- 작성 순서: 열이름, 데이터 타입, 제약조건
CREATE TABLE 회원테이블 (
회원번호 INT PRIMARY KEY,
이름 VARCHAR(20), -- 20 바이트
가입일자 DATE NOT NULL,
수신동의 BIT
);


  
  
/***************테이블 열 추가*******************/  
/* 성별 열 추가 */  
ALTER TABLE 회원테이블 ADD 성별 VARCHAR(2);
 
 
/***************테이블 열 데이터 타입 변경***************/  
/* 성별 열 타입 변경 */  
ALTER TABLE 회원테이블 MODIFY 성별 VARCHAR(20);
 
 
/***************테이블 열 이름 변경**************/  
/* 성별 -> 성 열 이름 변경 */  
ALTER TABLE 회원테이블 CHANGE 성별 성 VARCHAR(2);
 
 
/***************테이블명 변경**************/  
/* 테이블명 변경 */  
ALTER TABLE 회원테이블 RENAME 회원정보;
 
/* 회원테이블 조회 --> 이름이 변경되었기 때문에 조회되지 않음*/
SELECT  *
FROM  회원테이블;
  
/* 회원정보 조회 */
SELECT  *
FROM  회원정보;   
  
  
/***************테이블 삭제**************/  
/* 테이블 삭제 */  
DROP TABLE 회원정보;
 
/* 회원정보 조회 --> 삭제되었기 때문에 조회되지 않음*/
SELECT  *
  FROM  회원정보;
```
