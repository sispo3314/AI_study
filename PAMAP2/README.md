# 📘 **PAMAP2 Physical Activity Monitoring Dataset**

📌 **출처:** UCI Machine Learning Repository / Universität Stuttgart
📌 **연도:** 2012
📌 **목적:**
세 개의 웨어러블 센서를 통해 **일상 활동(Physical Activity)을 자동 인식(HAR)** 하는 알고리즘을 개발하기 위함

---

## 👥 **참가자(Subjects)**

* 총 **9명(Subjects 1~9)**
* 남녀 혼합, 실내·실외 다양한 환경에서 실험 진행
* 각 참가자는 총 **12가지 주요 활동**(원본은 18개) 수행

---

# ⚙️ **실험 구성 (Experimental Setup)**

## 🎛️ **사용된 센서 장비**

PAMAP2는 **3개의 웨어러블 IMU + 1개의 심박 센서**를 사용함.

| 위치                 | 센서 구성                             |
| ------------------ | --------------------------------- |
| **손목(Hand)**       | 3축 가속도계 / 3축 자이로스코프 / 3축 자기계 / 온도 |
| **가슴(Chest)**      | 3축 가속도계 / 3축 자이로스코프 / 3축 자기계 / 온도 |
| **발목(Ankle)**      | 3축 가속도계 / 3축 자이로스코프 / 3축 자기계 / 온도 |
| **심박(HR monitor)** | 심박 센서 (Polar belt)                |

---

## 📍 **센서 부착 위치**

* **왼쪽 손목(Hand)**
* **가슴(Chest)**
* **발목(Ankle)**
  (각 센서에 동일한 IMU 구성)

---

## 📈 **수집 신호(Sensor Modalities)**

총 **17개 센서 채널 × 3부위 = 51개 + 온도 + 심박수** 등 수십 개의 RAW 채널 존재.

* 가속도(Acc): x,y,z
* 자이로(Gyro): x,y,z
* 자기계(Mag): x,y,z
* 온도(Temp)
* 심박수(HR)

---

## ⏱️ **샘플링 주파수**

* **100 Hz** (초당 100개 샘플)
* 일부 결측 존재 → forward/backward fill로 보정 필요

---

## 🧪 **전처리 과정 (Data Preprocessing)**

논문 및 오픈소스에서 PAMAP2 전처리는 다음을 따름:

1. **필요한 feature만 선택**
   (예: hand_acc_x, chest_gyro_y, ankle_mag_z 등)
2. **행동(activity)별로 NaN 보간**

   * `ffill`, `bfill`
   * 또는 linear interpolation
3. **Min-Max Scaling(0~1)**

   * 신호 특성상 StandardScaler보다 안정적
4. **슬라이딩 윈도우 생성**

   * 보통 **2초(200 샘플)** 또는 **2.56초(256 샘플)** 창
   * **50% 중첩** 권장
5. **Label encoding**(12-class)

---

# 🏃‍♂️ **수집된 활동(Activity Set)**

PAMAP2 원본: **18 classes**
일반 HAR 연구에서는 **12 classes** 사용 (논문 공통)

| ID                           | Activity          | 설명      |
| ---------------------------- | ----------------- | ------- |
| 0                            | Lying             | 눕기      |
| 1                            | Sitting           | 앉기      |
| 2                            | Standing          | 서 있기    |
| 3                            | Walking           | 걷기      |
| 4                            | Running           | 뛰기      |
| 5                            | Cycling           | 자전거 타기  |
| 6                            | Nordic Walking    | 노르딕 워킹  |
| 7                            | Watching TV       | TV 보기   |
| 8                            | Computer Work     | 컴퓨터 작업  |
| 9                            | Car Driving       | 운전      |
| 10                           | Ascending Stairs  | 계단 오르기  |
| 11                           | Descending Stairs | 계단 내려가기 |
| *(논문마다 12-class 체계가 약간씩 다름)* |                   |         |

---

# 🗂️ **데이터 파일 구조(File Structure)**

PAMAP2는 `.dat` 원본 형태로 제공.

예시 파일 구조:

```
PAMAP2_Dataset/
 ├── Protocol/
 │     ├── subject101.dat
 │     ├── subject102.dat
 │     └── ...
 ├── Optional/
 │     ├── subject106.dat
 │     ├── subject107.dat
 │     └── ...
 ├── pamap2ActivityLabels.pdf
 └── pamap2.pdf (데이터셋 상세 설명)
```

### `.dat` 파일 구성

* 각 row = timestamp + 3개 센서의 RAW values (총 60+ columns)
* 100 Hz로 연속 기록
* activityID는 실험자가 버튼으로 기록한 label

---

# 📊 **특징(Feature) 구성**

연구에서 보통 사용하는 33개 주요 RAW feature 예시:

* `hand_acc_x`, `hand_gyro_y`, `hand_mag_z`
* `chest_acc_x`, `chest_gyro_y`, `chest_mag_z`
* `ankle_acc_x`, `ankle_gyro_z`, `ankle_mag_x`
* * 온도, 심박

총 Feature 수는 연구 목적에 따라 **~30~50개 선택**
(원본은 60+ 컬럼)

---

# 💡 **요약**

| 항목     | 내용                            |
| ------ | ----------------------------- |
| 참가자 수  | **9명**                        |
| 활동 수   | **12~18개**                    |
| 샘플링 속도 | **100 Hz**                    |
| 윈도우 크기 | **2~3초(200~300 샘플)**          |
| 센서 위치  | 손목 / 가슴 / 발목                  |
| 센서 종류  | 가속도계 / 자이로스코프 / 자기계 / 온도 / 심박 |
| 파일 수   | Protocol + Optional 약 1GB     |
| 목적     | 웨어러블 기반 인간 활동 인식(HAR)         |

---

# 🎯 **활용 목적**

* 딥러닝 기반 HAR 연구(CNN, LSTM, Transformer)에 최적
* 스마트 헬스케어(낙상 감지, 비정상 행동 탐지)
* IoT · 웨어러블 디바이스 기반 행동 분석
* Multi-Sensor Fusion 연구의 레퍼런스 데이터셋

---
