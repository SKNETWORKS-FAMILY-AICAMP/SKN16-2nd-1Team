# 🎯 SKN16-2nd-1Team: 2023년 흡연율 예측 프로젝트

[![Python](https://img.shields.io/badge/Python-3.10-blue)](https://www.python.org/)  
[![Streamlit](https://img.shields.io/badge/Streamlit-v1.0-orange)](https://streamlit.io/)

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

### 2.4 데이터 전처리 결과서  
- **결측치 처리**: CPI·진료비 일부 → 중앙값 대체  
- **이상치 제거**: 흡연율 범위(0–100%) 벗어남 → 삭제  
- **라벨 인코딩**: `시도`, `시군구` → 숫자 코드  

---

### 2.5 모델 성능 비교 및 해석

#### 2.5.1 모델별 R² 비교  
<div align="center">
  <img width="987" height="590" alt="Image" src="https://github.com/user-attachments/assets/77656f4e-634d-46bc-9eb3-5446dae529c8" />
  <p><em>그림 4. 모델별 R²</em></p>
</div>

#### 2.5.2 최상위 모델(Gradient Boosting) 예측 성능  
- **산점도**: 실제 vs 예측  
- **히스토그램**: 오차 분포  
<div align="center">
  <img width="1489" height="390" alt="Image" src="https://github.com/user-attachments/assets/de415e93-5406-40ad-9aa0-434c96adcccf" />
  <p><em>그림 5. Gradient Boosting 예측 성능</em></p>
</div>

---

## 🚀 3. 시연 페이지  
Streamlit 웹 앱에서 제공하는 기능:  
- **데이터 확인**: 원본 데이터 미리보기 & 기초 통계  
- **연도별 트렌드**: 시도/시군구별 지표 시계열  
- **상관분석**: 변수 간 산점도·막대차트  
- **예측 및 튜닝**: 10개 모델 하이퍼파라미터 튜닝, 2023년 흡연율 실시간 예측  

<div align="center">
  <img src="assets/streamlit_preview.png" alt="Streamlit 시연 화면" width="700"/>
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

### 📂 폴더 구조 예시
```plain
SKN16-2nd-1Team/
├─ README.md
├─ requirements.txt
├─ app.py
├─ data/
│   └─ 시군구별_흡연률.csv
└─ assets/
    ├─ 1__.png
    ├─ 2__.png
    ├─ 3__top10.png
    ├─ 4__r2.png
    ├─ 5_Top_.png
    └─ streamlit_preview.png
© 2025 SKN 16기 2차 단위프로젝트 1팀
makefile
::contentReference[oaicite:0]{index=0}

ChatGPT에게 묻기
