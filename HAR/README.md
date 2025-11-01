📘 **UCI HAR 데이터셋**

출처: University of California, Irvine (UCI Machine Learning Repository)
연도: 2012년
목적: 스마트폰 내 센서(가속도계·자이로스코프)를 이용하여 사용자의 **일상 활동(Activity)** 을 자동 인식하는 모델을 개발하기 위함
참가자(Subjects): 총 **30명** (ID: 1~30)
각 참가자가 다양한 일상 동작을 수행하면서 스마트폰을 **허리에 착용**한 상태로 데이터 수집

---

⚙️ **실험 구성 (Experimental Setup)**

**센서 장비:**

* **스마트폰 Samsung Galaxy S II**
* 내장 센서: 3축 가속도계(Accelerometer) + 3축 자이로스코프(Gyroscope)

**센서 부착 위치:**

* 허리 (waist)

**수집 신호:**

* 가속도 (acceleration: 중력 + 몸통)
* 자이로스코프 (angular velocity)

**샘플링 주파수:**

* **50Hz (초당 50개의 데이터 포인트)**

**전처리 과정:**

* 중력 성분 분리를 위해 **Butterworth 저역통과 필터(0.3Hz cutoff)** 사용
* 신호를 **2.56초 길이(128 샘플)**의 윈도우로 분할, **50% 중첩** 처리

---

🏃‍♂️ **수집된 활동 (Activity Set)**

| Label | Activity           | 설명      |
| :---: | :----------------- | :------ |
|   L1  | WALKING            | 걷기      |
|   L2  | WALKING_UPSTAIRS   | 계단 오르기  |
|   L3  | WALKING_DOWNSTAIRS | 계단 내려가기 |
|   L4  | SITTING            | 앉기      |
|   L5  | STANDING           | 서 있기    |
|   L6  | LAYING             | 눕기      |

※ 각 활동은 짧은 구간 동안 반복적으로 수행되었으며, 훈련·테스트용으로 무작위 분할됨 (70% / 30%)

---

🗂️ **데이터 파일 구조 (File Structure)**

모든 데이터는 `UCI HAR Dataset/` 디렉토리 내에 저장되어 있으며,
**train/** 과 **test/** 두 하위 폴더로 구성.

| 파일명                                                 | 내용                                   |
| --------------------------------------------------- | ------------------------------------ |
| `README.txt`                                        | 데이터셋 설명 파일                           |
| `features_info.txt`                                 | 각 특징(feature)의 의미 및 계산 방식            |
| `features.txt`                                      | 561개 특징 이름 목록                        |
| `activity_labels.txt`                               | 활동 ID와 이름 매핑 (1~6)                   |
| `train/X_train.txt` / `test/X_test.txt`             | 각 샘플의 561차원 특징 벡터                    |
| `train/y_train.txt` / `test/y_test.txt`             | 각 샘플의 활동 라벨                          |
| `train/subject_train.txt` / `test/subject_test.txt` | 각 샘플의 피험자 ID (1~30)                  |
| `Inertial Signals/`                                 | 원시(time series) 센서 신호 (가속도/자이로 각 축별) |

---

📊 **특징(Feature) 구성**

총 **561개 특징(columns)** 으로 구성
→ 시간영역(Time domain) + 주파수영역(Frequency domain) 신호 결합

| 예시 변수명                            | 설명                     |
| :-------------------------------- | :--------------------- |
| `tBodyAcc-mean()-X`               | 몸통 가속도 X축의 평균 (시간영역)   |
| `tBodyAcc-std()-Z`                | 몸통 가속도 Z축의 표준편차        |
| `tGravityAcc-mean()-Y`            | 중력 가속도 Y축의 평균          |
| `fBodyGyro-meanFreq()-X`          | 몸통 자이로 X축의 평균 주파수      |
| `angle(tBodyAccMean,gravityMean)` | 몸통 평균 가속도와 중력 평균 간의 각도 |

> 접두사 `t`는 시간영역(time), `f`는 주파수영역(frequency)을 의미함
> `mean()` = 평균, `std()` = 표준편차, `Mag` = 크기 벡터(magnitude)

---

💡 **요약**

| 항목     | 내용                     |
| ------ | ---------------------- |
| 참가자 수  | 30명                    |
| 활동 수   | 6가지                    |
| 샘플링 속도 | 50Hz                   |
| 윈도우 크기 | 2.56초 (128샘플, 50% 중첩)  |
| 특징 수   | 561개                   |
| 센서 위치  | 허리 (스마트폰 내장)           |
| 센서 종류  | 가속도계, 자이로스코프           |
| 파일 수   | 약 30MB (train/test 포함) |

---

🎯 **사용 목적**

* 사람의 활동(Activity)을 분류하는 **기계학습/딥러닝 모델 학습**
* SVM, Random Forest, CNN, LSTM, Transformer 기반의 HAR 연구에 널리 사용
* 센서 융합 기반의 **스마트 헬스케어 및 행동 인식 연구**의 대표 데이터셋

---
