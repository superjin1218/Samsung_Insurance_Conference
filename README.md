# 🌪️ 제주도 태풍 위험도 지수 및 지수형 보험·옵션 파생상품 개발
> **AI 기반 기상 데이터 분석을 통한 태풍 피해 예측 및 날씨 파생상품(옵션) 연계 보험 모델링**

## 📝 프로젝트 개요 (Project Overview)
기후 변화로 인해 태풍의 빈도와 강도가 증가함에 따라, 기존의 전통적 손해보험과 재보험 시스템은 손해율 악화와 갱신 불확실성이라는 한계에 직면했습니다.
본 프로젝트는 **제주도**를 타겟으로 하여 다음 두 가지 솔루션을 제시합니다.

1.  **태풍 위험도 지수(Typhoon Risk Index):** 기상 데이터를 기반으로 태풍 피해 규모를 예측하는 AI 모델 개발
2.  **금융 파생상품 연계(Weather Derivatives):** 예측된 지수를 기초자산으로 하는 콜옵션 및 지수형 보험(Parametric Insurance) 설계

이를 통해 농민에게는 신속한 보상을, 보험사에게는 자본시장으로의 리스크 전가(Risk Transfer) 기회를 제공합니다.

## 👥 팀원 (Team 비룡아 안뇽)
* **김승원, 김하경, 조진우, 최인영**
* **기간:** 2025.05

---

## 💡 배경 및 필요성 (Background)
* **기후 리스크 증가:** 한반도의 기온 상승폭은 지구 평균의 3배에 달하며, 제주도는 태풍 피해의 88.5%를 차지하는 등 리스크가 집중됨.
* **전통적 보험의 한계:** 손해사정 비용 과다, 역선택 및 도덕적 해이, 거대 재해 발생 시 보험사의 지급 여력 부족 문제 발생.
* **해결책:** 객관적인 기상 데이터를 트리거(Trigger)로 사용하는 **지수형 보험**과, 리스크를 자본시장 투자자에게 분산시키는 **날씨 파생상품(CAT Bond, Option)**의 도입 필요.

---

## 🛠️ 기술적 방법론 (Methodology)

### 1. Data Analysis & AI Modeling
제주도의 과거 태풍 피해 규모를 예측하기 위해 기상청 및 공공데이터를 활용하여 모델링을 수행했습니다.

* **데이터셋 (Data Source):**
    * 제주특별자치도 자연재해발생현황 (1970~2023)
    * 주요 변수(X): 중심기압, 순간최대풍속, 일최대강수량, 누적강수량 등 12개 파생 변수
    * 타겟 변수(Y): 재산피해규모 (로그 변환 적용)
* **모델 (Model):** **Random Forest Regressor**
    * 선정 이유: 소규모 데이터에 대한 강건성, 변수 스케일 자동 처리, 비선형 관계 포착 유리.
* **성능 (Performance):**
    * $R^2$: **0.5368**
    * RMSE: 591.98
    * MAE: 354.55

### 2. Financial Engineering (Option Pricing)
개발된 '태풍 위험도 지수'를 기초자산으로 하는 콜옵션 가격을 산정했습니다.

* **가격 결정 모형:** **이항 트리 모델 (Binomial Tree Model)**
* **주요 파라미터:**
    * **행사가격 ($K$):** 보험사가 감당 가능한 Retention Level (자기자본 및 리스크 수용 한도 고려).
    * **변동성 ($\sigma$):** Random Forest 모델의 예측 오차를 기반으로 산출.
    * **리스크 중립 확률 ($p$):** 무위험 차익 거래가 없도록 설정.
    * **기대 손실액 ($Q$):** $EL = mean(max(0, Y-K))$.
* **구조:** 보험사가 $K$를 초과하는 손실에 대해 옵션 매수 포지션을 취하여 리스크 헷징.

### 3. Index-Based Insurance Design
* **구조:** 선형 지급형 파라메트릭 보험 (Linear Payout Structure).
* **트리거 설정:** 포아송 분포(Poisson Distribution) 검정을 통해 기준 지수(1.5년 주기)와 한도 지수(8년 주기) 산정.
* **가중치:** 제주 전체 피해액 중 농업 피해 비중(약 26%) 반영.

---

## 📊 분석 결과 (Results)

### 태풍 위험도 지수 시각화
> *(여기에 프로젝트 리포트의 `Actual vs Predicted` 그래프 이미지를 삽입하세요)*
> 2000년~2023년 태풍 피해액 예측 시뮬레이션 결과, 실제 피해 규모의 추세를 유의미하게 따라가는 것을 확인했습니다.

### 보험료 및 수익성 분석
시뮬레이션 결과, 제안된 보험 상품은 다음과 같은 기대 효과를 가집니다.

| 구분 | 수치 | 비고 |
| :--- | :--- | :--- |
| **손해율** | **24.20%** | 안정적인 손해율 확보 |
| **합산비율** | 34.20% | 사업비 및 운영비 포함 |
| **농가 혜택** | 신속 지급 | 손해사정 절차 생략으로 인한 빠른 복구 지원 |
| **보험사 혜택** | 리스크 헷징 | 옵션 계약을 통한 거대 재해 리스크 분산 |

---

## 💻 Tech Stack
* **Language:** Python
* **Data Processing:** Pandas, NumPy
* **Machine Learning:** Scikit-learn (Random Forest)
* **Financial Math:** Math (Binomial Tree implementation)
* **Visualization:** Matplotlib, Seaborn

---

## 📂 Repository Structure
```bash
.
├── data/                  # 기상 데이터 및 피해 현황 원본 데이터
├── notebooks/             # 전처리 및 모델링 실험 (Jupyter Notebook)
│   ├── 01_preprocessing.ipynb
│   ├── 02_random_forest_modeling.ipynb
│   └── 03_option_pricing_binomial.ipynb
├── src/                   # 소스 코드
│   ├── typhoon_index.py   # 태풍 위험도 지수 산출 모듈
│   └── insurance_design.py # 보험료 및 옵션 가격 계산 모듈
├── results/               # 모델 예측 그래프 및 성능 지표 이미지
└── README.md              # 프로젝트 소개


GitHub에 소개 페이지(README.md)를 올리는 방법은 아주 간단합니다. 아래 1. 복사할 코드를 복사해서 2. 적용하는 방법을 따라 하시면 됩니다.

1. 복사할 코드 (Raw Markdown)
아래 박스 오른쪽 상단의 'Copy' 버튼을 눌러 전체 내용을 복사하세요.

Markdown

# 🌪️ 제주도 태풍 위험도 지수 및 지수형 보험·옵션 파생상품 개발
> **AI 기반 기상 데이터 분석을 통한 태풍 피해 예측 및 날씨 파생상품(옵션) 연계 보험 모델링**

## 📝 프로젝트 개요 (Project Overview)
기후 변화로 인해 태풍의 빈도와 강도가 증가함에 따라, 기존의 전통적 손해보험과 재보험 시스템은 손해율 악화와 갱신 불확실성이라는 한계에 직면했습니다.
본 프로젝트는 **제주도**를 타겟으로 하여 다음 두 가지 솔루션을 제시합니다.

1.  **태풍 위험도 지수(Typhoon Risk Index):** 기상 데이터를 기반으로 태풍 피해 규모를 예측하는 AI 모델 개발
2.  **금융 파생상품 연계(Weather Derivatives):** 예측된 지수를 기초자산으로 하는 콜옵션 및 지수형 보험(Parametric Insurance) 설계

이를 통해 농민에게는 신속한 보상을, 보험사에게는 자본시장으로의 리스크 전가(Risk Transfer) 기회를 제공합니다.

## 👥 팀원 (Team 비룡아 안뇽)
* **조진우, 김하경, 김승원, 최인영**
* **기간:** 2025.05

---

## 💡 배경 및 필요성 (Background)
* **기후 리스크 증가:** 한반도의 기온 상승폭은 지구 평균의 3배에 달하며, 제주도는 태풍 피해의 88.5%를 차지하는 등 리스크가 집중됨.
* **전통적 보험의 한계:** 손해사정 비용 과다, 역선택 및 도덕적 해이, 거대 재해 발생 시 보험사의 지급 여력 부족 문제 발생.
* **해결책:** 객관적인 기상 데이터를 트리거(Trigger)로 사용하는 **지수형 보험**과, 리스크를 자본시장 투자자에게 분산시키는 **날씨 파생상품(CAT Bond, Option)**의 도입 필요.

---

## 🛠️ 기술적 방법론 (Methodology)

### 1. Data Analysis & AI Modeling
제주도의 과거 태풍 피해 규모를 예측하기 위해 기상청 및 공공데이터를 활용하여 모델링을 수행했습니다.

* **데이터셋 (Data Source):**
    * 제주특별자치도 자연재해발생현황 (1970~2023)
    * 주요 변수(X): 중심기압, 순간최대풍속, 일최대강수량, 누적강수량 등 12개 파생 변수
    * 타겟 변수(Y): 재산피해규모 (로그 변환 적용)
* **모델 (Model):** **Random Forest Regressor**
    * 선정 이유: 소규모 데이터에 대한 강건성, 변수 스케일 자동 처리, 비선형 관계 포착 유리.
* **성능 (Performance):**
    * $R^2$: **0.5368**
    * RMSE: 591.98
    * MAE: 354.55

### 2. Financial Engineering (Option Pricing)
개발된 '태풍 위험도 지수'를 기초자산으로 하는 콜옵션 가격을 산정했습니다.

* **가격 결정 모형:** **이항 트리 모델 (Binomial Tree Model)**
* **주요 파라미터:**
    * **행사가격 ($K$):** 보험사가 감당 가능한 Retention Level (자기자본 및 리스크 수용 한도 고려).
    * **변동성 ($\sigma$):** Random Forest 모델의 예측 오차를 기반으로 산출.
    * **리스크 중립 확률 ($p$):** 무위험 차익 거래가 없도록 설정.
    * **기대 손실액 ($Q$):** $EL = mean(max(0, Y-K))$.
* **구조:** 보험사가 $K$를 초과하는 손실에 대해 옵션 매수 포지션을 취하여 리스크 헷징.

### 3. Index-Based Insurance Design
* **구조:** 선형 지급형 파라메트릭 보험 (Linear Payout Structure).
* **트리거 설정:** 포아송 분포(Poisson Distribution) 검정을 통해 기준 지수(1.5년 주기)와 한도 지수(8년 주기) 산정.
* **가중치:** 제주 전체 피해액 중 농업 피해 비중(약 26%) 반영.

---

## 📊 분석 결과 (Results)

### 태풍 위험도 지수 시각화
> *(여기에 프로젝트 리포트의 `Actual vs Predicted` 그래프 이미지를 삽입하세요)*
> 2000년~2023년 태풍 피해액 예측 시뮬레이션 결과, 실제 피해 규모의 추세를 유의미하게 따라가는 것을 확인했습니다.

### 보험료 및 수익성 분석
시뮬레이션 결과, 제안된 보험 상품은 다음과 같은 기대 효과를 가집니다.

| 구분 | 수치 | 비고 |
| :--- | :--- | :--- |
| **손해율** | **24.20%** | 안정적인 손해율 확보 |
| **합산비율** | 34.20% | 사업비 및 운영비 포함 |
| **농가 혜택** | 신속 지급 | 손해사정 절차 생략으로 인한 빠른 복구 지원 |
| **보험사 혜택** | 리스크 헷징 | 옵션 계약을 통한 거대 재해 리스크 분산 |

---

## 💻 Tech Stack
* **Language:** Python
* **Data Processing:** Pandas, NumPy
* **Machine Learning:** Scikit-learn (Random Forest)
* **Financial Math:** Math (Binomial Tree implementation)
* **Visualization:** Matplotlib, Seaborn

---

## 📂 Repository Structure
```bash
.
├── data/                  # 기상 데이터 및 피해 현황 원본 데이터
├── notebooks/             # 전처리 및 모델링 실험 (Jupyter Notebook)
│   ├── 01_preprocessing.ipynb
│   ├── 02_random_forest_modeling.ipynb
│   └── 03_option_pricing_binomial.ipynb
├── src/                   # 소스 코드
│   ├── typhoon_index.py   # 태풍 위험도 지수 산출 모듈
│   └── insurance_design.py # 보험료 및 옵션 가격 계산 모듈
├── results/               # 모델 예측 그래프 및 성능 지표 이미지
└── README.md              # 프로젝트 소개

📚 References
본 프로젝트는 다음 문헌 및 자료를 참고하여 수행되었습니다.

제주특별자치도 자연재해발생현황 및 기상청 기상자료개방포털

금융감독원 및 보험연구원(KIRI) 지수형 보험 관련 보고서

IAIS, "Issues Paper on Index-Based Insurances"

태풍 시 보험금 청구 자료를 이용한 강풍 취약도 모델링 (한국풍공학회)
