# JOIN

## 표준 조인

E.F.CODD가 주장

SQL은 일반 집합연산자 4개 + 순수 관계연산자 4개가 필요.

### 일반 집합연산자

UNION->UNION(합집합)

INTERSECTION->INTERSECT (교집합)

DIFFERENCE -> EXCEPT(Oracle 에서 MINUS) 

PRODUCT -> CROSS JOIN 곱집합

### 순수 관계연산자

SELECT->WHERE( 행 고르기)

PROJECT -> SELECT (열 고르기)

(NATURAL JOIN) ->다양한 JOIN, 테이블 여러개 존재 하는데 1개로 합치는 것.

DIVIDE->현재 사용 X

## EQUI JOIN

두 개의 테이블 간 칼럼 값들이 서로 정확하게 일치하는 경우 사용되는 방법. ( = ) 

```sql
//문제 X EQUI JOIN 구문.
SELECT ENAME,DNAME FROM EMP,DEPT WHERE EMP.DEPTNO= DEPT.DEPTNO;

//에러 , DEPTNO은 중복칼럼이기 때문에 어디서 가져와야 되는지 모름.
//중복되지 않아도 테이블 명 쓰는것 권장
SELECT ENAME,DEPTNO,DNAME FROM EMP,DEPT WHERE EMP.DEPTNO= DEPT.DEPTNO;

//정상 작동.
SELECT EMP.ENAME,EMP.DEPTNO,DEPT.DNAME FROM EMP,DEPT WHERE EMP.DEPTNO=DEPT.DEPTNO;
```

### ALIAS 사용

테이블에도 ALIAS 사용 가능![image-20210511155725961](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511155725961.png)



셋 이상의 테이블의 조인은 실제로는 두 테이블 간 조인이 연쇄적으로 일어남. 

## NON-EQUI JOIN

EQUAL 이 아닌 조인

BETWENN , > >=, <= 등 



```sql
//해당 직원의 급여등급을 SALGRADE 테이블을 참고하여 몇등급인지 JOIN해서 나타내라.
SELECT E.ENAME 사원명 ,E.SAL 급여 , S.GRADE 등급
FROM EMP E, SALGRADE S
WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL;
```



## INNER JOIN 

![image-20210511161248933](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511161248933.png)  



```sql
SELECT E.NAME , E.DEPTNO, E.SAL, D.DNAME 
FROM EMP E, DEPT D
WHERE E.DEPTNO=D.DEPTNO AND E.SAL>2000;
```

- 조인 조건과 일반 조건이 혼용되어 가독성 떨어짐. -> 명시적 조인의 필요성

- 명시적 조인에서 조인관련 조건은 ON절에, 그 외의 조건은 WHERE절에 기술.

  ```sql
  SELECT E.NAME ,E.DEPTNO, E.SAL, D.DNAME
  FROM EMP E INNER JOIN DEPT D
  ON E.DEPTNO=D.DEPTNO
  WHERE E.SAL > 2000;
  ```

  

### NATURAL JOIN 

- inner join의 특수한 경우
  - natural inner join =natural join
- 두 테이블 간 동일한 이름을 갖는 모든 칼럼에 대해 equl join 수행.
  - 칼럼 간 데이터 타입도 동일해야 함.
  - 별도의 조인 칼럼 및 조건을 지정할 수 없음.
- 조인의 대상이 되는 칼럼에는 접두사(테이블 명 또는 alias ) 사용 불가.

``` 
SELECT EMPNO,ENAME,DEPTNO,DNAME 
FROM EMP NATURAL JOIN DEPT;
// 두 코드는 같은 코드 내츄럴/이너조인 차이
SELECT EMTNO,ENAME ,D.DEPTNO,DNAME
FROM EMP E INNERJOIN DEPT D,
ON E.DEPTNO=D.DEPTNO;
```



![image-20210511163522199](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511163522199.png)

중복되는 컬럼 DEPTNO에 대해서 달라지게 된다.

NATURAL은 이름이 중복되는 걸 알고있기 때문에 맨 앞에 중복되는 컬럼 뽑아줌.

INNER은 중복되는 컬럼을 2개로 나눠서 뽑아줌.

QUIZ) A 테이블 컬럼 8개, B테이블 컬럼 3개 NATRUAL 조인 했는데 컬럼 9개. 중복컬럼 갯수는? -> 2개.

### USING 조건절

- ON절에의 '=' 대신 USING 절 사용 가능

  - ON절에서는 괄호 생략 가능, USING에서는 불가.

- 접두사 사용 불가. 

  ```sql
  SELECT E.NAME, E.DEPTNO, D.DNAME
  FROM EMP E JOIN DEPT D
  ON (E.DEPTNO= D.DEPTNO);
  // 같은 코드
  SELECT E.NAME ,E.DEPTNO, D.DNAME
  FROM EMP JOIN DEPT 
  USING (DEPTNO)
  //괄호 생략 안되고 접두사 사용 불가.
  ```

  ![image-20210511164821027](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511164821027.png)

natural 조인과의 차이: 

만약 중복된 컬럼이 3개가 있다

모든 컬럼에 대해 조인 -> natrual join

선택 컬럼에 대해 조인-> using 

## Outer JOIN 

![image-20210511165810028](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511165810028.png)

INNER 조인 INNER 생략 하듯이

LEFT, RIGHT ,FULL에 대해선 OUTER 생략 가능.

### FULL OUTER JOIN

LEFT와 RIGHT를 UNION 하면 얻을 수있음

## CROSS JOIN

![image-20210511171302280](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511171302280.png)



## QUIZ

![image-20210511171621240](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511171621240.png)

1. 3

2. 12

3. 4

4. 4 -> 함정 쉬움

   KIM B CS

   LEE C BIO

   PART B CS

   NULL A MIS

5. 5

   KIM B CS

   LEE C BIO

   PARK B CS

   NULL A MIS

   CHOI D NULL

   

   아우터조인은 교집합 구하고 빠진것 더하기.



## SELF JOIN

- 동일 테이블 사이의 조인

  - FROM 절에 동일 테이블이 두 번 이상 나타남

- 테이블 식별을 위해 반드시 ALIAS를 사용해야함

  - 동일한 테이블을 개념적으로 서로 다른 두 개의 테이블로 사용함

  - 예) FROM EMP E JOIN EMP M

    ![image-20210511174341618](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511174341618.png)

```
SELECT E.EMPNO, E.ENAME, E.MGRNO, M.NAME
FROM EMP E LEFT JOIN EMP M
ON E.MGRNO = M.EMPNO
```

## 계층형 데이터

SELF JOIN의 확장. 

### 순방향 계층

![image-20210511180627889](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511180627889.png)

![image-20210511180827463](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511180827463.png)

### 역방향 계층![image-20210511181211405](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511181211405.png)

역방향 인 경우 LEVEL과 CONNECT_BY_ISLEAF도 반대로 생각해야한다.

## 집합 연산자. 

#### UNION ALL (합집합)

![image-20210511183608596](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511183608596.png)

팀 ID 가 K06이거나 GK인 선수들 뽑아와서 합집합 K06이고 GK인 선수들 까지 포함



#### INTERSECT (교집합)

![image-20210511183740333](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511183740333.png)

#### MINUS(차집합)

![image-20210511183906828](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511183906828.png)

1. 집합연산자 이용
2. 조건연산 이용



### 집합 연산과 ALIAS

![image-20210511184436850](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210511184436850.png)

1. 컬럼에 Alias 줘서 orderby 할 때 사용 -> O
2. 컬럼에 Alias 줬지만 orderby할 때 별칭 사용 x -> O
3. 컬럼에 Alias줬고 집합연산 할 때 사용 ->o
4. 컬럼에 Alias 줬고 집합연산 할 때 별칭 사용 x -> ERROR

집합연산을 사용했을 때 ORDERBY는 제일 마지막에 와야한다.

ORDERBY에서 사용 할 수 있는 컬럼은 첫번쨰테이블이다. 

컬럼명은  본명 말고 ALIAS를 사용해야 한다. 