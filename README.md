# 🩺 Cirrhosis Patient Survival Prediction – Manabu Style 프로젝트

이 프로젝트는 간경변증(Cirrhosis)을 앓고 있는 환자들의 생존 상태를 예측하는 분류 모델을 구축하는 것을 목표로 합니다. 머신러닝 알고리즘을 통해 환자가 살아 있는지(C), 간 질환으로 살아 있는지(CL), 사망했는지(D)를 예측합니다.

---

## 📦 데이터 설명

| 파일명              | 설명 |
|--------------------|------|
| `train.csv`        | 훈련 데이터, `Status` 컬럼은 예측 대상 클래스 |
| `test.csv`         | 테스트 데이터, `Status` 없음 |
| `sample_submission.csv` | 제출 파일 형식 예시 |

예측 대상(`Status`)은 다음 세 가지 클래스 중 하나입니다:
- `C`: 생존 (Censored)
- `CL`: 간 질환 관련 생존
- `D`: 사망

---

## ⚙️ 사용한 라이브러리

- pandas, numpy, seaborn, matplotlib
- scikit-learn
- XGBoost

---

## 🧼 데이터 전처리 과정

1. **이상치 제거**
   - `N_Days > 8000`, `Tryglicerides > 500`, `SGOT > 500`, `Platelets > 600`, `Prothrombin > 15` 제거
   - `Age`를 일 → 년 단위로 변환, 20세 미만, 100세 초과 제거

2. **결측치 처리**
   - 수치형: `median`으로 채움
   - 범주형: `mode`로 채움

3. **Label Encoding**
   - `Drug`, `Sex`, `Ascites`, `Hepatomegaly`, `Spiders`, `Edema` → LabelEncoder 사용

4. **Target Encoding**
   - `Status`: `{'C':0, 'CL':1, 'D':2}` 변환

5. **정규화**
   - `StandardScaler` 사용하여 스케일링

---

## 🧪 모델 학습 및 평가

```python
models = {
    "Decision Tree": DecisionTreeClassifier(),
    "Random Forest": RandomForestClassifier(),
    "XGBoost": XGBClassifier()
}
📊 평가지표
Accuracy

F1 Score (macro)

Log Loss

Confusion Matrix (heatmap)

🏆 모델 성능 요약
모델	Accuracy	F1 Macro	LogLoss
Decision Tree	0.7737	0.5700	8.1561
Random Forest	0.8600	0.5931	0.4414
✅ XGBoost (최종 선택)	0.8639	0.6613	0.4359

XGBoost 모델이 가장 우수한 성능을 보여 최종 제출에 사용되었습니다.
