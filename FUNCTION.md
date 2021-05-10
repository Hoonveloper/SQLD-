# Function

## 함수의 유형

### 생성 주체

내장함수 : min,max,length 등 이미 시스템에서 만들어진 함수.

사용자 정의 함수: 우리가 만드는 함수 

### 적용 범위

#### 단일 행 함수(single-row function)

한 행에 대해서 적용하는 함수. 

```
SELECT PLAYER_NAME , LENGTH(PLAYER_NAME) AS 길이 FROM PLAYER;
```

각 행에 대해 개별적으로 작용하고 그 결과 반환.

SELECT , WHERE, ORDERBY 절에 사용 가능.

##### 문자열함수

LOWER, UPPER ,ASCII 함수는 딱 보면 알 수 있음. 

CONCAT ('A','B')=> 'AB' (합치는거)

SUBSTR('ABCDE',2,2) => 'CD' 2번째 IDX부터 2개

TRIM ('X' FROM 'XXYZXZXX' ) => 'YZXZ' 앞뒤에서 지정 문자 제거

LTRIM ('XXYZXZXX','X') =>'YZXZXX' 왼쪽에서부터제거.

##### 숫자형함수

SIGN(50) => 1 (양수) , -123 => -1 (음수)  , 0=> 0 (0)

CEIL (38.2) => 39 숫자보다 큰 정수

FLOOR(38.2) =? 38 숫자보다 작은정수

ROUND(123.1234,2) => 셋자리에서 반올림해서 2자리까지만 표현.

ROUND(123.1274,2) => 123.123 

2번쨰 INDEX값 없으면 정수로 나타냄 소숫점 첫째자리에서 반올림

TRUNC(123.1234,2) => 123.12 , 2번째 이후  소숫점 쳐냄

##### 변환형 함수

명시적 데이터 유형 변환: TO_NUMBER, TO_CHAR , TO_DATE

암시적 데이터 유형 변환 : 시스템이 자동으로 데이터 변환 대신 성능저하 및 에러 발생 가능.

```
//날짜=> 문자열
SELELCT TO_CHAR(SYSDATE) //Default 타입으로 변환
SELECT TO_CHAR (SYSDATE,'YYYY/MM/DD');
SELECT TO_CHAR(SYSDATE,'YYYY.MM.DD.HH24.MI.SS')
```

```
SELECT EXTRACT(YEAR FROM '20170123') FROM DUAL; => 에러
SELECT EXTRACT( YEAR FROM TO_DATE('20170123','YYYY/MM/DD')) FROM DUAL; =>변환해야함
```

##### CASE 표현

![image-20210510135719626](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210510135719626.png)

```
DECODE(POSITION, 'GK','골키퍼','DF','수비수','그 외') FROM PLAYER;
```

##### NULL 관련 함수

NULL은 공백도 아니고 0도아니고 값이 없는 것.

NULL을 포함한 모든 산술 연산의 결과는 NULL (논리연산 ,TRUE OR NULL -> TRUE)

```
NVL(POSITION,'없음')
//PISITION이있으면 POSITION, 없으면 오른쪽
//NULL을 특정값으로 바꾼다
NULLIF(POSITION,'GK')
//두 값이 같으면 NULL, 같지 않으면 왼쪽 값 반환. 
```

NULL을 특정값으로 바꾼다

![image-20210510143033145](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210510143033145.png)





#### 다중 행 함수(multi-row function)

