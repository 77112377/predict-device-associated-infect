# Machine learning algorithms for prediction of device-associated infection in neurocritically ill patients
呼吸器相關肺炎（ Ventilator-Associated Pneumonia, VAP）是ICU中常見且嚴重的併發症， 雖然臨床上已有風險評估指標APSIII，但其設計並非針對神經
加護病人。因此本研究聚焦於神經加護病人族群， 建立針對該族群進行VAP預測的機器學習模型，作為臨床上APSIII的補充，協助早期辨識高風險病人 。

## Project Files Overview
- `code.ipynb`：主程式碼，進行預測與 SHAP 分析  
- `all_data_4_17.csv`：經預處理後的特徵資料集
- `sql/`：用於從MIMIC-IV提取cohort的SQL
  - `chronic_comorbidity.txt`
  - `firstlab.txt`
  - `patient_demographic.txt`
  - `stroke_cohort.txt`
  - `vitalsign.txt`

## Usage
### Dataset
資瞭來源使用MIMIC-IV，使用SQL提取cohort以及特徵，主要包含以下內容:
- `sql/stroke_cohort.txt`：建立神經加護病房病人族群
- `sql/patient_demographic.txt`：人口統計資訊
- `sql/firstlab.txt`：首次實驗室檢查
- `sql/vitalsign.txt`：生命徵象紀錄
- `sql/chronic_comorbidity.txt`：慢性病紀錄

這些資料合併並前處理後，儲存為 `all_data_4_17.csv`，供模型訓練使用。

### How to run
將`all_data_4_17.csv`放在與`code.ipynb`相同的目錄下，並依序執行`code.ipynb`的每個block，流程包含:
- 資料載入與前處理（缺失值處理、特徵提取）
- Upsampling(ADASYN)
- 模型訓練(XGBoost、LightGBM等)
- SHAP特徵重要性分析
- Top-K特徵重訓

## Result Summary
- 機器學習模型整體表現優於臨床常用的APSIII指標，其中XGBoost與LightGBM為最佳表現者(AUC = 0.69）

| Model               | AUC (95% CI)      |
|---------------------|-------------------|
| APSIII              | 0.61              |
| XGBoostClassifier   | 0.69 (0.60–0.78)  |
| LightGBMClassifier  | 0.69 (0.59–0.78)  |
| RandomForest        | 0.68 (0.59–0.76)  |
| AdaBoostClassifier  | 0.65 (0.55–0.73)  |
| DecisionTree        | 0.63 (0.52–0.71)  |
| KNN                 | 0.54 (0.47–0.66)  |
