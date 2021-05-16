# PL-SQL

SQL안에서 프로그램을 사용하겠다.

기본적으로는 host language안에서 sql을 쓰는데

기본적인건 sql안에서 host language를 사용할 수 있게 하자.

## PROCEDURE

![image-20210515171333078](https://user-images.githubusercontent.com/77804950/118401139-6ece4800-b69f-11eb-9c7d-624138fb105f.png)

![image-20210515171804410](https://user-images.githubusercontent.com/77804950/118401141-6ece4800-b69f-11eb-8e2b-59d01bb51973.png)

![image-20210515172142934](https://user-images.githubusercontent.com/77804950/118401143-6f66de80-b69f-11eb-8c99-6fb1e2e7af53.png)

![image-20210515172204003](https://user-images.githubusercontent.com/77804950/118401327-3aa75700-b6a0-11eb-8345-c6a5b9c978a4.png)

![image-20210515172905582](https://user-images.githubusercontent.com/77804950/118401314-31b68580-b6a0-11eb-82a7-b577fd8ba3cf.png)

![image-20210515173038198](https://user-images.githubusercontent.com/77804950/118401316-324f1c00-b6a0-11eb-8553-f5029ef72962.png)==

```
INTO cnt 는
갯수를 뽑아서 cnt변수에 넣어준다.
(이미 있는 부서가 몇개 있는지)

```

![image-20210515173656589](https://user-images.githubusercontent.com/77804950/118401317-32e7b280-b6a0-11eb-9501-4bf5e319b5c3.png)

rslt를 varchar2(30)로 선언.

rslt 는 procedure 실행하고 난 결괏값.

## 사용자 정의 함수

![image-20210515173912938](https://user-images.githubusercontent.com/77804950/118401318-33804900-b6a0-11eb-8028-f330c8d39079.png)

이 함수 자체가 RETURUN 값.

PROCEDURE은 RSLT를 정의해줘서 그게 OUT값이라고 생각.

RETURN이 PROCEDURE와의 차이점.

호출 할떄도 excute하느냐 이름만 호출하느냐 차이

실제 로직은 BEGIN~END

## Trigger

procedure와 함수는 비슷하지만 트리거는 다름.

많이 사용하면 성능저하

![image-20210515174447723](https://user-images.githubusercontent.com/77804950/118401319-33804900-b6a0-11eb-8b76-831631e8055d.png)

ex) 고객 물건 주문시 포인트 줘야하고 고객 등급을 올려줘야함.

이름은 있지만 이름으로 호출하는게 아니라 조건만 맞으면 바로 실행 

![image-20210515175122223](https://user-images.githubusercontent.com/77804950/118401320-3418df80-b6a0-11eb-8c5b-4514e74d37c8.png)

![image-20210515175320306](https://user-images.githubusercontent.com/77804950/118401362-5e6a9d00-b6a0-11eb-8ee1-d4262eec07d4.png)

![image-20210515175723379](https://user-images.githubusercontent.com/77804950/118401363-5f9bca00-b6a0-11eb-81cd-b9d0f7004750.png)

마우스 하나 팔리면 order insert

sales 테이블은 두번 이상 팔린 물건 update

오늘 처음 팔린 물건은 insert

![image-20210515175940040](https://user-images.githubusercontent.com/77804950/118401364-60346080-b6a0-11eb-85aa-18d5ce5ac156.png)

procedure 와 함수는 생성하고 호출 해야하는데

trigger는 생성만 하고 자동으로 실행되서 호출 필요 x



order라는 테이블에

각 행마다 

 insert가 일어나고 이후에 트리거 해라.



