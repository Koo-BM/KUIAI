<h2 align="center">:checkered_flag:제2회 KUIAI-해커톤<br><br>🌿갯끈풀 발생 영향 변수 도출 및 갯벌별 위험등급 산출</h2>

<p align = "center"><img src = "README Images/상장.png" width = "400" height = "500"></p>

<h2>🌿 1. 개요</h2>

- 공모전: 2021년 해양수산부에서 주최한 [2021 JOISS 해양과학 빅데이터 활용 경진대회](https://dacon.io/competitions/official/235793/overview/description)

- 주제: 갯끈풀 발생 영향 변수 도출 및 갯벌별 위험등급 산출

- 수행기간: 2021. 09. 01 ~ 2021.10. 24

- 수행인원: 구희연, 양재영, 구병모

- 결과 및 성과: `한국해양학회장상(2등)`

- 담당역할: IDW 보간기법을 사용하여 데이터 생성, 갯끈풀 발생 영향 변수 EDA, PCA, SMOTE, Logistic 기법을 통한 가중치 설정

<h2>🌿 2. 분석 내용</h2>

- 해양특성평가격자(약 5km)를 세분한 후, 갯벌 공간정보와 JOIN하여 1,399개의 격자 생성

- 해양데이터 특성상 모든 격자에 변수 값이 존재하지 않기 때문에 IDW 보간기법 사용

- EDA, PCA, Random Forest 모델을 통한 갯끈풀 영향 변수 선택

- Logistic Regression을 통한 각 변수의 가중치 설정

- 군집분석(K-Means) 수행 후, 각 군집 특성을 파악하여 위험등급 설정
