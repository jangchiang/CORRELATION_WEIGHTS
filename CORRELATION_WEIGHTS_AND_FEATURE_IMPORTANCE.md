# Correlation Weights และ Feature Importance สำหรับ Dominant Factor Analysis

**วันที่สร้าง:** 9 ธันวาคม 2025
**Model Version:** v12.0 (Manual Stacking Ensemble)
**วัตถุประสงค์:** ข้อมูลสำหรับหา Dominant Factor ของแต่ละ Load Type

---

## 1. สรุป Dominant Factors (ภาพรวม)

| Rank | Feature | เฉลี่ยทุก Model | หมายเหตุ |
|------|---------|-----------------|----------|
| **1** | **Height** | **41.0%** | ปัจจัยสำคัญที่สุด |
| **2** | **c' (Cohesion)** | **29.7%** | ปัจจัยสำคัญอันดับ 2 |
| 3 | Rain_Duration_hr | 7.4% | ผลกระทบปานกลาง |
| 4 | Rainfall_Duration | 7.3% | ผลกระทบปานกลาง |
| 5 | Rain_Intensity_mm | 6.4% | ผลกระทบปานกลาง |
| 6 | Angle (Slope) | 3.9% | ผลกระทบต่ำ |
| 7 | γ_unsat | 2.7% | ผลกระทบต่ำ |
| 8 | γ_sat | 1.6% | ผลกระทบต่ำมาก |

**สรุป:** Height และ Cohesion รวมกัน = **70.7%** ของ Feature Importance ทั้งหมด

---

## 2. Correlation Weights (จาก Correlation Matrix กับ FS)

ค่า Correlation Weight คำนวณจาก `corr()['FS']` แล้ว normalize

### 2.1 UC Model (Uplift + Compressive)

| Feature | Correlation Weight |
|---------|-------------------|
| c' (Cohesion) | **0.3064** |
| Height | **0.2925** |
| Rain_Duration_hr | 0.1154 |
| Rainfall_Duration | 0.1154 |
| Rain_Intensity_mm | 0.0783 |
| Angle | 0.0575 |
| γ_unsat | 0.0308 |
| γ_sat | 0.0035 |

**UC Dominant:** c' > Height > Rainfall parameters

---

### 2.2 UU Model (Uplift + Uplift)

| Feature | Correlation Weight |
|---------|-------------------|
| c' (Cohesion) | **0.3653** |
| Height | **0.3289** |
| Rain_Duration_hr | 0.0854 |
| Rainfall_Duration | 0.0854 |
| Rain_Intensity_mm | 0.0579 |
| Angle | 0.0435 |
| γ_sat | 0.0271 |
| γ_unsat | 0.0067 |

**UU Dominant:** c' > Height (ใกล้เคียงกัน)

---

### 2.3 CC Model (Compressive + Compressive)

| Feature | Correlation Weight |
|---------|-------------------|
| Height | **0.3459** |
| c' (Cohesion) | **0.2217** |
| Rain_Duration_hr | 0.1248 |
| Rainfall_Duration | 0.1248 |
| Angle | 0.0822 |
| Rain_Intensity_mm | 0.0720 |
| γ_unsat | 0.0256 |
| γ_sat | 0.0029 |

**CC Dominant:** Height > c' > Rainfall Duration

---

### 2.4 CU Model (Compressive + Uplift)

| Feature | Correlation Weight |
|---------|-------------------|
| Height | **0.4677** |
| c' (Cohesion) | **0.3633** |
| Angle | 0.1049 |
| Rain_Duration_hr | 0.0160 |
| Rainfall_Duration | 0.0160 |
| γ_unsat | 0.0173 |
| Rain_Intensity_mm | 0.0109 |
| γ_sat | 0.0040 |

**CU Dominant:** Height > c' (Height มีอิทธิพลมากที่สุด)

---

## 3. XGBoost Feature Importance (จาก Model)

ค่า Feature Importance คำนวณจาก XGBoost `feature_importances_`

### 3.1 UC Model

| Rank | Feature | Importance (%) |
|------|---------|----------------|
| 1 | c' (Cohesion) | 27.45% |
| 2 | Height | 24.24% |
| 3 | Rainfall_Duration | 16.56% |
| 4 | Rain_Intensity_mm | 10.04% |
| 5 | Rain_Duration_hr | 9.56% |
| 6 | γ_unsat | 6.92% |
| 7 | Angle | 3.57% |
| 8 | γ_sat | 1.65% |

**Rainfall Total Impact (UC):** 36.16%

---

### 3.2 UU Model

| Rank | Feature | Importance (%) |
|------|---------|----------------|
| 1 | Height | **43.46%** |
| 2 | c' (Cohesion) | **39.51%** |
| 3 | Rain_Duration_hr | 5.40% |
| 4 | Rain_Intensity_mm | 4.01% |
| 5 | Angle | 2.43% |
| 6 | Rainfall_Duration | 2.33% |
| 7 | γ_sat | 1.84% |
| 8 | γ_unsat | 1.01% |

**Height + c' Total (UU):** 82.97%

---

### 3.3 CC Model

| Rank | Feature | Importance (%) |
|------|---------|----------------|
| 1 | Height | **38.96%** |
| 2 | c' (Cohesion) | 20.53% |
| 3 | Rain_Duration_hr | 12.07% |
| 4 | Rain_Intensity_mm | 9.83% |
| 5 | Rainfall_Duration | 9.13% |
| 6 | Angle | 5.98% |
| 7 | γ_unsat | 2.03% |
| 8 | γ_sat | 1.48% |

**Rainfall Total Impact (CC):** 31.03%

---

### 3.4 CU Model

| Rank | Feature | Importance (%) |
|------|---------|----------------|
| 1 | Height | **57.32%** |
| 2 | c' (Cohesion) | **31.23%** |
| 3 | Angle | 3.79% |
| 4 | Rain_Duration_hr | 2.65% |
| 5 | γ_sat | 1.56% |
| 6 | Rain_Intensity_mm | 1.51% |
| 7 | Rainfall_Duration | 1.32% |
| 8 | γ_unsat | 0.63% |

**Height + c' Total (CU):** 88.55%

---

## 4. สรุปเปรียบเทียบทุก Load Type

### 4.1 Feature Importance เปรียบเทียบ (%)

| Feature | UC | UU | CC | CU | Average |
|---------|-----|-----|-----|-----|---------|
| **Height** | 24.24 | **43.46** | **38.96** | **57.32** | **41.00** |
| **c' (Cohesion)** | **27.45** | **39.51** | 20.53 | 31.23 | **29.68** |
| Rain_Duration_hr | 9.56 | 5.40 | 12.07 | 2.65 | 7.42 |
| Rainfall_Duration | 16.56 | 2.33 | 9.13 | 1.32 | 7.34 |
| Rain_Intensity_mm | 10.04 | 4.01 | 9.83 | 1.51 | 6.35 |
| Angle | 3.57 | 2.43 | 5.98 | 3.79 | 3.94 |
| γ_unsat | 6.92 | 1.01 | 2.03 | 0.63 | 2.65 |
| γ_sat | 1.65 | 1.84 | 1.48 | 1.56 | 1.63 |

---

### 4.2 Dominant Factor ของแต่ละ Load Type

| Load Type | Dominant Factor #1 | Dominant Factor #2 | รวม Top 2 |
|-----------|--------------------|--------------------|-----------|
| **UC** | c' (27.45%) | Height (24.24%) | 51.69% |
| **UU** | Height (43.46%) | c' (39.51%) | 82.97% |
| **CC** | Height (38.96%) | c' (20.53%) | 59.49% |
| **CU** | Height (57.32%) | c' (31.23%) | 88.55% |

---

### 4.3 Rainfall Sensitivity ของแต่ละ Load Type

| Load Type | Total Rainfall Impact | Sensitivity |
|-----------|----------------------|-------------|
| **UC** | 36.16% | สูง (Sensitive to rainfall) |
| **CC** | 31.03% | ปานกลาง-สูง |
| **UU** | 11.74% | ปานกลาง |
| **CU** | 5.48% | ต่ำ (Least sensitive) |

---

## 5. ข้อสรุปสำหรับการวิเคราะห์ Dominant Factor

### 5.1 ปัจจัยหลัก (Dominant Factors)

1. **Height (ความสูงของ Slope)**
   - มีผลกระทบมากที่สุดในทุก Load Type (ยกเว้น UC)
   - CU Model: สูงสุดที่ 57.32%
   - ค่าเฉลี่ย: 41.00%

2. **c' (Cohesion - แรงยึดเกาะของดิน)**
   - มีผลกระทบอันดับ 2 ในเกือบทุก Load Type
   - UC Model: สูงสุดที่ 27.45%
   - ค่าเฉลี่ย: 29.68%

### 5.2 ปัจจัยรอง

3. **Rainfall Parameters (รวม 3 ตัว)**
   - UC Model ไวต่อฝนมากที่สุด (36.16%)
   - CU Model ไวต่อฝนน้อยที่สุด (5.48%)

4. **Slope Angle**
   - มีผลกระทบค่อนข้างต่ำ (3-6%)
   - CC Model ไวต่อมุมมากที่สุด

### 5.3 ปัจจัยที่มีผลกระทบต่ำ

- **γ_unsat, γ_sat (Density):** < 3% ในทุก Model
- **Offset, Diff_Leg, φ' (phi), E₅₀_ref:** ไม่มีผลกระทบ (0%)

---

