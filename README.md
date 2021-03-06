# ThePirates Coding Test
더파이러츠 입사지원 코딩테스트

## 1. 설치 및 환경설정 가이드
    ### 개발 환경
    - Java 8
    - Spring Boot 2.4.2 - maven 사용
    
    ### 데이터 베이스
    - H2(in memory)
    
    -- 데이터 베이스 환경설정 -- 
    1. http://localhost:8082 접속
    2. jdbc url 에 jdbc:h2:~/test 를 입력해 사용자의 디렉토리에 test.mv.db 파일 생성 확인
    3. 스프링 구동후 http://localhost:8081(현재 api사용 포트) 접속
    4. jdbc url 은 jdbc:h2:mem:test 로 인메모리 테스트 환경으로 접속
    
    ### 사용 라이브러리 / 프레임워크
    - Spring Boot DevTools
    - Spring Web
    - Lombok
    - MyBatis Framework
    
    ### API POST 테스트 환경
    - Postman
    
---------------------

## 2. 테이블 생성 SQL

    ### schema.sql
    
    drop table if exists market CASCADE;
    create table market
    (
        id INTEGER generated by default as identity,
        name varchar (255),
        owner varchar (255),
        description varchar (255),
        level INTEGER (3),
        address varchar (255),
        phone varchar (255),
        holidays varchar (255),

        primary key (id)
    );



    drop table if exists time CASCADE;

    create table times
    (
        id INTEGER generated by default as identity,
        day varchar (255),
        open varchar (255),
        close varchar (255),
        marketId INTEGER,
        primary key (id),
        foreign key (marketId) references market(id)
    );

---------

## 3. API 사용 가이드

    ### A. 점포 추가 API
    
    - Postman을 사용하여 post 요청 환경 테스트
    - Post 방식 
    - 1개의 json타입의 데이터를 받아서 DB에 인서트
    
    - http://localhost:8081/marketRegister
    - {
     "name": "인어수산",
     "owner": "장인어",
     "description": "인천소래포구 종합어시장 갑각류센터 인어수산",
     "level": 2,
     "address": "인천광역시 남동구 논현동 680-1 소래포구 종합어시장 1층 1호",
     "phone": "010-1111-2222",
     "businessTimes": [
     {
     "day": "Tuesday",
     "open": "13:00",
     "close": "23:00"
     },
     {
     "day": "Wednesday",
     "open": "09:00",
     "close": "18:00" },
     {
     "day": "Thursday",
     "open": "09:00",
     "close": "23:00"
     },
     {
     "day": "Friday",
     "open": "09:00",
     "close": "23:00"
     }
     ]
    }
    
    
    - {
    "name": "해적수산",
     "owner": "박해적",
     "description": "노량진 시장 광어, 참돔 등 싱싱한 고퀄 활어 전문 횟집",
     "level": 1,
     "address": "서울 동작구 노량진동 13-8 노량진수산시장 활어 001",
     "phone": "010-1234-1234",
     "businessTimes": [
     {
     "day": "Tuesday",
     "open": "09:00",
     "close": "24:00"
     },
     {
     "day": "Wednesday",
     "open": "09:00",
     "close": "24:00"
     },
     {
     "day": "Thursday",
     "open": "09:00",
     "close": "24:00"
     }, {
     "day": "Friday",
     "open": "09:00",
     "close": "24:00"
     }
     ]
    }
    
-----

    ### B. 점포 휴무일 등록 API
    
    - 1개의 json타입의 데이터를 받아서 DB에 인서트
    - Post 방식으로 요청 
    - http://localhost:8081/holidays
    
    - {
     "id": 1,
     "holidays": [
     "2021-01-21",
     "2021-01-22"
     ]
    }
    
    - {
     "id": 2,
     "holidays": [
     "2021-01-22",
     "2021-01-23"
     ]
    }
    
-----

    ### C. 점포 목록 조회 API
    
    - http://localhost:8081/markets
    - Get 방식으로 요청을 처리
    - db에 있는 모든 정보를 가져와 응답
    - 요청한 순간의 날짜와 시간을 받아 데이터 처리

-----

    ### D. 점포 상세 정보 조회 API
    
    - http://localhost:8081/markets/1
    - http://localhost:8081/markets/2
    
    - @Pathvariable를 사용
    - Get 방식으로 요청을 처리
    - id를 통해 1개의 데이터요청에 응답
    - 요청한 순간의 날짜와 시간을 받아 데이터 처리

-----

    ### E. 점포 삭제 API
    
    - http://localhost:8081/marketDelete/1
    
    - @Pathvariable를 사용
    - Get 방식으로 요청을 처리
    - id를 통해 1개의 데이터요청에 응답
    - 외래키로 연결되어 있는 데이터를 Transaction을 통해 처리
    
