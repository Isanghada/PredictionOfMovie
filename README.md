# 🎬머신러닝 프로젝트 : 영화 관람객 수 예측
## Introduction
- 영화의 흥행에 영향을 주는 변수 데이터를 수집하여 머신러닝 학습을 통해 관람객 수 예측 모델을 구현하였습니다.(2011년 ~ 2019년 데이터 활용)
- 추가로 2020년의 영화들이 코로나19의 영향이 없었을 때 관객의 수 예측치를 확인하여 코로나19의 영향을 확인하고자 하였습니다.
  
## Contents
- [1. 프로젝트 소개](#1-프로젝트-소개) 
- [2. 데이터 수집 및 전처리](#2-데이터-수집-및-전처리)
  - [데이터 수집](#데이터-수집)
  - [데이터 전처리](#데이터-전처리)
- [3. 모델링](#3-모델링)
- [4. 최종 결과](#4-최종-결과)
- [5. 한계 및 보완점](#5-한계-및-보완점)
- [6. 참고 자료](#6-참고-자료)
- [7. 구성 인원](#7-구성-인원)

## 1. 프로젝트 소개
### 배경
- 영화 산업은 지속적인 성장을 거듭하고 있었습니다.
- 2019년 말 코로나19가 유행하며 2020년 관객수는 70% 가량 감소하였습니다.
- 코로나19 이전의 데이터로 관객수에 영향을 미치는 요소를 분석하여 2020년 영화 관객수 예측으로 그 영향을 파악해보자 하였습니다.

<p align="center"><img src="https://lh3.googleusercontent.com/0O2qnkN7_RpY9Hi2b5xrYtJX0WXMHKOPZGR4fQBTf_MewXwYLrAAqEr8816mNl_SO0k7C8WPK2hD08S7G36nWEmOEhanIaXgscJxpks4oIvzGThMRsEdPOIYIvij1vB4FPi8jTHB" width="40%" height="40%"></p>  
<p align="center">[영화진흥위원회]</p>

### 프로젝트 개요
- 구성인원 : 김남규, 노연우, 이은지, 이태기
- 수행기간 : 2021년 11월 25일 ~ 2021년 12월 03일
- 목표 : 영화 관람객 수 예측
- 데이터 : KOBIS 공식 통계(2011년 ~ 2020년), 네이버 영화 데이터
  - 학습(2011년 ~ 2019년) : 2,145개
    - 훈련용 : 1,716개
    - 테스트 : 429개
  - 테스트(2020년) : 234개 
  - 평가지표 : RMSE(Root Mean Square Error)

### 개발 환경
- 시스템 : Colab
- 언어 : Python
- 라이브러리 : **Python**, **BeautifulSoup4**, **Pandas**, **Numpy**, **Matplotlib**, **Seaborn**, **Sklearn(Scikit-Learn)**
- 알고리즘 : Linear Regression, RandomForest, GradientBoosting

## 2. 데이터 수집 및 전처리
### 데이터 수집
- KOBIS 공식 통계
  - 주요 컬럼 : [영화명, 감독, 제작사, 수입사, 배급사, 개봉일, 영화유형, 영화형태, 국적, 전국스크린수, 장르, 등급, 영화구분 등]
  - 링크 : https://www.kobis.or.kr/kobis/business/stat/offc/searchOfficHitTotList.do?searchMode=year  
  <img src ='https://postfiles.pstatic.net/MjAyMjAzMTdfMjE1/MDAxNjQ3NDk2Njc3Njkz.CfvXnZ96nsSifMKIzmjX77IADUc3705q-hPMG4dgJ0kg.PU1PNMIkmb7ZztO5pFMmN0vRqj-2hR12OrXDtdoaQlwg.PNG.isanghada03/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-03-17_145719.png?type=w966' width = '70%' height='70%'>

- 네이버 영화 데이터
  - `beautifulsoup4` 라이브러리를 이용하여 KOBIS 자료에 없는 [주연배우, 평점, 평가자수, 상영시간] 추가 획득   
    - KOBIS 통계를 전처리하고 남은 데이터를 기준으로 실행  

    <img src = 'https://postfiles.pstatic.net/MjAyMjAzMTdfMjE0/MDAxNjQ3NDk3NDgzNzY1.uPdblQg79v3KCmYOUrPRXxaNQ1ZgIL5Pl0ZVId_EPRUg.BeifQvkjYWCD2d_dXSSMnErAQLtoUn19Z4c94sxD_7Ig.PNG.isanghada03/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-03-17_151111.png?type=w966' width = '70%' height='70%'>

- 역대 박스 오피스 Top 300
  - KOBIS 공식 통계와 같은 자료를 사용하여 관객수 기준 역대 Top300의 영화 데이터 정리
  - 감독, 배급사, 배우의 흥행 실적을 수치화하기 위해 사용

### 데이터 전처리
- **데이터 정리**
  - 결측치가 많은 제작사, 수입사 등은 `삭제`
  - 다른 결측치는 `기타`로 통일  
    <img src = 'https://user-images.githubusercontent.com/90487843/159144350-92195402-ffb1-4846-bf2c-43fd7a2406c4.png' width='30%' height='30%'>

  - 값이 1개만 있던 `개봉영화` 컬럼 삭제
  - 발권 정보를 기준으로 수집된 데이터 이므로 `전국 매출액`과 `전국 관객수`는 거의 동일한 컬럼이므로 삭제  
    <img src = 'https://user-images.githubusercontent.com/90487843/159144642-f6805ddb-c2a2-4634-9061-bce65dc3c43d.png' width = '30%' height='30%'>

  - 총 68개의 국적을 [Top5와 기타]로 대체  
    <img src='https://user-images.githubusercontent.com/90487843/159144683-d78115cf-3bb6-4e63-8719-f25e9d1356c3.png' width='30%' height='30%'>

- **복수 값 처리**
  - 감독, 배급사, 등급과 같이 값이 복수로 주어진 경우 대표값으로 조정
    - 감독, 배급사 : 가장 앞에 있는 값으로 설정
    - 등급 : 연령대가 가장 작은 값으로 설정  

    <img src = 'https://user-images.githubusercontent.com/90487843/159144598-b585f086-e552-437b-b77b-c235ba0a64cf.png' width = '30%' height='30%'>

- **이상치 처리**
  - `스크린수`가 너무 적거나, `관객수`가 너무 적거나 많은 경우는 일반적인 경우가 아닌 이상치로 판단하여 제외
    - 1,000 < `전국관객수` < 11,000,000
    - 50 < `전국스크린수`
- [감독, 배급사, 배우]는 해당 영화 이전에 개봉한 영화의 성적을 기준으로 수치화 진행
- [영화 형태, 국적, 장르, 등급, 영화 구분] 컬럼은 `One-Hot Encoding`을 통해 변환

## 3. 모델링
#### - 모델 구현
- 학습 데이터 : 코로나19 이전인 2011년 ~ 2019년 영화 데이터(2,145개)
  - 훈련용 : 1,716개
  - 테스트 : 429개
- 스케일러 : StandardScale
- 회귀 모델 : 가장 지표가 좋은 모델을 최종 모델로 선택하여 2020년 관객수 예측 진행  
  - LinearRegression  
  - RandomForestRegressor  
  - GradientBoostingRegressor  
- 평가기준 : RMSE(Root Mean Square Error)
- 예측 결과 : 예상 영화 관객수(단위 : 천 명)
#### - 모델 구현 결과
<img src='https://user-images.githubusercontent.com/90487843/159145179-4357a6e0-4b2c-40fe-b207-b9e325d00ee6.png' width='50%' height='50%'>

|Model|Train RMSE|Test RMSE|
|-|-|-|
|Linear Regression|854.4|946.8|
|Random Forest|651.3|812.6|
|Gradient Boosting|643.4|842.8|

## 4. 최종 결과
- 가장 지표가 좋은 `Random Forest` 모델을 최종 모델로 선택
- 2020년 데이터 예측 결과(RMSE) : **716.4** 
  - 2020년 이전 데이터의 결과와 달리 실제값보다 높게 예측하는 경우가 많음
  - 정확하다고 볼 수는 없지만 경향은 확인은 가능한 것으로 보임

  <img src = 'https://user-images.githubusercontent.com/90487843/159146817-51679b97-45b0-4d12-99d0-eefd1a7d147d.png' width='30%' height='30%'>

- 2020년의 실제 관객수 보다 약 3배 많을 것으로 예측 
  - 코로나19의 영향으로 확실히 많은 관객이 줄었음을 확인할 수 있었음  

  <img src = 'https://user-images.githubusercontent.com/90487843/159146841-0af00784-9e75-4fd0-b67b-8fef4546485e.png' width='30%' height='30%'>

## 5. 한계 및 보완점
#### 🛠데이터 부실
- 짧은 기간 동안 진행한 프로젝트이다 보니 다양한 데이터를 살펴볼 시간이 부족했다.
  - 쉽게 얻을 수 있는 데이터(KOBIS 통계, 네이버 영화 등)를 중점으로 큰 변경없이 진행하였다.
- 현재의 데이터를 이용해 새로운 컬럼을 생성하거나 리뷰를 이용한 감성 분석 등 다양한 방식을 사용하면 조금 더 보완할 수 있을 것이라 생각한다.

#### 🛠간단한 수치화 구조
- [감독, 배급사, 배우]의 경우 이전 개봉 영화의 성적을 이용해 수치화를 진행하였다.
  - 추가적인 처리없이 단순하게 계산을 한 것이기 때문에 정확하게 영향력을 수치화했다고 보기 힘들다.
- 수치화 공식을 다양하게 구성하여 예측을 진행해보며 최적값을 찾아보는 과정이 필요했다고 생각한다.

## 6. 참고 자료
  - https://www.koreascience.or.kr/article/CFKO202015463051319.pdf
  - http://www.bigdata-map.kr/datastory/new/story_24
  - http://koreascience.or.kr/article/JAKO201834663385693.pdf
  - https://rpubs.com/leesohee/689993
  - https://smecsm.tistory.com/69

## 7. 구성 인원
- 구글 드라이브 : https://drive.google.com/drive/folders/1KfbVX6P0QhPFNE1A--cmuceCTLGKelgU?usp=sharing
- 김남규 (<a href = 'https://github.com/Isanghada' href='_blank'>github</a>)
- 노연우
- 이은지
- 이태기
