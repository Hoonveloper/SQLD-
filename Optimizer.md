# Optimizer

![image-20210516150815159](https://user-images.githubusercontent.com/77804950/118401365-60346080-b6a0-11eb-925a-0d417f8d5793.png)

![image-20210516151224644](https://user-images.githubusercontent.com/77804950/118401366-60ccf700-b6a0-11eb-9216-d1be64f73ff9.png)

## 동작 구조

![image-20210516151546989](https://user-images.githubusercontent.com/77804950/118401367-61658d80-b6a0-11eb-9cfb-2044d454540a.png)

ex) 심부름을 할 때 약국 ,빵집의 거리를 고려해서 어떻게 가는게 효율적인가? 생각하는 것이 옵티마이저

규칙 기반 옵티마이저는 이미 규칙이 다 정해져있음 따라서 바로 실행계획이 나옴.

비용기반 옵티마이저는 통계가 필요함. 딕셔너리를 사용해 비용을 예측하고 총 비용이 젤 덜 드는 방법을 추천.

## 규칙 기반 옵티마이저

![image-20210516151808080](https://user-images.githubusercontent.com/77804950/118401368-61658d80-b6a0-11eb-9809-34777f0152f9.png)

1~14는 인덱스스캔을 사용.

15는 full scan 사용 (가장 느림. 최악의 경우)

![image-20210516152112165](https://user-images.githubusercontent.com/77804950/118401369-61fe2400-b6a0-11eb-99e2-905dcb9cfa5a.png)

규칙 1. 바로 rowid 이용해서 테이블 행에 접근

규칙 4. pk를 이용해 접근하고 ,인덱스의 row를 추출해 행에 접근

ex) 학번을 통해 rowid 추출해 접근.

규칙8. 인덱스 매칭률

### 인덱스 매칭률

![image-20210516152510735](https://user-images.githubusercontent.com/77804950/118401370-6296ba80-b6a0-11eb-8ae6-9e8b77b1995c.png)

인덱스 1 , 3개의 칼럼으로 구성. 2/3

인덱스 2, 2개의 칼럼으로 구성. 2/2

인덱스 2가 더 높아서 2채택

![image-20210516152631145](https://user-images.githubusercontent.com/77804950/118401371-6296ba80-b6a0-11eb-9188-523f966ea51e.png)

1. 순서 연속 x  그래서 '시'까지만 매칭 인정 => 1/3
2. 구는 =(equal)이 아니므로 끊어짐 => 1/3
3. like에서 끊어져서 2/3
4. 2/3
5. 3/3

![image-20210516153010460](https://user-images.githubusercontent.com/77804950/118401372-632f5100-b6a0-11eb-88a8-aeebaaca4a53.png)

## 비용기반 옵티마이저

![image-20210516153230613](https://user-images.githubusercontent.com/77804950/118401373-632f5100-b6a0-11eb-83d5-8bf7d4260998.png)

![image-20210516153503525](https://user-images.githubusercontent.com/77804950/118401374-63c7e780-b6a0-11eb-90cc-bba3a6fb1114.png)

대안계획 생성 - 무엇을 만들것인가를 sql문에서 알려주면

그 무엇을 만드는 계획을 다양하게 만들어냄.

단, 가능한 모든 계획을 생성하지는 않아서 최적 대안이 누락될 수 있음.

![image-20210516153856870](https://user-images.githubusercontent.com/77804950/118401375-63c7e780-b6a0-11eb-8a9b-ad300224d14b.png)

## full table scan

![image-20210516154145912](https://user-images.githubusercontent.com/77804950/118401376-64607e00-b6a0-11eb-8bdc-8bc712c4a656.png)

## 인덱스

![image-20210516155212533](https://user-images.githubusercontent.com/77804950/118401378-64607e00-b6a0-11eb-84df-bcf18d23eaa0.png)

### b트리 인덱스

![image-20210516155752772](https://user-images.githubusercontent.com/77804950/118401379-64f91480-b6a0-11eb-85e7-248463ebe90f.png)

## 조인기법

### Nested Loop 조인

![image-20210516160750457](https://user-images.githubusercontent.com/77804950/118401380-6591ab00-b6a0-11eb-840a-c4ebc0bdee1d.png)

연봉 1억 이상인 직원, 차 배기량 2000cc미만. 

첫번쨰 선행 테이블에서 연봉 1억이상 직원 100명

100번 후행 테이블 돔

### Sort Merge 조인

![image-20210516161314537](https://user-images.githubusercontent.com/77804950/118401381-6591ab00-b6a0-11eb-8765-c659fdff1191.png)

정렬하고 합친다는 뜻

정렬은 조인 대상 기준 컬럼을 기준으로.

###  Hash Join

![image-20210516162042907](https://user-images.githubusercontent.com/77804950/118401382-662a4180-b6a0-11eb-8962-089ef358deb0.png)

![image-20210516162253438](https://user-images.githubusercontent.com/77804950/118401383-662a4180-b6a0-11eb-8ab6-20020d587807.png)