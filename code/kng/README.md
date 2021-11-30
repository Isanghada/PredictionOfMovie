# 파일 내용

### 1. 211127 ver1
- 사용 데이터 : 영화 데이터(KOBIS 공식 통계)
  - 훈련 및 검증 데이터 : 2011년 ~ 2019년(이하 Train)
  - 타겟 데이터 : 2020년(이하 Target)
  - 사용 컬럼 : 감독, 배급사, 영화형태, 국적, 전국스크린수, 전국관객수, 장르, 등급, 영화구분
    - 라벨 : [전국관객수 / 1000] 의 값 사용
  - 총 데이터 수 : 13308개
  - `전국관객수 2000명 이상`의 데이터만 사용 : 4051개
    - 훈련 및 검증 데이터 : 3695개(Train)
    - 테스트 데이터 : 356개(Target)
- 사용 모델 : LinearRegression, RandomForestRegressor
  - LinearRegression
    - Train : 훈련_RMSE(928.1568646599026), 테스트_RMSE(885.8223485299833)
  - RandomForestRegressor
    - Train : 훈련_RMSE(773.3568140400358), 테스트_RMSE(794.3812569358838)
  - RandomForestRegressor 모델을 2020년에 적용
    - Target : 1235.698240525406
- 결과
  - Target 예측에서 RMSE가 높은 값을 기록하였다.
  - 실제값과 예측값의 그래프를 보면 Train 데이터는 예측값이 낮은 경우가 많지만, Target 데이터의 경우 대부분 예측값이 높았다.
  - 이러한 결과를 코로나19의 영향이라 판단하였다.
---
### 2. 211128 ver1.1
- 사용 데이터 : 영화 데이터(KOBIS 공식 통계)-movie, 네이버 영화 데이터(크롤링)-naver
  - 훈련 및 검증 데이터 : 2011년 ~ 2019년(이하 Train)
  - 타겟 데이터 : 2020년(이하 Target)
  - movie 주요 컬럼 : 감독, 배급사, 영화형태, 국적, 전국스크린수, 전국관객수, 장르, 등급, 영화구분
  - naver 주요 컬럼 : 평점, 평가자수, 상영시간
    - 라벨 : [전국관객수 / 1000] 의 값 사용
  - 총 데이터 수 : 13308개
  - `전국관객수 1000명`, `전국스크린수 50개`을 넘어서는 데이터만 사용 : 2393개
    - 훈련 및 검증 데이터 : 2159개(Train)
    - 테스트 데이터 : 234개(Target)
- 사용 모델 : LinearRegression, RandomForestRegressor
  - LinearRegression
    - Train : 훈련_RMSE(1041.1150283139289), 테스트_RMSE(1087.4318569187596)
  - RandomForestRegressor
    - Train : 훈련_RMSE(749.7392867099799), 테스트_RMSE(1005.376789876842)
  - RandomForestRegressor 모델을 2020년에 적용
    - Target : 904.1762262284616
- 결과
  - 전체적으로 RMSE값이 이전보다 커졌다.
    - 데이터를 사용하는 조건이 달라졌기 때문이다.
    - (211127_ver1) : 관객수 2000명
    - (211128_ver1.1) : 스크린수 50개, 관객수 1000명
  - 여전히 실제값과 예측값의 그래프를 보면 Train 데이터는 예측값이 낮은 경우가 많지만, Target 데이터의 경우 대부분 예측값이 높았다.
  - 추가적인 데이터 또는 현재 데이터의 정리가 필요할 듯하다.
---
### 3. 211129 ver1.2
- 사용 데이터 : (기존 데이터), `역대박스오피스 Top300`
  - 관객수 기준 Top300 영화 데이터
  - 컬럼 : 순번, 영화명, 감독, 국적, 전국관객수, 개봉년도, 개봉일, 배우
- 감독, 배우의 `흥행 여부`를 전체 데이터가 아닌 역대 박스 오피스 기준으로 선정
  - 역대 박스 오피스 영화 중 현재 영화의 개봉일 이전의 영화에서만 검색
  - `감독_흥행` : 역대 박스 오피스 영화 중 감독이 연출한 영화가 있는지 여부(1, 0으로 표현)
  - `주연배우_흥행` : 역대 박스 오피스 영화에 출연한 경험이 있는 배우의 수
- 사용 모델 : LinearRegression, RandomForestRegressor, `GradientBoostingRegressor(추가)`
  - LinearRegression
    - Train : 훈련_RMSE(1119.7725448653905), 테스트_RMSE(1172.584826214767)
  - RandomForestRegressor
    - Train : 훈련_RMSE(818.1246561965911), 테스트_RMSE(1144.6069312410973)
  - GradientBoostingRegressor
    - Train : 훈련_RMSE(608.4830834127497), 테스트_RMSE(1177.907388733862)
  - RandomForestRegressor 모델을 2020년에 적용
    - Target : 1253.4820852032774
- 결과 및 진행할 개선사항
  - 470만명이 최대였던 2020년 데이터의 예측치가 700만명, 800만명이 보인 것을 보아 코로나의 영향이 컸음을 보일 수 있다.
  - 2011년~2019년 데이터의 경우 여전히 예측률이 좋지않아 개선이 필요하다.
  - 범주형 데이터에 대한 처리, 추가할 수 있는 데이터 찾기
---
### 3. 211129 ver1.3
- 사용 데이터 : (기존 데이터)
- 배급사 흥행 실적 추가
  - **역대 박스 오피스 영화 중 현재 영화의 개봉일 이전의 영화 중 해당 배급사의 수**
- **관객수 1100만명 이상은 이상치로 판단하여 **
- 사용 모델 : LinearRegression, RandomForestRegressor, GradientBoostingRegressor
- Scaler : `StandardScaler`, `Log Scaler`
  |Scaler|미적용|StandardScaler|LogScaler|
  |------|------|--------------|---------|
  |LinearRegression|Train : 863.67</br>Test : 949.92|Train : 863.67</br>Test : 948.28|Train : 1034.01</br>Test : 1130.87|
  |RandomForest|Train : 652.94</br>Test : 818.63|Train : 656.09</br>Test : 813.55|Train : 653.06</br>Test : 819.30|
  |GradientBoosting|Train : 647.12</br>Test : 869.06|Train : 647.12</br>Test : 853.03|Train : 647.12</br>Test : 869.08|
  |2020년 데이터|LR : 1062.91</br>RF : 896.91</br>GB : 994.62|LR : 830.27</br>RF : 693.80</br>GB : 768.70|LR : 791.24</br>RF : 898.18</br>GB : 996.28|
- 결과 및 진행할 개선사항
  - 이상치 제거로 2011년 ~ 2019년 데이터의 정확성이 눈에 보이게 늘어났다.
  - Scaler의 효과는 크게 보이지 않았으며 RandomForest, GradientBoosting이 좋은 성능을 보였다.
  - 2020년 예측치에서도 대부분 실제 관객수보다 예측치가 높았다.
---
