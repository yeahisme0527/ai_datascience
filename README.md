# 🍷 Wine Quality Regression Analysis
> AI & Data Science Course – Extended Regression Practice

## 1. Project Overview
본 프로젝트는 와인 화학적 특성과 와인 유형(type)을 활용하여  
와인의 품질 점수(quality)를 예측하는 회귀 모델을 구축하는 것을 목표로 한다.

수업 과제로 수행했던 탐색적 데이터 분석(`winequ-ana.ipynb`)에 그치지 않고,  
회귀 모델 구축 및 성능 평가까지 분석을 확장하여  
데이터 전처리부터 모델 선택, 잔차 분석까지의 전 과정을  
스스로 정리하며 연습하기 위해 추가로 진행한 개인 학습 프로젝트이다.

---

## 2. Dataset
- Wine Quality Dataset (Red & White Wine)
- Target Variable: `quality` (와인 품질 점수, 정수값)
- Feature Variables:
  - 화학적 특성 변수 (alcohol, density, residual sugar 등)
  - 와인 유형 변수 (`type`: red / white)

---

## 3. Analysis Pipeline

### (1) Data Preparation
- 레드와인 / 화이트와인 데이터 병합
- 와인 유형을 구분하기 위한 `type` 변수 생성

### (2) Train / Validation / Test Split
- Train / Validation / Test 데이터 분리
- 데이터 누수를 방지하기 위해 **분할 이후 전처리 수행**

### (3) Preprocessing (Train 기준)
- 결측치 확인 (결측치 없음)
- 이상치 처리  
  - 변수 특성과 품질과의 관계를 고려하여  
    `chlorides`, `sulphates`에만 IQR 기반 이상치 처리 적용
- 범주형 변수 인코딩  
  - `type` 변수 원-핫 인코딩 (`drop='first'`)
- 수치형 변수 스케일링  
  - StandardScaler 적용
- 다중공선성 확인  
  - 수치형 변수에 대해 VIF 계산

### (4) Validation / Test Preprocessing
- Train 데이터에서 학습된 기준(bounds, encoder, scaler)을  
  Validation / Test 데이터에 **동일하게 적용(transform)**

---

## 4. Models Compared
다음 선형 회귀 계열 모델들을 비교하였다.
- Linear Regression
- Ridge Regression
- Lasso Regression
- ElasticNet Regression

모델 평가는 Validation 데이터 기준으로  
- R²
- RMSE  
지표를 사용하여 수행하였다.

---

## 5. Final Model Selection
Validation 성능 비교 결과,  
Linear Regression과 Ridge 모델이 거의 동일한 성능을 보였다.

그러나 데이터에 다중공선성이 높은 변수(`density`, `residual sugar`)가 존재한다는 점을 고려하여,  
계수의 안정성을 확보할 수 있는 **Ridge Regression**을 최종 모델로 선택하였다.  
이는 예측 성능을 유지하면서도 다중공선성의 영향을 완화하기 위한 선택이다.

---

## 6. Residual Analysis
최종 모델에 대해 잔차 분석을 수행하였다.
- 잔차 히스토그램 및 KDE
- 예측값 vs 잔차 산점도
- Q-Q Plot

잔차 평균이 0에 매우 가까워 전반적인 과대·과소 예측 편향이 크지 않았으며,  
잔차 분포가 0을 중심으로 비교적 안정적으로 분포함을 확인했다.  
이를 통해 선형성 및 등분산성 가정이 크게 위반되지 않음을 점검했다.

---

## 7. Test Performance
Train 기준 전처리를 동일하게 적용한 Test 데이터에 대해 최종 성능을 평가하였다.

- Test R²: 약 0.26  
- Test RMSE: 약 0.75  

Validation 성능과 큰 차이가 없는 수준으로,  
모델이 과도하게 과적합되지 않고 일정 수준의 일반화 성능을 유지함을 확인했다.

---

## 8. Limitations & Future Work

### Limitations
- 타겟 변수 `quality`가 정수형 이산값이기 때문에  
  예측값-잔차 산점도에서 띠 형태가 나타나는 한계가 존재한다.
- 하이퍼파라미터(alpha, l1_ratio)를 고정값으로 비교하여  
  최적 탐색이 충분하지 않았다.
- 화학적 특성만으로 와인 품질을 완전히 설명하기에는 설명력(R²)에 한계가 있다.

### Future Work
- GridSearchCV를 활용한 하이퍼파라미터 최적화
- 이상치 처리 방식을 Train/Valid/Test에 완전히 일관되게 적용
- 비선형 관계를 고려한 모델(트리 기반 모델 등)과의 비교 실험

---

## 9. Files in This Repository
- `winequ_regression.ipynb`  
  : 회귀 모델 구축, 전처리, 모델 비교, 잔차 분석, 테스트 평가를 포함한 메인 분석 파일  
  *(EDA 과제를 확장하여 수행한 개인 학습용 분석)*
- `winequ-ana.ipynb`  
  : 수업 과제로 수행한 탐색적 데이터 분석(EDA) 결과
- `README.md`  
  : 프로젝트 개요 및 분석 흐름 정리
