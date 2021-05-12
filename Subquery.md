# 서브쿼리

![image-20210512163307109](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512163307109.png)

![image-20210512164042935](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512164042935.png)

## 단일행 서브쿼리

![image-20210512165226523](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512165226523.png)

![](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512165400475.png)

```sql
UPDATE EMP SET MGR= (SELECT MGRNO FROM EMP WHERE EMPNO= 7369)
WHERE EMPNO=7499
```

![image-20210512170144223](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512170144223.png)

왼쪽: 서브쿼리 결과가 1개이므로 정상실행

오른쪽: 서브쿼리 결과가 2개이므로 런타임에러

동명이인이 존재할 수 있으므로 에러는 안났지만 왼쪽도 바람직한 쿼리가 아님. 

## 다중행 서브쿼리

결과건수 **2건** 이상 일 가능성 있을 때 사용

다중행 비교 연산자와 함께 사용.

![image-20210512170550840](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512170550840.png)

### IN :

 OR여러개 붙인 것과 같은 역할

예시) 등번호가 15인 선수들의 신장과 같은 선수들을 뽑아라

![image-20210512170855425](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512170855425.png)

### ALL: 

결과의 모든값을 만족

예시: 등번호가 15인 선수들의 신장들 보다 큰 신장을 가진 선수들을 뽑아라 

![image-20210512171136852](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512171136852.png)

### ANY: 

결과의 한 값이라도 만족하면 된다.

예시:  X> ANY(1,2,3,4,5) 일 경우 X>1이면 OK



### Exists

존재하는지 여부만 관심. 갯수는 상관X

예시) 등번호가 15인 선수들의 신장이 존재하면 , 전부 다 출력 

```
SELECT PLAYER_NAME , HEIGHT ,BACK_NO
FROM PLAYER
WHERE EXISTS (SELECT HEIGHT FROM PLAYER WHERE BACK_NO =15);
```

![image-20210512171747806](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512171747806.png)

Exists는 조건문이 중요! (where).

## 연관 서브쿼리

![image-20210512172208436](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512172208436.png)

Q. 해당 직원의 급여가 직원이 속한 부서의 평균 급여보다 높은 것만 뽑아라

![image-20210512172907543](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512172907543.png)

## 다중컬럼 서브쿼리

단일컬럼은 지금껏 한 것 HEIGHT -> 한 컬럼 가지고 비교.

![image-20210512173828558](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512173828558.png)

 

## 다중컬럼 다중행 서브쿼리

![image-20210512174150804](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512174150804.png)

위에는 '마니치가' 1명이므로 단일행이다. 단일행 이므로 <, =와 같은 비교연산자를 쓸 수 있고 HEIGHT,POSITION 두개의 행을 비교하므로 단일행, 다중컬럼 서브쿼리.

밑에는 '김충호'가 2명이므로 다중행이다. 그러므로 다중행 비교 연산자인 IN을 써주어야 하고 마찬가지로 다중컬럼이다. ->그래서 ERROR

![image-20210512174451595](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512174451595.png)

```sql
SELECT PLAYER_NAME,DEPTNO,SAL
FROM EMP
WHERE (DEPTNO,SAL) IN (SELECT DEPTNO,MAX(SAL) FROM EMP GROUP BY DEPTNO);
```

DEPTNO와 SAL을 비교하므로 다중컬럼

부서가 3개라 3개의 행이라 =이 아니라 IN 사용해야함.

## 스칼라 서브쿼리

![image-20210512175221379](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512175221379.png)

1번 쿼리: EMP 멤버들을 돌면서 해당 멤버들이 속한 부서 이름 출력

2번쿼리:부서코드가 'SALES'인 부서 멤버들을 출력.



![image-20210512175722527](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512175722527.png)

```sql
SELECT EMPNO, ENAME, (SUBSTR(SELECT DNAME FROM DEPT WHERE DEPTNO=A.DEPTNO),1,3) D_NAME
FROM EMP A;

```

## View 뷰 

![image-20210512180337708](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512180337708.png)

![image-20210512180918280](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512180918280.png)

V_PLAYER_TEAM에 있는 VIEW의 정의가 FROM에서 REWRITE 된 것.



###  계층적 뷰 생성.

![image-20210512181553121](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512181553121.png)

```sql
CREATE VIEW V_EMP_DEPT AS 
SELECT ENAME,EMPNO,DEPTNO, DNAME 
FROM EMP JOIN DEPT
USING (DEPTNO);

SELECT* FROM V_EMP_DEPT //VIEW 확인

CREATE VIEW V_EMP_DEPT2 AS
SELECT ENAME,DNAME 
FROM V_EMP_DEPT2;

```

### VIEW의 장점: 

독립성: 테이블 구조가 바뀌어도 뷰를 다시 만들면댐

편리성: 가독성 좋음

보안성:민감한 정보 제외하고 뷰 생성 가능.

### 인라인 뷰 (inline View)

![image-20210512182723956](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512182723956.png)

```sql
//1번째 쿼리 중
SELECT EMPNO, ENAME FROM EMP ORDER BY MGR
```

위 구문은 시스템에 저장 X

실행 순간에만 암시적으로 생성되어 DB에 저장 X

동적할당처럼 잠깐 쓰는것.

인라인 뷰의 SELECT문에서 정의된 컬럼만 메인쿼리에서 사용 가능. 

```sql
SELECT MGR
FROM (SELECT EMPNO,ENAME FROM EMP ORDER BY MGR); //MGR이 inline view에서 정의되지 않아서 메인쿼리에서 사용 불가
```

![image-20210512183502359](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210512183502359.png)

```sql
SELECT E.EMPNO,E.ENAME,E.SAL,D.DNAME 
FROM (SELECT EMPNO, ENAME ,SAL,DEPTNO FROM EMP WHERE SAL>2000) E, DEPT D
WHERE E.DEPTNO=D.DEPTNO;
```

E.DEPTNO는 인라인 뷰에서 가져왔는데 SELECT뒤에 컬럼에 정의해주지 않았으므로 에러가 난다. 따라서 DEPTNO를 SELECT에서 정의해주면 된다.