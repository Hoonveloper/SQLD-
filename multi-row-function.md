# multi-row  function 

## 집계함수

### GROUP BY

각 포지션 별로 몇명이 있고, 그 중 키값이 있는 선수는 몇 명이 있고, 그 의 평균값을 골라서 포지션별로 묶은 SQL문

```sql
SELECT POSITIION ,COUNT(*) 전체행수, COUNT(HEIGHT) 키건수, ROUND(AVG((HEIGHT),2) 평균키 
FROM PLAYER
GROUP BY POSITION;

SELECT POSITIION ,PLAYER_NAME ,COUNT(*) 전체행수, COUNT(HEIGHT) 키건수, ROUND(AVG((HEIGHT),2) 평균키 
FROM PLAYER
GROUP BY POSITION;
//PLAYER_NAME을 쓰게되면 출력 불가. 
//GROUPBY쓰면 집계함수와 그룹지은 컬럼만 올 수 있음.
```

![image-20210514024321369](https://user-images.githubusercontent.com/77804950/118400934-aa1c4700-b69e-11eb-8b18-db5d1849a21c.png)

GROUP BY에서 정의되지 않은 컬럼을 사용해서 에러가 나옴.  

2번 쿼리는 전체에 대해 그룹을 지은거라 GROUP BY생략된거라서 집계함수를 썼는지 구분 힘듬 => COUNT사용으로 아! 저게 집계함수구나 알 수 있음. 

![image-20210514025106845](https://user-images.githubusercontent.com/77804950/118400945-b1dbeb80-b69e-11eb-83eb-6444ece4335e.png)

GROUP BY절에서 조건을 주고 싶을 때 WHERE을 쓰면 에러 발생!

```
WHERE (AVG(HEIGHJT))>=180
```

이 부분에서 아직 GROUPBY가 실행이 안되었기 떄문에 HEIGHT가 뭔지 모름 => HAVING 사용

#### HAVING

![image-20210514025230433](https://user-images.githubusercontent.com/77804950/118400946-b2748200-b69e-11eb-8c3a-9884d156e18c.png)

```
SELECT POSITION ,ROUND(AVG(HEIGHT),2) 평균키 
FROM PLAYER
GROUP BY POSITION
HAVING MAX(HEIGHT) >=190;
```

#### 그룹핑 기준이 2개인 경우

EX) 학년과 성별 기준으로 그룹핑 하고 싶은 경우 ( 1학년 남자, 1학년 여자, 2학년 여자 ,남자 등 )

![image-20210514025847208](https://user-images.githubusercontent.com/77804950/118400989-e2bc2080-b69e-11eb-9ed9-c7be566766c2.png)





### ORDERB3Y

### SELECT 문의 실행 순서.

![image-20210515145836575](https://user-images.githubusercontent.com/77804950/118401007-f5cef080-b69e-11eb-92ba-4a36b12bc7c7.png)

![image-20210515150332223](https://user-images.githubusercontent.com/77804950/118401009-f7001d80-b69e-11eb-9076-839ce8fb9acb.png)

```
SELECT PLAYER_NAME ,HEIGHT, ORGNO, ROWNUM
FROM (SELECT PAYER_NAME , HEIGHT ,ORGNO, ROWNUM FROM PLAYER ORDERBY HEIGHT)
WHERE ROWNUM<4;

```

![image-20210515150848102](https://user-images.githubusercontent.com/77804950/118401010-f798b400-b69e-11eb-862a-4ca3eb44a0c0.png)

![image-20210515152101146](https://user-images.githubusercontent.com/77804950/118401043-15feaf80-b69f-11eb-812c-84107dc26a5c.png)

## 고급 집계 함수

### ROLLUP

![image-20210515152416361](https://user-images.githubusercontent.com/77804950/118401082-39c1f580-b69f-11eb-803c-43dd85e77beb.png)



![image-20210515153432625](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210515153432625.png)

어디까지 집계된 것이다 정도를 알려주는 컬럼.

GROUPING 이 1이면 그 컬럼에 대해 집계가 나온 것 .

전체 집계는 모두 1.

![image-20210515154131713](https://user-images.githubusercontent.com/77804950/118401086-3c244f80-b69f-11eb-9565-60abfa146faa.png)

ROLLUP과 CASE 문을 써서 1일경우 다른 값을 넣어주어

시각적으로 잘 보이게 해준다.

### CUBE

![image-20210515154321831](https://user-images.githubusercontent.com/77804950/118401087-3cbce600-b69f-11eb-842d-e0a19e0d8dea.png)

순서 무관

```
ROLLUP(A,B) UNION ROLLUP(B,A)
```

ROLLUP 순서를 바꿔서 합집합을 하면 CUBE구현이 가능.

### GROUPING SETS

![image-20210515154829165](https://user-images.githubusercontent.com/77804950/118401090-3d557c80-b69f-11eb-9e78-7c234ffb40ae.png)

![image-20210515155229447](https://user-images.githubusercontent.com/77804950/118401144-6f66de80-b69f-11eb-8929-4214b0345589.png)

```
DECODE(A,B,'VALUE1','VALUE2')
A와B의 값이 같으면 VALUE1, 다르면 VALUE2입력
```

![image-20210515155655444](https://user-images.githubusercontent.com/77804950/118401145-6fff7500-b69f-11eb-84b7-bb913f2e44ae.png)

JOB으로 GROUPBY를 하고 DNAME으로 GROUPBY를 해서 중복을 허용하는 UNION ALL 을 해줌. 

위,아래 중복된 값이 없기 때문에 UNION을 해줘도 됨.

(중복 확인 안해서 UNION ALL 이 훨씬 빠름)

### 비교

![image-20210515155933947](https://user-images.githubusercontent.com/77804950/118401146-6fff7500-b69f-11eb-8150-e310ac06b375.png)1.GROUP BY A,B

소규모 소계 X

2.GROUP BY ROLLUP(A,B) 

3.GROUP BY CUBE(A,B)

4.GROUP BY GROUPING SETS(A,B)

## 윈도우 함수

![image-20210515160406424](https://user-images.githubusercontent.com/77804950/118401147-70980b80-b69f-11eb-8227-cc690f421006.png)

![image-20210515160609058](https://user-images.githubusercontent.com/77804950/118401149-70980b80-b69f-11eb-9d96-7c06c5c0b6ff.png)

![image-20210515161046477](https://user-images.githubusercontent.com/77804950/118401152-71c93880-b69f-11eb-9e64-4340285f8502.png)

### 순위함수

![image-20210515161319607](https://user-images.githubusercontent.com/77804950/118401153-7261cf00-b69f-11eb-90cf-b85c8484909f.png)

```
SELECT JOB,ENAME,SAL,
		RANK() OVER (ORDER BY SAL DESC) AS ALL_RANK,
		RANK() OVER (PARTITION BY JOB ORDER BY SAL DESC) AS JOB_RANK
FROM EMP;
//PARTITION BY 사용하면 JOB별로 나눠서 랭크 맥임.
```

![image-20210515161740339](https://user-images.githubusercontent.com/77804950/118401124-6970fd80-b69f-11eb-9142-eef5538c1bf2.png)

### 집계 함수

![image-20210515162354410](C:\Users\hoonveloper\AppData\Roaming\Typora\typora-user-images\image-20210515162354410.png)

```
SELECT JOB,ENAME,SAL
MAX(SAL) OVER (PATITION BY JOB) JOB_MAX
FROM EMP
ORDER BY JOB,ENAME;
```

![image-20210515162907695](https://user-images.githubusercontent.com/77804950/118401126-6aa22a80-b69f-11eb-8eaf-86a874a33019.png)

1.파티션에서 그룹을 만들고

2.SAL내림차순으로 정렬.

3.그 그룹에서 나까지의 급여 총합 JOB_SUM에 넣어줌

만약 동점이 있으면 옆에 나란히 있다고 생각하기 

=> 값이 동일하면 동시에 반영![image-20210515163450911](https://user-images.githubusercontent.com/77804950/118401128-6bd35780-b69f-11eb-879f-bda4d652d522.png)

내 직종 위에서 바로 위,아래 출력

JOB으로 PARTITION 쳐놔서 JOB을 뛰어넘을 수는 없다.

![image-20210515163734614](https://user-images.githubusercontent.com/77804950/118401129-6c6bee00-b69f-11eb-9412-fa390919e003.png)

파티션 주지 않고 오름차순.

```sql
RANGE BETWEEN 100 PRECEDING AND 200 FOLLOWING
```

현재 값보다 -100~+200

MILLER씨의 경우는 1200~1500까지 몇 명 인거냐?

### 행 순서 함수

![image-20210515164746224](https://user-images.githubusercontent.com/77804950/118401130-6c6bee00-b69f-11eb-9f3b-37af08b22b0b.png)

DEPTNO 별로 나눠서 각 DEPT 마다 연봉 킹 뽑기.

![image-20210515165042081](https://user-images.githubusercontent.com/77804950/118401131-6d048480-b69f-11eb-923d-6207b997a623.png)

```
SELECT ENAME ,SAL,
		LEAD(SAL) OVER (ORDER BY SAL DESC),
		LAG(SAL) OVER (ORDER BY SAL DESC )
FROM EMP
WHERE JOB='SALESMAN';

```

### 비율 함수

![image-20210515165836307](https://user-images.githubusercontent.com/77804950/118401133-6d048480-b69f-11eb-909a-01dddb01c13d.png)

![image-20210515170136142](https://user-images.githubusercontent.com/77804950/118401134-6d9d1b00-b69f-11eb-99c7-0a4fecfa55e8.png)

0에서부터 시작한다 !

![image-20210515170517759](https://user-images.githubusercontent.com/77804950/118401136-6d9d1b00-b69f-11eb-8ee9-14d0cc38bd5e.png)

0초과 1이하의 값을 가짐.

동일 값은 큰 백분율 적용

![image-20210515170702715](https://user-images.githubusercontent.com/77804950/118401138-6e35b180-b69f-11eb-8b4f-3cffd32f0295.png)

딱 나누어 떨어지지 않을 경우

1. 1그룹에 다 떄려박는 경우
2. 위에있는 그룹부터 나누어 때려박는 경우



