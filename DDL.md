# DDL

Data Definition Language

데이터 정의어 

DML은 인스턴스를 다루는 반면 DDL은 테이블을 다룬다.

## 테이블 생성

### 테이블 생성 규칙

####  테이블 명 

객체를 의믜할 수 있는 이름으로 단수형을 권고(권장)

다른테이블의 이름과 중복되지 않아야 함.(필수)

#### 칼럼명

한테이블 내에서는 칼럼명 중복되면 안됨, 다른 테이블에서는 OK

테이블 생성시 각 칼럼들은 괄해 내에 서 콤마로 구분. 

#### 테이블 명 , 컬럼명

사전에 정의된 예약어 (SELECT, FROM)등은 사용 불가

문자,숫자,일부기호만 허용

 반드시 문자로 시작해야함.

제약조건명: 다른 제약조건의 이름과 중복되지 않아야 함. 



#### 제약조건

제약조건을 주면 이름을 부여해야 한다.

그 이름을 명시적으로 사용자가 부여할 수도 있고, 제약 조건명 없이 제약 조건 설정할 수 있음.(시스템이 만들어줌)

  

```
CREATE TABLE STADIUM(
STADIUM_ID CHAR(3) NOT NULL,
STADIUM_NAME VARCHAR(4) NOT NULL,
HOMETEAM_ID CHAR(3),
SEAT_COUNT NUMBER,
ADDRESS VARCAR2(60),
DDD VARCHAR2(3),
TEL VARCHAR2(10),
CONSTRAINT STADIUM_PK PRIMARY KEY(STADIUM_ID)
//CONSTRAINT 는 컬럼 만드는건 아님.
//CONSTRAINT 제약조건 이름 / PRIMARY KEY로 지정(어떤걸 지정할건지)
);
```

 Foreign Key 제약조건 걸기

```
CREATE TABLE TEAM(
TEAM_ID CHAR(3) NOT NULL,
....
...

CONSTRAINT TEAM_PK PRIMARY KEY (TEAM_ID),
CONSTRAINT TEAM_FK FOREIGN KEY (STADIUM_ID) REFERENCES STADIUM(STADIUM_ID)
//CONSTRAINT 제약조건명 FOREIGN KEY(어떤걸) REFERENCES 테이블명(그 테이블의 컬럼명)
//여기서 STADIUM 테이블이 먼저 생성되어 있어야 함. 
);
```

##### 제약조건의 지정

```
//묵시적
CREATE TABLE PLAYER1 (
PLAYER_ID CHAR(7) PRIMARY KEY,
PLAYER_NAME VARCHAR2(20) NOT NULL,
NICKNAME VARCHAR2(20) UNIQUE,
HEIGHT NUMBER(3) CHECK(HEIGHT >=150 AND HEIGHT<=200),
TEAM_ID CHAR(3) REFERENCES TEAM(TEAM_ID) , // <=FOREIGN KEY
);
//명시적 (묵시적 + CONSTRAINT 제약조건명만 붙음 )
CREATE TABLE PLAYER2(
PLAYER_ID CHAR(7) CONSTRAINT P2_PK_ID PRIMARY KEY ,
PLAYER_NAME CONSTRAINT P2_NN_NAME NOT NULL,
NICKNAME VARCHAR2(20) CONSTRAINT P2_UN_NICK UNIQUE,
HEIGHT NUMBER(3) CONSTARINT P2_CK_HEIGHT CHECK (HEIGHT>=150 AND HEIGHT<=200),
TEAM_ID CHAR(3) CONSTRAINT P2_FK_TID REFERENCES TEAM(TEAM_ID)
);
//뒷북.
뒤에 다 뫃아놓은것.
```

제약조건 모아놓은 테이블 (시스템에서 만들어 놓음).

```
SELECT * FROM ALL_CONSTRAINT
```



##### FK제약 조건의 옵션.

FK를 참조하는 부모테이블의 값이 바뀌면 참조 무결성의 제약이 위배된다.

어떤 명령에 의해 동작할 것인가?

ON UPDATE, ON DELETE 각 경우마다 ACTION을 지정해준다.



RESTRICT (Default) : 문제가 될 것 같으면 삭제 또는 갱신 불허

NO ACTION: RESTRICT와 동일하게 동작

CASCADE : 기본키가 삭제되면 외래키로 갖는 레코드도 삭제

​				  : 기본키가 갱신되면 이를 참조하는 왜래키를 새로운값으로 업데이트

아무일도 없었다는 듯이 처리해줌.

SET NULL:

기본키가 삭제 또는 갱신되면 이름 참조하는 왜래키를 NULL로 수정.

![image-20210509193822136](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210509193822136.png)



### 기존 테이블을 활용한 테이블 생성

```
CREATE TABLE PLAYER_TEMP AS SELECT * FROM PLAYER;
//PLAYER에서 가져온 정보들로 PLAYER_TEMP 테이블 생성 .
```

다만 CONSTRAINT 제약조건은 NOT NULL 만 복제

FK .PK .UNIQUE , CHECK등은 수동으로 추가 해야함.



## 테이블 변경 

 

### 칼럼 추가(ADD)

추가한 컬럼은 테이블의 맨 마지막에 추가됨.

```
ALTER TABLE PLAYER_TEMP ADD (ADDRESS VARCHAR2(20));
```

### 칼럼 삭제(DROP COLUMN)

삭제 후 최소 하나의 컬럼이 남아있어야 함

```
ALTER TABLE PLAYER_TEMP DROP COLUMN ADDRESS;
```

### 칼럼명 변경 (RENAME COLUMN A TO B)

A라는 컬럼명을 B로 바꾼다.

이름은 바뀌었지만 해당 칼럼의 모든 정의가 그대로 유지됨. (CHAR(7) ,NOT NULL 등)

```
ALTER TABLE PLAYER_TEMP RENAME COLUMN PLAYER_ID TO PLAYER_NEW_ID;
```

### 칼럼의 정의 수정(MODIFY )

**이미 입력되어 있는 값에 영향을 미치는 변경은 허용하지 X**

데이터 타입 변경 -> 테이블에 아무 행도 없거나 NULL일 때만 가능.(손님 오기 전 바꾸는 느낌)

칼럼의 크기 변경 -> 칼럼의 크기 확대는 가능. 축소는 없거나, NULL, 저장된 값 수용할 수 있는 크기로 축소 가능.

DEFAULT 값 추가 및 수정 -> 추가 및 수정 이후 삽입되는 행에만 영향을 미침 (이전값에는 영향 X ) 



NOT NULL 제약조건 추가 : 테이블에 아무 행도 없거나 해당 칼럼에 NULL이 없을 때 가능

제거는 항상 가능

```
ALTER TABLE PLAYER_TEMP MODIFY(PLAYER_NAME NULL); 
```



##### ADD/DROP을 이용한 CONSTRAINT 추가 삭제

```
//추가
ALTER TABLE PLAYER_TEMP ADD CONSTRAINT PLAYER_TEMP_FK FOREIGN KEY (TEAM_ID) REFERENCES TEAM(TEAM_ID);
//삭제
ALTER TABLE PLAYER_TEMP DROP CONSTRAINT PLAYER_TEMP_FK;
```

## 테이블 이름 변경 (RENAME)

```
RENAME PLAYER_TEMP TO OLD_PLAYER;
//이름 변경.
SHOW TABLES;
```

## 테이블 삭제 (DROP)

테이블 삭제함으로써 참조 무결성 위반하는 문제 때문에 오류나옴

```
DROP TABLE TEAM CASCADE CONSTRAINT ;
//제약조건을 함께 삭제한다는 뜻.
```

