# DML(Database Manipulation Language)

## INSERT 

일반적으로는 한 건의 레코드를 추가. 

2가지 유형이 있음

1. 특정 컬럼에만 값 넣기

   ```sql
   INSERT INTO PLAYER (PLAYER_ID, PLAYER_NAME) VALUES ('0000001','KIM');
   //PLAYER_ID, PLAYER_NAME 컬럼에 값을 넣음
   ```

2. 전체 컬럼에 값 넣기

   ```sql
   INSERT INO PLAYER VALUES ('KIM','00001',NULL,NULL);
   ```

   주의 할 점: 값을 칼럼 순서대로 넣어야함

   빈 값은 꼭 NULL 혹은 ''로 표시

여러건의 레코드를 추가하기 위해서는

INSERT ALL 사용 .

예시 1. 처음에 값 많이 넣을 때.

예시 2. TABLE 1에서 값을 뽑아 TABLE 2, 3 에 분할해서 저장할 때

주의: INSERT ALL 사용 시**SELECT**를 꼭 사용해야함

```sql
INSERT ALL 
INTO TABLE2 VALUES (ID,NAME)
INTO TABLE3 VALUES (ID, SALARY)
SELECT ID,NAME,SALARY FROM TABLE1;
```

SELECT 할 거 없을 경우 DUAL 사용.

```sql
INSERT ALL
INTO PLAYER VALUES('손흥민','K01')
INTO PLAYER VALUES('김지훈' ,'K02')
SELECT * FROM DUAL; 
```



## DELETE

테이블에 존재하는 전체 레코드 삭제

```sql
DELETE STADIUM; // 
DELETE FROM STADIUM
//두개 같은 뜻
```

WHERE절을 사용하여 특정 레코드 삭제

```sql
DELETE STADIUM
WHERE STADIUM_ID='TP1';
//STADIUM_ID가 1인 레코드 STADIUM테이블에서 삭제
```

## UPDATE

테이블에 존재하는 전체 레코드의 값 변경

```sql
UPDATE PLAYER SET POSITION = 'GK';
//POSITION칼럼 모든 POSITION값이 GK로 바뀜.
```

DELETE 와 마찬가지로 WHERE절을 줘서 특정 레코드 변경

```sql
UPDATE STADIUM SET STADIUM_NAME ='우리 경기장' WHERE STADIUM_NAME ='임시경기장'
```

### ROWNUM을 이용한 채번

PLAYER 테이블 내에 ROW_ID라는 빈 커럼 추가.

```sql
ALTER TABLE PLAYER ADD (ROW_ID NUMBER);

//PLAYER 테이블 수정하고 그 테이블에는 ROW_ID컬럼 추가하고 그 컬럼은 숫자가 들어간다.

```

ROWNUM을 ROW_ID로 복사.

```sql
UPDATE STADIUM SET ROW_ID =ROWNUM;
```

