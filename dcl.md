# TCL/DCL

## TCL

트랜잭션

![image-20210510152447380](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210510152447380.png)





## DCL (Data control Language)

### 권한(grant/revoke)

![image-20210510154949756](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210510154949756.png)

1번 실행시 mis/pw1 가진 유저 생성

2번 생성시 mis/new_pw 유저 변경.

3번 실행 시 mis2/pw2 만들려 했는데 에러 권한 없기 떄문에

4 번실행시 mis1에게 유저 만들 권한 줌(GRANT)

5번 실행 시 Mis2/pw2 유저 만듬.

6번 실행 시 권환 회수(REVOKE A 권한FROM B유저 )

7번 실행 시 Mis3/pw3 만들수 없음. 권한 뺏겨서.

8번 실행 시 drop user => 계정 날림

그런데 mis1이 많은 일을 했음. 

CASCADE 사용시 mis1이 한 일들 같이 삭제.

cascade 안하면 mis1 한 일들이 없어야 삭제 가능.



#### CREATE SESSION

CREATE SESSION 없으면 DB에 접속할 권한이 없다는 것 . 

```sql
GRANT CREATE SESSIN TO KMUMIS;
```

이제 DB접속 가능 

#### Object 권한 

테이블 생성할 수 있어야 데이터도 넣고 수정 할 수있기 때문에 create table 권한 필요

```sql
GRANT CREATE TABLE TO KMUMIS;
```

이제 테이블 만들어서 해당 테이블에 대해는 모든 권한 가능 (추가, 삭제 ,어떤 사용자에게 접속 허용여부 줄 수 있음)

DB에 접속하는 유저 A,B있다고 할 떄,

B가 A가 만든 테이블에 접속하자고 할 떄

테이블에 대한 권한을 A, 관리자가 만들 수 있음.

##### 관리자 계정으로 접속한 상태 

```sql
//GRANT 권한 ON 소유계정.테이블명 TO 계정명
GRANT SELECT ON kmumis.PLAYER TO Mis1;
//mis1 사용자에게 kmumis가 가지고 있는 PLAYER테이블의 select권한을 주어라
```

##### KMUMIS 계정으로 접속 한 상태

```sql
GRANT SELECT ON PLAYER TO Mis1;
//이 때는 자신의 테이블임을 알고 있기 때문에 바로 주면됨.
```

##### Mis1 계정에서 kmusis계정의 테이블 조회

```sql
SELECT * FROM KMUSIS.PLAYER ;
//테이블 생성자 계정 아니면 생성자 이름 찍어줘야함.
```



![image-20210510160409839](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210510160409839.png)

### ROLE을 이용한 권한 부여.

권한 주고 ,회수하는 것 편하게 !

ROLE: 권한들의 Package.

예를들면 a급 사용자에겐 a,b,c권한주고

b급 사용자에겐 a,b권한 주고 이런식.

![image-20210510160714069](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210510160714069.png)

```sql
CREATE MY_ROLE: 새로운 롤 만들기.
```

```sql
GRANT CREATE_SESSION, CREATE_TABLE TO MY_ROLE;
//각 권한 MY_ROLE에 부여.
```

```sql
GRANT MY_ROLE_TO mis1;
//mis1에 my_role부여.
```

role과 role을 합쳐서 새로운 role도 합칠 수 있음.

![image-20210510160812090](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210510160812090.png)

```
GRANT CONNECT ,DBA ,RESOURCE TO myid
```

를 통해 권한을 준 게 아니라  3가지 ROLE을 줬음.

ROLE권한 확인 위해 코드 실행하면

CONNECT ROLE에 있는 권한,

RESOURCE에 있는 각 권한 확인 가능.



