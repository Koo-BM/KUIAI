<h2 align="center">:checkered_flag:제2회 KUIAI-해커톤<br><br>:convenience_store:도로명주소별 입지 점수 예측 모델 개발 및 입지 점수 등급 시각화</h2>

<p align = "center"></p>

<h2>:convenience_store: 1. 개요</h2>

- 공모전: [제2회 KUIAI-해커톤](https://ie.korea.ac.kr/ie/news/notice.do?mode=view&articleNo=284519)

- 주제: 도로명주소별 입지 점수 예측 모델 개발 및 입지 점수 등급 시각화

- 수행기간: `2022. 01. 10 ~ 2022.01. 14`

- 수행인원: 구병모, 김가영, 안정수

- 결과 및 성과: `2등상`

- 담당역할: 좌표 추출, 거리 계산 컬럼 도출, 모델링 및 함수 구현, 보고서 제작 (구병모)

- 개발일정

<p align = "center"><img src = "photo/1. 개발 일정.JPG" width = "500" height = "400"></p>

<h2>:convenience_store: 2. 분석 배경</h2>

- 자영업자의 소업률은 13.3%, 창업 후 5년 생존율이 26.9%로 신생업체의 3/4 가량이 5년 내에 폐업하는 현실

- 창업의 낮은 생존율 문제는 경제적 악순환을 야기함

- 창업의 중요 고려 요소인 점포 입지는 한번 정해지고 나면 쉽게 바꿀 수 없는 장기적인 성격을 가지는 고정 투자이므로 신중한 결정 필요
 
<h2>:convenience_store: 3. 분석 목표</h2>

#### 건축물 입지 점수 예측 모델 구축
- 서울시 건축물 생애 이력 데이터의 도로명 주소로 입지 점수를 예측

#### HEATMAP 시각화
- 도로명주소 검색 시 해당 건물의 반경 500m를 heatmap으로 표현

#### 건축물 등급화
- 예측된 입지 점수를 기반으로 4개의 등급(최상, 상, 하, 최하)으로 나눠 영역 표현

<h2>:convenience_store: 4. 분석 프로세스</h2>

<p align = "center"><img src = "photo/2. 분석 프로세스.JPG" width = "600" height = "500">

<h2>:convenience_store: 5. 분석 내용</h2>

#### 1.1 생애이력 데이터 결합
<img src = "photo/3. 생애이력 데이터 결합.JPG" width = "500" height = "500"></p>

- 용도별, 기역구별 엑셀 데이터를 결합하여 120009건의 데이터셋 구축

#### 1.2 좌표 정보 추출

- 건물 생애이력 도로명주소 좌표 <---- 구글 api 활용

- 지하철역, 학교, 대규모 점포, 상권 좌표 <---- 카카오 api 활용

#### 1.3 거리 계산, 평균 생활인구, 평균 매출액(y) 추출

<img src = "photo/4. 거리 계산.JPG" width = "500" height = "500">

- 기존 아이디어: 건물별로 모든 지하철역, 학교, 대규모 점포들과의 거리를 비교한 결과 소요 시간이 컸음

- New 아이디어: 각각의 건물별로 일정 크기의 버퍼(ex. 5km, 10km)를 그려서 해당 버퍼에 속한 지하철역, 학교, 대규모 점포들과 건물 간의 거리 중 최소값을 선택

- 도로명주소별 500m 반경 내에 있는 상권코드를 구하여 해당 상권들의 총 매출액과 총 생활인구의 평균을 내서 평균 생활인구, 평균 매출액(y)으로 사용

- 도로명 주소별 가장 가까운 지하철역, 학교, 대규모 점포와의 거리 & 평균 매출액, 평균 생활인구 

#### 1.4 건물별 생애 이력 데이터 결합

<img src = "photo/5. 건물별 생애 이력.JPG" width = "500" height = "500">

- 건물별 생애 이력 데이터 중 건물 용도, 연면적, 층수, 주차장 개수, 승강기 개수만을 추출

- 앞서 만든 데이터셋과 건물별 생애 이력 데이터를 결합하여 분석에 활용할 최종 데이터셋을 구축

#### 2.1 데이터 결측치 처리

- 도로명주소의 500m 반경에 상권이 하나도 없을 경우 평균매출액(y)과 평균 생활인구 값은 NaN

- 해당 경우 상권이 하나도 없어서 입지 점수를 산출할 수 없기에 빼는 것이 적합하지만 input으로 해당 도로명주소가 들어오면 오류가 날 수 있기에 0으로 처리

#### 2.2 중복 도로명 주소

- 하나의 도로명 주소에 여러 개의 건물이 할당될 수 있음

- 도로명 주소를 그룹으로 하여 중복된 도로명주소의 건물들의 컬럼 정보를 평균

<img src = "photo/6. 중복 도로명.JPG" width = "500" height = "500">

- 최종 78,835개의 도로명 주소를 모델링에 활용

#### 2.3 입지점수 예측 모델 개발

- 단순선형회귀, 라쏘회귀, 릿지회귀, XGBoost, LightGBM, K-nearest Neighbors 회귀모델을 고려하여 비교

<img src = "photo/7. 모델 비교.JPG" width = "500" height = "500">

- 가장 높은 성능을 보인 K-nearest Neighbors 회귀모델을 선택해서 학습을 진행하고 예측값을 산출

<img src = "photo/8. KNN.JPG" width = "500" height = "500">

<h2>:convenience_store: 6. 분석 결과</h2>

#### Input: 도로명 주소

#### Output: 입지점수, 점수별 히트맵, 건물별 등급 지도

<img src = "photo/9. 함수.JPG" width = "500" height = "500">

<img src = "photo/10.1. 결과.JPG" width = "500" height = "500">

<img src = "photo/10.2. 결과.JPG" width = "500" height = "500">

- 상권평가지수(상권 내 음식, 서비스 소매 등 전반적인 업종 등을 기타 설비 서비스를 종합하여 산출한 등급)의 보조적 장치로 활용 가능

<h2>:convenience_store: 7. 참고 문헌</h2>

- E Maria, E Budiman, Haviluddin, M Taruk. Measure distance locating nearest public facilities using Haversine and Euclidean Methods. Journal of Physics: Conference Series. 2020;1450(1):1. Accessed January 13, 2022.

- Huff, D. L.(1963). "A probability analysis of shopping centre trade areas," Land
Economics, Feb : 81-90.

- 김황배, & 김시곤. (2006). 접근성이론과 GIS 공간분석기법을 활용한 행정기관의 입지선정. 대한토목학회논문집 D, 26(3D), 385-391.

- 심재헌, & 이성호. (2008). 대형할인점의 입지선정을 위한 의사결정에 관한 연구. 대한토목학회논문집 D, 28(5D), 705-712.

- 홍준혁.(2019). 빅 데이터를 활용한 서울시 요식업 분석 : 데이터 마이닝을 활용한 서울시 내 요식업 창업 전략수립을 위한 연구.

- 김일광.(2018). 우리나라 자영업 업체 현황과 재무특성에 관한 연구. 지역산업연구 제 41권 제3호, 343-364.

- 김진철 & 양현철. 빅데이터를 활용한 소상공인 영업지원 점포 분석 사례 연구, 한국정보처리학회 2015년도 추계학술발표대회 2015 Oct. 28, 2015년 pp.1244 - 1247

- 유지은, & 양혜원. (2011). 패스트푸드점 입지요인이 방문의도에 미치는 영향. 한국외식산업학회지, 7(1), 23-41.

- 태경섭, & 임병준. (2010). 상권경쟁을 고려한 신규점포의 입지선정에 관한 연구: 서울시 대형마트를 대상으로. 대한지리학회지, 45(5), 609-627.
