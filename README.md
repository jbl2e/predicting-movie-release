# Predicting Movie Release Duration
영화 상영일수 예측 프로젝트

극장 상영 데이터와 흥행 흐름을 기반으로 영화의 총 상영일수를 사전에 예측하는 머신러닝 모델을 구축한 프로젝트입니다.

---

## Project Overview

최근 영화 산업에서는 Hold Back (극장 개봉 이후 OTT 공개까지의 유예 기간) 단축이 중요한 이슈로 떠오르고 있습니다. 무분별한 Hold Back 단축은 극장 수익 감소와 OTT 수익 저하로 이어질 수 있으며, 이에 따라 영화의 상영 지속 가능성을 사전에 예측하는 것이 중요한 과제가 되었습니다.

본 프로젝트는 영화가 얼마나 오래 상영될 것인가를 예측함으로써 극장 상영 스케줄 최적화, OTT 전환 시점 결정, 배급 및 마케팅 전략 수립에 활용 가능한 예측 모델을 구축하는 것을 목표로 합니다.

---

## Data Collection

### Data Sources
- KOBIS (영화진흥위원회)
  - 최근 10년간 영화별 일별 데이터
  - 관객 수, 스크린 수, 상영 횟수, 좌석 점유율
  - 영화 기본 정보 (개봉일, 장르, 국적, 감독, 배우, 배급사)

- 외부 지표
  - 문화 영화 관람 활성 지수  
  (일별 및 영화별 매핑 한계로 모델에는 직접 사용하지 않음)

---

## Data Preprocessing and Feature Engineering

### Categorical Variable Processing
- 장르 및 국적
  - One-Hot Encoding
  - Target Encoding (상영일수 기여도 기반)

- 배우, 감독, 배급사
  - 카디널리티가 높아 One-Hot Encoding 적용 불가
  - 출연 영화 수, 평균 매출액 기반 Target Encoding

- Sparse feature 증가 문제 해결을 위해 Truncated SVD 적용

---

### Time-Series Based Feature Engineering
개봉 이후 흥행 흐름을 반영하기 위해 일별 관객 및 스크린 데이터를 기반으로 시계열성 피처를 설계했습니다.

- 기본 통계량 (mean, std, max)
- 극대점 위치
- 평균 변화량
- 선형 회귀 기울기

---

### Half-Life Feature
- 개봉 첫날 대비 관객 수가 절반으로 감소하는 시점
- 관객 감소 속도를 정량적으로 측정
- 반감기가 길수록 흥행 유지력이 높은 경향 확인

---

### Box Office Stability Index
흥행의 지속성과 안정성을 종합적으로 반영한 지표입니다.

구성 요소
- 유지력 (초기 대비 관객 유지 비율)
- 관객 수 규모
- 스크린 점유율의 선형 회귀 기울기

3일 및 10일 구간으로 분리하여 초기 흥행 패턴과 장기 추세를 구분했습니다.

---

## Exploratory Data Analysis

- 인기 영화일수록
  - 일별 평균 관객 수 증가
  - 스크린 수 증가
  - 변동성 증가

이를 통해 상영일수 예측에는 개봉일 자체보다 흥행 흐름과 스크린 배정 전략이 중요한 영향을 미친다는 점을 확인했습니다.

---

## Modeling

### Models Used
- Random Forest
- XGBoost
- LightGBM
- CatBoost

---

### CatBoost (Final Model)

전체 데이터 기준
- MAE: 1.412
- R²: 0.953

초기 10일 데이터만 사용
- MAE: 4.766
- R²: 0.596

초기 데이터만으로도 일정 수준의 설명력은 확보 가능하나, 전체 상영 기간 정보가 예측 성능에는 더 중요한 역할을 수행했습니다.

---

## Conclusion

본 프로젝트에서는 영화 상영 데이터와 흥행 흐름을 반영한 시계열 기반 피처와 흥행 안정성 지표를 설계하여 영화 상영일수 예측 모델을 구축했습니다.


---

## Team
기계학습 팀 프로젝트 (2025-1학기)

팀 구성 : 이재용(팀장), 이치우, 홍채민, 김영우, 김다솔, 전서연

