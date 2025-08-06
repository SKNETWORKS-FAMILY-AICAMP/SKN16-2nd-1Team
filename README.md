# 🎯 SKN16-2nd-1Team: 2023년 흡연율 예측 프로젝트



---

## 📌 1. 프로젝트 개요

### 1.1 프로젝트 소개  
> 2020–2022년 지방자치단체별 인구·보건·경제 지표를 활용해 2023년 흡연율을 머신러닝/딥러닝으로 예측

### 1.2 주제선정 배경  
  - 최근 지방자치단체들은 금연 정책을 보다 효과적으로 추진하기 위해 지역 단위 맞춤형 프로그램과 첨단 기술을 적극 도입함
  - 
    2025년 7월, 서울 서초구는 전국 최초로 배터리 내장형 인공지능(AI) 기반 흡연 감지장치 “서초 AI 흡연 제로”를 본격 운영하겠다고 밝혔으며, 간접흡연으로 인한 주민 피해를 획기적으로 저감하고 금연 환경 조성에 나섬
    
    이처럼 시군구 단위에서의 혁신적 금연 지원서비스가 확대됨에 따라, **시군구별 흡연율을 정확히 예측하고 모니터링하는 데이터 기반 접근**의 필요성이 대두됨
- **공중보건 강화**  
  자치단체별 흡연율 변화를 사전에 예측하여 금연 정책의 효과를 극대화  
- **데이터 활용**  
  시·군·구 단위의 상세 데이터를 모델에 적용해 많은 정보 제공

### 1.3 프로젝트 목표  
1. 과거(2020–2022년) 데이터로 2023년 흡연율 예측  
2. 10종 회귀 모델(RF, XGBoost, LGBM, Stacking 등) 성능 비교  
3. Streamlit 웹 앱으로 대시보드 배포  

---
## 🛠️ 기술 스택

| 구분                   | 도구 및 라이브러리                                                                                                                      |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| **언어**                | [![Python](https://img.shields.io/badge/Python-3.10-blue)](https://www.python.org/)                                                     |
| **데이터 처리**           | [![pandas](https://img.shields.io/badge/pandas-v1.5-blue)](https://pandas.pydata.org/)  [![NumPy](https://img.shields.io/badge/NumPy-v1.24-blue)](https://numpy.org/)                                   |
| **머신러닝**             | [![scikit-learn](https://img.shields.io/badge/scikit--learn-v1.2-orange)](https://scikit-learn.org/)  [![XGBoost](https://img.shields.io/badge/XGBoost-v1.7-orange)](https://xgboost.readthedocs.io/)  [![LightGBM](https://img.shields.io/badge/LightGBM-v3.3-green)](https://lightgbm.readthedocs.io/)  [![CatBoost](https://img.shields.io/badge/CatBoost-v1.1-purple)](https://catboost.ai/)                     |
| **딥러닝**              | [![PyTorch](https://img.shields.io/badge/PyTorch-v1.13-red)](https://pytorch.org/)                                                     |
| **하이퍼파라미터 튜닝**      | [![Optuna](https://img.shields.io/badge/Optuna-v3.1-yellow)](https://optuna.org/)                                                     |
| **웹 프레임워크**          | [![Streamlit](https://img.shields.io/badge/Streamlit-v1.0-orange)](https://streamlit.io/)                                              |

---

## 🔍 2. 데이터분석 및 전처리

### 2.1 데이터셋    
- **기간**: 2020–2023년, 전국 207개 시·군·구  
- **주요 컬럼**:

| 컬럼명             | 설명                          |
| ------------------ | ----------------------------- |
| `연도`             | 조사 연도 (2020–2023)         |
| `시도`, `시군구`   | 행정구역 식별자               |
| `총 인구수`, `성인인구`, `흡연인구` | 인구 통계               |
| `담배소매지점개수`, `소비자물가지수` | 경제 지표               |
| `평균 기대수명`, `평균소득월액`, `진학률`, `진료비(천원)` | 보건·복지 지표 |
| `흡연율`           | (흡연인구 / 성인인구) × 100(%)  |

<details>
<summary>🗂️ 전체 컬럼 보기</summary>

['연도','시도','시군구','총 인구수','성인인구','흡연인구',
'담배소매지점개수','소비자물가지수','평균 기대수명',
'평균소득월액','진학률','진료비(천원)','흡연율']


</details>

---

### 2.2 연도별 지표 변화  
2020–2023년 **시도별** 주요 지표 추이  
<div align="center">
   <img width="1790" height="1625" alt="Image" src="https://github.com/user-attachments/assets/606cec0a-218d-4d09-bad1-d25fb83af914" />
  <p><em>그림 1. 시도별 연도 추이 종합</em></p>
</div>

---

### 2.3 변수 간 상관관계

#### 2.3.1 Pearson 상관관계 히트맵  
<div align="center">
  <img width="859" height="754" alt="Image" src="https://github.com/user-attachments/assets/bbdbd46a-98aa-4d54-8da6-469976601b17" />
  <p><em>그림 2. 변수 상관관계 히트맵</em></p>
</div>

#### 2.3.2 ‘흡연율’과 상관계수 Top 10  
<div align="center">
  <img width="789" height="590" alt="Image" src="https://github.com/user-attachments/assets/495ac861-926f-41c0-ad1b-cfd75e4fa697" />
  <p><em>그림 3. 흡연율 상관계수 Top 10 (절댓값)</em></p>
</div>

---

### 2.5 모델 성능 비교 및 해석

#### 2.5.1 모델별 R² 비교  
<div align="center">
  <img width="482" height="431" alt="4__r2" src="https://github.com/user-attachments/assets/7801140e-347b-4cc1-b586-c1f10b6e8987" />
  <img width="987" height="590" alt="deeplearning" src="https://github.com/user-attachments/assets/a2b1a5fa-f19f-4bab-b731-8fb1d3180d43" />
  <p><em>그림 4. 모델별 R²</em></p>
</div>

### 2.5.2 최상위 모델: DeepLearning (딥러닝)

가장 우수한 성능을 보인 **DeepLearning** 모델을 최상위 모델로 선정 (Optuna로 최적의 하이퍼파라미터 적용)  
<p> epoch 500 </p>

| 모델명    | 구조                                    | 활성화 | 드롭아웃 | R² (테스트) |
|----------|-----------------------------------------|--------|---------|-------------|
| DeepLearning | Input → 99 → 80 → Output               | ReLU   | 0.3703     | 0.9207       |

<div align="center">
  <img width="590" height="590" alt="lin" src="https://github.com/user-attachments/assets/cc805eff-6e0b-4af0-94aa-4a81032bedb4" />
  <p><em>그림 5. DeepLearning 실제 vs 예측 산점도</em></p>
</div>

<div align="center">
 <img width="689" height="467" alt="loss" src="https://github.com/user-attachments/assets/6ff45f5b-58f8-48a3-863a-b773db8c1e84" />
  <p><em>그림 6. 학습 손실 곡선</em></p>
</div>

| 모델명    | 구조                                    | 활성화 | 드롭아웃 | R² (테스트) |
|----------|-----------------------------------------|--------|---------|-------------|
| DeepLearning | Input → 99 → 80 → Output               | ReLU   | 0.3703     | 0.8813       |
<p> epoch 100 </p>
<div align="center">
<img width="459" height="375" alt="100_MSE" src="https://github.com/user-attachments/assets/7b94577c-8587-46f9-a21d-035d8a9c1c4b" />
<img width="987" height="590" alt="100_CORR" src="https://github.com/user-attachments/assets/69d7aa5a-0e0c-4c96-9516-e469dc447c6b" />
<img width="590" height="590" alt="100_SCATTER" src="https://github.com/user-attachments/assets/cdb91c17-4f11-4102-88a2-be318f39200d" />
<img width="689" height="467" alt="100_CURVE" src="https://github.com/user-attachments/assets/4aba6326-8284-4e4f-97a7-e570f352f316" />
</div>

| 모델명    | 구조                                    | 활성화 | 드롭아웃 | R² (테스트) |
|----------|-----------------------------------------|--------|---------|-------------|
| DeepLearning | Input → 99 → 80 → Output               | ReLU   | 0.3703     | 0.9042       |
<p> epoch 200 </p>
<div align="center">
<img width="458" height="369" alt="MSE_250" src="https://github.com/user-attachments/assets/cf1185de-0256-4acc-b643-573598084126" />
<img width="987" height="590" alt="250_CORR" src="https://github.com/user-attachments/assets/d2659703-1aeb-45fb-887e-bb479bbba50f" />
<img width="590" height="590" alt="250_SCATTER" src="https://github.com/user-attachments/assets/65c926a7-09b5-47c4-b167-b858017562eb" />
<img width="689" height="467" alt="250_CURVE" src="https://github.com/user-attachments/assets/e7fa5a01-8bf1-43ff-adad-5ebad7616027" />

</div>

| 모델명    | 구조                                    | 활성화 | 드롭아웃 | R² (테스트) |
|----------|-----------------------------------------|--------|---------|-------------|
| DeepLearning | Input → 99 → 80 → Output               | ReLU   | 0.3703     | 0.8853       |
<p> epoch 600 </p>
<div align="center">
<img width="464" height="369" alt="600_MSE" src="https://github.com/user-attachments/assets/a18b40f9-cbef-4ff0-af9d-8c226d3a6db4" />
<img width="987" height="590" alt="600_CORR" src="https://github.com/user-attachments/assets/c1bce91a-00a7-4ed7-b8e7-a4c2d5f99aa2" />
<img width="590" height="590" alt="600_SCATTER" src="https://github.com/user-attachments/assets/067e489c-40a5-4be3-8bfa-c393342629ef" />
<img width="689" height="467" alt="600_CURVE" src="https://github.com/user-attachments/assets/8be3011a-0dae-48db-9c60-f6f3b2248c13" />



</div>

| 모델명    | 구조                                    | 활성화 | 드롭아웃 | R² (테스트) |
|----------|-----------------------------------------|--------|---------|-------------|
| DeepLearning | Input → 99 → 80 → Output               | ReLU   | 0.3703     | 0.9517       |
<p> epoch 1000 </p>
<div align="center">
<img width="461" height="367" alt="1000_MSE" src="https://github.com/user-attachments/assets/c4802869-dca1-4392-9a68-8b000cf2f666" />
<img width="987" height="590" alt="1000_CORR" src="https://github.com/user-attachments/assets/72cd0a76-d9d5-42a3-af55-be729c0a96bc" />
<img width="590" height="590" alt="1000_SCATTER" src="https://github.com/user-attachments/assets/e3c93973-bbf4-4fde-941a-6de5975dc578" />
<img width="689" height="467" alt="1000_CURVE" src="https://github.com/user-attachments/assets/a2ccef3c-ce20-4aac-8356-b4bc5fc33b88" />
</div>

| 모델명    | 구조                                    | 활성화 | 드롭아웃 | R² (테스트) |
|----------|-----------------------------------------|--------|---------|-------------|
| DeepLearning | Input → 99 → 80 → Output               | ReLU   | 0.3703     | 0.9172       |
<p> epoch 2000 </p>
<div align="center">
<img width="461" height="365" alt="MSE_2000" src="https://github.com/user-attachments/assets/a07f2286-2556-4c1e-8339-7c8ac038c09a" />
<img width="987" height="590" alt="2000_CORR" src="https://github.com/user-attachments/assets/9e6329c0-21bd-4d3d-9627-b6cc302a9b5b" />
<img width="590" height="590" alt="2000_SCATTER" src="https://github.com/user-attachments/assets/717c3317-e25a-4ac6-acc3-527949f7c22f" />
<img width="689" height="467" alt="2000_CURVE" src="https://github.com/user-attachments/assets/09f75a83-5f9c-48ee-b9e5-c285828c346c" />
</div>


---

## 🚀 시연 페이지 (Demo)

아래는 Streamlit 웹 앱의 주요 화면입니다.  
이미지를 클릭하면 원본 크기로 보실 수 있습니다.

---

### 그림 1. 연도별 주요 지표 추이

<a href="https://github.com/user-attachments/assets/3cc2267b-ccbe-482d-b6e7-bdbdbd02db2c" target="_blank">
  <img width="1473" height="670" alt="스크린샷 2025-08-06 오후 12 30 07" src="https://github.com/user-attachments/assets/5bb2f6df-3949-425c-86ea-0a1baffc4a96" />
  <img width="1490" height="600" alt="스크린샷 2025-08-06 오후 12 31 05" src="https://github.com/user-attachments/assets/381c56a5-aa5f-4087-b2f2-8ee0d9734f96" />
</a>


<p align="center"><em>그림 1. 2020–2023년 시도별 주요 지표 연도별 추이</em></p>

---

### 그림 2. 변수 간 상관관계 히트맵

<a href="https://github.com/user-attachments/assets/2844e05c-570d-4c91-923d-ec947088d7aa" target="_blank">
  <img width="1127" height="683" alt="스크린샷 2025-08-06 오후 12 22 31" src="https://github.com/user-attachments/assets/c730011d-0b65-45b5-a898-553f9c5d1007" />
</a>
<p align="center"><em>그림 2. 데이터 전체 변수 간 Pearson 상관관계</em></p>

---

### 그림 3. 전체 예측 성능

<a href="https://github.com/user-attachments/assets/ace84df2-f33a-4510-b262-775b0344456b" target="_blank">
  <img width="1501" height="699" alt="스크린샷 2025-08-06 오후 12 23 51" src="https://github.com/user-attachments/assets/d8c5bcb4-42b8-49ea-a53d-964f0dec21c5" />
  <img width="1143" height="556" alt="스크린샷 2025-08-06 오후 12 27 05" src="https://github.com/user-attachments/assets/8721a400-f199-4f69-9f7f-8f5408479d5f" />
  <img width="1229" height="554" alt="스크린샷 2025-08-06 오후 12 27 58" src="https://github.com/user-attachments/assets/92c599a2-7822-4378-a85a-fa43568ad3e7" />

</a>


<p align="center"><em>그림 3. 실제 vs 예측</em></p>

---

### 그림 4. 예측 오차 분포 히스토그램

<a href="https://github.com/user-attachments/assets/81304333-0720-4ed4-921f-6ef139852990" target="_blank">
  <img width="1176" height="418" alt="스크린샷 2025-08-06 오후 12 07 11" src="https://github.com/user-attachments/assets/03fb1ac6-a6a0-471d-8f5f-14c3a78a1431" />
</a>

<p align="center"><em>그림 4. 예측 오차(Actual – Predicted) 분포 히스토그램</em></p>

---

### 그림 5. 전체 지역 예측 결과 비교

<a href="https://github.com/user-attachments/assets/db6c9ea1-59f4-4f1b-a4d3-9200c57e1dba" target="_blank">
  <img width="1171" height="377" alt="스크린샷 2025-08-06 오후 12 08 20" src="https://github.com/user-attachments/assets/edf78a84-c578-4996-96fa-6ea4dc2c7d71" />
</a>
<p align="center"><em>그림 5. 전국 시군구별 실제 vs 예측 흡연율 추이 비교</em></p>

---

### 그림 6. 모델 성능 순위 (R²)

<a href="https://github.com/user-attachments/assets/f3e6d558-d749-4d62-8be2-a549397353c9" target="_blank">
  <img width="1107" height="679" alt="스크린샷 2025-08-06 오후 12 32 42" src="https://github.com/user-attachments/assets/e5afc9e3-796b-46c0-9887-5e5c2ec948de" />
  <img width="1107" height="647" alt="스크린샷 2025-08-06 오후 12 33 43" src="https://github.com/user-attachments/assets/e2f8ffb4-5930-4167-99f1-ed72c433eafc" />

</a>

<p align="center"><em>그림 6. 전체 모델 R² 순위 비교</em></p>


<!-- 순위 표는 별도로 -->
<div align="center">
  <img width="1102" height="332" alt="스크린샷 2025-08-06 오전 11 52 22" src="https://github.com/user-attachments/assets/7dc33a4e-7898-4182-be99-00fee6de797d" />
  <em>그림 7. 전체 모델 순위 표</em>
</div>

</div>


</div>

---

## 📝 4. 결론

### 4.1 한계점  
- 단일 연도(2023) 예측 시 R² 산출 한계  
- 사회·환경 변수 미반영  
- 소규모 지역 샘플 수 부족  

### 4.2 기대효과  
- 지자체 금연 정책 의사결정 지원  
- 예측 vs 실제 비교를 통한 정책 개선 인사이트  
- 추가 지표 결합으로 모델 확장 가능  

---
© 2025 SKN 16기 2차 단위프로젝트 1팀

