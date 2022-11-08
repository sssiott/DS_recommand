# 이커머스 로그데이터를 활용한 추천데이터시스템 제작
* 사용 툴
python 3.8  
pandas  
surprise


## 데이터셋
### 구성
| 컬럼명        | 설명                           |
| ------------- | ------------------------------ |
| event_time    |                                |
| event_type    | view, cart, purchase로 구성    |
| product_id    |                                |
| category_id   |                                |
| category_code | .을 기준으로 최대 3가지로 분류 |
| brand         |                                |
| price         |                                |
| user_id       |                                |
| user_session  |                                |
----
### 전처리
* 결측치
    * session
        2개의 결측치를 발견  
        두 결측치 모두 동일 날짜에 이루어진 이전 이벤트와 시간이 유의미한 차이가 없다고 판단해 이전 이벤트의 세션값과 같게 채워넣음
    * brand
        결측치 총 6117080개  
        결측치 중 총 데이터 내에 product_id가 같은 brand명이 이미 존재하는 데이터의 경우 같은 브랜드명으로 대체
        brand명이 존재하지 않는 경우 etc로 대체
    * category_code
        brand와 같은 방식으로 category_id를 확인해 결측치 처리
        id를 통한 기존 code 데이터가 없는 경우 empyt로 처리
* session의 길이 축소
기존 session의 길이를 고유한 정보가 유지될 정도로 축소 하였음  
ex) aac8b299-4355-4c25-bbfe-c0369f19a3ce -> fe-c0369f19a3ce

* event_time 분리
날짜와 시간으로 분리  
2019-10-01 00:00:00 UTC -> 2019-10-01 // 00:00:00

* category_code 컬럼 분할
하나의 카테고리 컬럼을 3개로 분할  
3분류까지 있지만 1분류 또는 2분류까지만 있는 데이터는 해당 데이터의 마지막 분류를 3분류까지 채워넣음

* 경량화
분석과 모델링 제작에 여건을 맞추기 위해 데이터의 경량화를 진행  
user_id를 기준으로 1백만명의 user의 데이터만 추출해 진행

----

## 데이터 탐색
### userId와 session
user_id를 기준으로 session의 수를 카운트  
비정상적으로 session의 수가 많은 id를 살펴본 결과 session이 정확하게 기록되지 않아  
추가적인 session의 전처리가 필요할 것 같다

### 고객 구분
* 유저id와 product_id를 기준으로 event 파악
* event중 purchase가 있는 user를 카운트
|구분|인원 수|
|---|-----|
|구매경험 x| 884787|
|구매경험 o| 115213|
|총합|1000000|
* 구매경험 이외에도 다른 컨디션으로 고객 구분 가능  
### purchase 탐색
* 시간대에 따른 이벤트
이미지 추가
* 날짜에 따른 이벤트
이미지 추가
### 카테고리 탐색
* 카테고리별 행동의 전환율을 날짜에 따라 파악

----
