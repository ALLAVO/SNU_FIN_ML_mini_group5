# 시계열 예측 및 분석 프로젝트

이 프로젝트는 M4 예측 대회에서 제공된 시계열 데이터를 활용하여, 데이터 전처리, 군집화, 모델 파라미터 튜닝 및 예측 평가를 수행하는 것을 목표로 합니다. 본 프로젝트에서는 통계 기반 모델과 머신러닝 기반 모델을 비교 분석하고, 다양한 전처리 기법(로그 변환, 차분 등)을 활용하여 예측 성능을 향상시키고자 합니다.

---

## 목차

- [프로젝트 개요](#프로젝트-개요)
- [데이터셋 소개](#데이터셋-소개)
- [프로젝트 워크플로우](#프로젝트-워크플로우)
- [사용 기술](#사용-기술)
- [결과 및 주요 발견](#결과-및-주요-발견)
- [평가 지표](#평가-지표)

---

## 프로젝트 개요

본 프로젝트는 M4 예측 대회의 실제 시계열 데이터를 기반으로, 다음의 작업을 수행합니다:

- **데이터 전처리**: 결측치 처리, 이상치 제거, 로그 변환, 차분 등 데이터 정규화 기법 적용
- **탐색적 데이터 분석(EDA)**: 전처리 전후의 통계량 비교 및 분포 시각화
- **정상성 검정**: ADF, KPSS 테스트를 통해 시계열의 정상성 평가
- **군집화 및 모델 튜닝**: DTW 기반 시계열 군집화를 통해 유사한 시계열 그룹화 및 각 그룹별 최적 SARIMA 파라미터 탐색 (그리드 서치)
- **예측 및 평가**: 다양한 예측 모델(SARIMA, Holt-Winters, Theta, TBATS, Prophet 등) 구축 및 예측 성능 평가

---

## 데이터셋 소개

프로젝트는 **M4 예측 대회** 데이터셋을 사용합니다. 이 데이터셋은 다음과 같은 특징을 가지고 있습니다:

- **시계열 데이터**: 총 100,000개의 실제 시계열 데이터 (연간, 분기별, 월간, 주간, 일간, 시계열 등 다양한 주기)
- **속성**:
    - `series_name`: 시계열 이름
    - `start_timestamp`: 시작 시점
    - `series_value`: 관측값 리스트
    - `category`: 시계열 범주
    - `Frequency`: 데이터 빈도(예: weekly, monthly 등)
    - `Horizon`: 예측 기간
    - `SP`: 계절 주기
    - `StartingDate`: 시작 날짜

자세한 정보는 아래 문헌을 참조하세요:

- Makridakis, S., Spiliotis, E., Assimakopoulos, V., *The M4 Competition: 100,000 time series and 61 forecasting methods*, International Journal of Forecasting, 2020.

---

## 프로젝트 워크플로우

1. **데이터 전처리**
    - 결측치 처리 및 이상치 제거
    - 로그 변환, 차분 등을 통한 데이터 변환

2. **탐색적 데이터 분석(EDA)**
    - 전처리 전후의 통계량(평균, 중앙값, 표준편차, 왜도 등) 비교
    - 히스토그램 및 쌍축 그래프를 활용한 시각화

3. **정상성 검정**
    - ADF(augmented Dickey-Fuller) 및 KPSS 테스트를 통해 원본 시계열과 차분된 시계열의 정상성 평가
    - 결과 예시:
        - 원본 시계열: ADF 통계량 0.4623, p-value 0.9837 (비정상)
        - 1차 차분: ADF 통계량 -3.5832, p-value 0.0061 (정상)
        - 2차 차분: ADF 통계량 -1.7058, p-value 0.4281 (비정상)

4. **ACF/PACF 분석**
    - 차분된 시계열에 대해 ACF와 PACF 플롯을 작성하여 자기상관 및 부분자기상관 구조를 분석

5. **군집화 및 모델 파라미터 튜닝**
    - DTW 기반 TimeSeriesKMeans를 통해 유사한 시계열을 군집화
    - 각 군집별로 SARIMA 모델의 최적 파라미터를 그리드 서치로 탐색

6. **예측 및 평가**
    - 각 모델의 예측 성능을 sMAPE, MASE, OWA 등으로 평가
    - 잔차 분석 및 신뢰구간 시각화를 통해 모델의 신뢰성 검증

---

## 사용 기술

- **프로그래밍 언어**: Python 3.x
- **데이터 처리**: NumPy, Pandas
- **시각화**: Matplotlib, Seaborn
- **통계 분석**: statsmodels, pmdarima, scipy
- **시계열 군집화**: tslearn
- **예측 모델**: SARIMA, Holt-Winters, Theta, TBATS, Prophet
- **환경 및 도구**: Jupyter Notebook, Git, Markdown

---

## 결과 및 주요 발견

- **데이터 변환 결과**:
    - 로그 변환 및 차분을 통해 원본 시계열의 정상성이 개선됨.
    - 1차 차분 적용 시 ADF 검정 결과 p-value가 0.0061로 낮아져 정상성을 확보.
    - 2차 차분은 일부 지표에서 개선 효과가 미미함.

- **모델 튜닝 및 군집화**:
    - DTW 기반 군집화를 통해 유사한 시계열 그룹을 도출하고, 각 그룹별 최적 SARIMA 파라미터를 탐색.
    - 그리드 서치를 통해 최적의 모델 파라미터가 도출되어 예측 정확도가 향상됨.

---

## 평가 지표

프로젝트에서는 다음과 같은 지표를 사용하여 모델을 평가합니다:
- **정상성 테스트**: ADF, KPSS 테스트
- **예측 정확도 평가**:
    - **sMAPE** (대칭 평균 절대 백분율 오차)
    - **MASE** (평균 절대 스케일 오차)
    - **OWA** (전체 가중 평균 오차)
- **모델 선택 기준**: AIC (Akaike Information Criterion)

---
