## 📘 mHealth 데이터셋

* **출처:** University of Granada (스페인)
* **연도:** 2013년
* **목적:** 웨어러블 센서로 사람의 신체 움직임과 생체 신호(심전도 포함)를 기록하여,
  **신체 활동(activity)** 을 분류하거나 분석하는 연구에 활용하기 위함
* **참가자(subject):** 총 **10명 (subject1 ~ subject10)**
  -> 각 사람의 데이터가 **별도 파일(`mHealth_subjectX.log`)** 로 저장되어 있음.

---

## ⚙️ 실험 구성 (Experimental Setup)

* **수집 데이터:**

  * **신체 움직임(Motion) + 생체신호(Vital signs)**
  * **10명의 자원자**가 12가지 일상 활동을 수행
* **센서 장비:** `Shimmer2` 웨어러블 센서
* **센서 부착 위치:**

  * **가슴(chest)**
  * **오른쪽 팔목(right wrist)**
  * **왼쪽 발목(left ankle)**
* **측정 항목:**

  * 가속도(acceleration)
  * 자이로스코프(회전 속도, rate of turn)
  * 자기장(magnetic field orientation)
  * ECG (심전도, 2-lead)
* **샘플링 주파수:** 50Hz (즉, 초당 50개의 데이터 포인트 기록)
* **참고:** ECG는 활동 분류 모델에 직접 사용되지 않았지만,
  향후 심장 상태 분석 등의 목적을 위해 함께 기록됨.

---

## 🏃‍♂️ 수집된 활동 (Activity set)

| Label | Activity                  | 설명              |
| ----- | ------------------------- | --------------- |
| L1    | Standing still            | 가만히 서 있기 (1분)   |
| L2    | Sitting and relaxing      | 앉아서 쉬기 (1분)     |
| L3    | Lying down                | 누워 있기 (1분)      |
| L4    | Walking                   | 걷기 (1분)         |
| L5    | Climbing stairs           | 계단 오르기 (1분)     |
| L6    | Waist bends forward       | 허리 숙이기 (20회)    |
| L7    | Frontal elevation of arms | 팔 앞쪽으로 들기 (20회) |
| L8    | Knees bending (crouching) | 무릎 굽히기 (20회)    |
| L9    | Cycling                   | 자전거 타기 (1분)     |
| L10   | Jogging                   | 조깅 (1분)         |
| L11   | Running                   | 달리기 (1분)        |
| L12   | Jump front & back         | 앞뒤로 점프 (20회)    |

※ 괄호 안 숫자는 활동 시간(분) 또는 반복 횟수

---

## 🗂️ 데이터 파일 구조

각 참가자의 데이터는 별도의 로그 파일에 저장됨

* 예: `mHealth_subject1.log`, `mHealth_subject2.log`, … `mHealth_subject10.log`
* 각 행(row)은 한 시점의 측정값 (50Hz → 초당 50행)
* 각 열(column)은 센서 신호

---

## 📊 컬럼(열) 구조

| 열 번호  | 내용                         | 센서 위치 | 단위        |
| ----- | -------------------------- | ----- | --------- |
| 1–3   | 가속도 (X, Y, Z)              | 가슴    | m/s²      |
| 4–5   | ECG (심전도 2채널)              | 가슴    | mV        |
| 6–8   | 가속도 (X, Y, Z)              | 왼쪽 발목 | m/s²      |
| 9–11  | 자이로스코프 (X, Y, Z)           | 왼쪽 발목 | deg/s     |
| 12–14 | 자기장 (X, Y, Z)              | 왼쪽 발목 | —         |
| 15–17 | 가속도 (X, Y, Z)              | 오른쪽 팔 | m/s²      |
| 18–20 | 자이로스코프 (X, Y, Z)           | 오른쪽 팔 | deg/s     |
| 21–23 | 자기장 (X, Y, Z)              | 오른쪽 팔 | —         |
| 24    | **활동 라벨 (0 = null class)** | —     | 정수(label) |

---

## 💡 요약

* **총 데이터 수:** 10명의 피험자 × 각자 12개 활동 × 초당 50샘플
* **센서 수:** 3개 (가슴, 오른팔, 왼발)
* **특징(feature) 수:** 23개 센서 채널 + 1개 라벨 = **24개 컬럼**
* **사용 목적:**

  * CNN, RNN, Transformer 등으로 사람의 활동(Activity)을 인식하는 연구
  * ECG를 포함하면 헬스케어(mHealth) 분석까지 확장 가능

---
