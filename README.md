<div align="center">

<img width="262" height="74" alt="shanntilogo" src="https://github.com/user-attachments/assets/d4a06c2d-c7f8-4012-9cab-6f8c524ff3c4" />

**한국정보통신학회논문지(JKIICE)**  
*「영상 분석 기반 운동 자세 교정 및 유산소 게임 애플리케이션」*

<br>

[![Android](https://img.shields.io/badge/Platform-Android_API_26+-3DDC84?style=flat-square&logo=android&logoColor=white)](https://developer.android.com)
[![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)](https://www.java.com)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-GPU_Pipeline-4285F4?style=flat-square&logo=google&logoColor=white)](https://mediapipe.dev)
[![Firebase](https://img.shields.io/badge/Firebase-Auth_·_RTDB-FFCA28?style=flat-square&logo=firebase&logoColor=black)](https://firebase.google.com)
[![논문게재](https://img.shields.io/badge/📄_JKIICE-한국정보통신학회논문지_게재-0066CC?style=flat-square)](https://www.kiice.org)

</div>

---

<br>

| 항목 | 내용 |
|:---:|:---|
| 🎯 목적 | 카메라 기반 **실시간 자세 교정** + **유산소 게임**으로 운동 접근성·지속성 향상 |
| 🧩 핵심 구성 | 얼굴 인식 로그인 · MediaPipe 자세 분석 · 아두이노 에어마우스 게임 · Firebase 기록 |
| 📌 운동 종류 | 스쿼트 · 플랭크 · 런지 · 밸런스 |

---

## Ⅰ. 왜 만들었나

헬스장에서 혼자 운동할 때 자세가 올바른지 실시간으로 확인할 방법이 없다. PT 트레이너는 비용이 크고, 운동 후 녹화 영상을 돌려보는 방식은 교정 타이밍을 놓친다. **카메라 하나로 운동하는 동안 즉각 피드백**을 주는 앱이 필요하다고 판단했다.

---

## Ⅱ. 주요 기능 및 기술 스택

<table>
<tr>
<td width="50%" valign="top">

### 🏋️ 실시간 운동 자세 분석

- 스쿼트 · 플랭크 · 런지 · 밸런스 **4종** 실시간 분석
- MediaPipe GPU 파이프라인으로 **33개 신체 랜드마크** 추출
- 코사인 제2법칙 기반 관절 각도 판정 — 딥러닝 추론 없음
- 각 관절별 🟢 정상 / 🔴 비정상 / ⚫ 미감지 **3단계 시각화**

</td>
<td width="50%" valign="top">

### 🔊 한국어 TTS 실시간 피드백

- 잘못된 자세 감지 즉시 한국어 음성 교정 안내
- 부위별 맞춤 피드백 ("무릎을 더 굽혀주세요", "허리를 펴주세요")
- 판정 주기 **500ms** — 체감 지연 없는 실시간 반응
- 1회 분석 세션 5,000ms 고정 윈도우

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🎮 얼굴 인식 미니게임

- ML Kit 얼굴 좌표로 조종하는 **슈팅게임/캐릭터 잡기 게임**
- 정확한 자세 카운팅 — 스쿼트 5회 · 런지 5회
- **밸런스 10초 유지** 챌린지
- 점수 Firebase RTDB 저장 → 실시간 랭킹

</td>
<td width="50%" valign="top">

### 📊 통계 & 기록

- 운동 시간 · 소모 칼로리 **일별 캘린더** (MPAndroidChart)
- 세션별 자세 교정 성공률 추적
- Room DB 로컬 저장 + Firebase RTDB 클라우드 동기화

</td>
</tr>
</table>

---

| 분류 | 기술 | 비고 |
|------|------|------|
| **언어** | Java · Kotlin · C++ | JNI 네이티브 연동 |
| **카메라** | CameraX API | 실시간 프레임 캡처 |
| **포즈 추적** | Google MediaPipe | GPU 가속 파이프라인 |
| **ML 추론** | TensorFlow Lite 2.7.0 | 자세 유사도 분류 보조 |
| **얼굴 감지** | Google ML Kit | 미니게임 조작 입력 |
| **이미지 처리** | OpenCV (Native JNI) | C++ 네이티브 연동 |
| **인증** | Firebase Authentication | 이메일 / 비밀번호 |
| **클라우드 DB** | Firebase Realtime Database | 점수 · 랭킹 실시간 동기화 |
| **로컬 DB** | Room Database | 운동 세션 기록 |
| **차트** | MPAndroidChart v3.1.0 | 운동 통계 시각화 |
| **음성** | Android TTS | 한국어 실시간 피드백 |
| **Min / Target SDK** | 26 / 34 | Android 8.0+ |

---

## Ⅲ. 시스템 설계 및 구현 🛠️

### 3.1 시스템 구조

본 시스템의 핵심은 다음과 같다.

| 구성 요소 | 역할 |
|:---|:---|:---:|
| **ML Kit** | 얼굴 인식 로그인 |
| **MediaPipe Pose Detection** | 관절 각도 기반 자세 분석 |
| **아두이노 에어마우스** | 유산소 게임 입력(블루투스) |
| **Firebase** | 사용자·운동 기록 저장 |

- 사용자 정보와 얼굴 정보는 **MediaPipe Face Detection**을 활용해 **Firebase**에 저장·관리한다.  
- 로그인 후 **MediaPipe Pose Detection**으로 자세 분석·운동을 수행한다.  
- 게임 실행 시 **가속도·자이로 센서**가 장착된 아두이노 에어마우스를 **블루투스**로 연결한다.  
- 운동·게임 종료 후 **운동 시간·소모 칼로리** 등이 Firebase에 저장되어 기록에서 확인한다.

<div align="center">
<img width="268" height="153" alt="image (1)" src="https://github.com/user-attachments/assets/90dfbd22-d955-4367-b087-15c2c868ded3" />
</div>

---

### 3.2 얼굴 인식 로그인 👤

얼굴 인식 로그인은 **ML Kit**을 이용해 구현하였으며, 얼굴의 고유 특징을 분석해 신원을 확인한다.

**처리 흐름 (요약)**

| 단계 | 내용 |
|:---:|:---|
| 1️⃣ | 카메라로 실시간 이미지 캡처 |
| 2️⃣ | 전처리(형식 변환, 크기·해상도 조정) |
| 3️⃣ | 학습된 모델로 얼굴 감지 |
| 4️⃣ | 바운딩 박스 생성 |
| 5️⃣ | 다수 얼굴 감지 가능하나 **단일 얼굴만** 감지하도록 수정 |
| 6️⃣ | 바운딩 박스 좌표 **[0,1] 정규화** + 랜드마크 식별 → 임베딩 벡터 |

**등록·로그인 절차**

| 구분 | 설명 |
|:---|:---|
| 📝 **등록** | 최초 캡처 → 전처리·감지·랜드마크 → 특징 **암호화 저장** |
| 🔓 **로그인** | 동일 전처리·감지 → DB 특징과 **유사도 비교** |

유사도는 **유클리드 거리** 기반으로 비교하며, 데이터셋 분석으로 **평균·표준편차**를 고려한 임계값을 적용한다.

| Landmark | Registered Face (X,Y) | Detected A (X,Y) | Detected B (X,Y) |
|:---:|:---:|:---:|:---:|
| 👁️ Left Eye | (75,150) | (77,152) | (85,160) |
| 👁️ Right Eye | (175,150) | (177,152) | (185,160) |
| 👃 NoseBase | (125,225) | (127,227) | (135,235) |

예시에서 Left Eye 거리가 작은 A는 로그인 가능, B는 불가.

---

### 3.3 운동 자세 교정 🦴

**MediaPipe Pose Detection**으로 실시간 자세를 측정하고, 관절 랜드마크 좌표로 **관절 각도**를 분석한다. 특정 부위 각도가 기준과 일치하는지 판단해 **자세 교정**을 지원한다.

| 방법 | 특징 | 본 연구 선택 |
|:---|:---|:---:|
| 📏 **룰베이스** | 기준 각도와 실제 각도 비교 → 실시간 오류 감지 | ✅ 채택 |
| 🧠 **BPE(임베딩)** | 시계열·유사도 중심, 완료 후 점수 제공 성향 | ❌ 실시간 교정 목적에 부적합 판단 |

또한 **자세 교정**과 **실제 운동**으로 **모드를 분리**하여 개발하였다.

| 상태 | 색 | 의미 |
|:---:|:---:|:---|
| 🟢 | 초록 | 정확한 자세(허용 범위 내) |
| 🔴 | 빨강 | 잘못된 자세 |
| ⚫ | 회색 | 인식되지 않음 |

운동별로 **우선 교정 부위**를 선정하고, 중요 부위는 **오차 범위를 좁게**, 보조 부위는 **넓게** 설정하여 자세의 다양성을 수용.

**📌 스쿼트**

| 부위 | 연결(개념) | 기준 각도 / 허용 | 중요도 |
|:---|:---|:---|
| 머리 | ear–shoulder–hip | 180° (±50°) | 하 |
| 척추 | shoulder–hip–knee | 75° (±20°) | 상 |
| 팔 | shoulder–elbow–wrist | 180° (±30°) | 하 |
| 다리 | hip–knee–ankle | 65° (±20°) | 상 |

**📌 플랭크**

| 부위 | 연결(개념) | 기준 각도 / 허용 | 중요도 |
|:---|:---|:---|
| 머리 | eye–shoulder–hip | 180° (±50°) | 하 |
| 척추 | eye–shoulder–hip | 180° (±20°) | 상 |
| 팔꿈치 | shoulder–elbow–wrist | 90° (±20°) | 상 |

**📌 런지**

| 부위 | 연결(개념) | 기준 각도 / 허용 | 중요도 |
|:---|:---|:---|
| 머리 | ear–shoulder–hip | 180° (±50°) | 하 |
| 척추 | ear–shoulder–hip | 180° (±20°) | 상 |
| 팔 | shoulder–elbow–wrist | 60° (±40°) | 하 |
| 다리 | hip–knee–ankle 등 | (좌/우 예시) 90°/100° (±20°) | 상 |

**📌 밸런스**

| 부위 | 연결(개념) | 기준 각도 / 허용 | 중요도 |
|:---|:---|:---|
| 머리 | ear–shoulder–hip | 180° (±50°) | 하 |
| 척추·다리 | ear–hip–knee 등 | 150° (±30°) | 상 |
| 팔·손 | elbow–thumb 등 | 180° (±20°) 등 | 상 |

교정 메시지는 “조금 더 굽혀주세요 / 덜 굽혀주세요”처럼 **방향성**을 주어 음성·영상으로 실시간 교정을 돕는다.

<div align="center">
<img width="563" height="283" alt="image" src="https://github.com/user-attachments/assets/8c4e895a-2f57-420e-9693-625e14f21f34" />
</div>

**난이도**는 사용자 선택에 따라 **쉬움·보통·어려움**으로 설정할 수 있으며, 운동 횟수(스쿼트·런지)와 유지 시간(플랭크·밸런스)을 달리 설정할 수 있다.

**반복 운동(스쿼트·런지) 카운팅**  
- 예: 스쿼트에서 다리 각도 **160° 이상**을 서 있는 상태, **100° 이하**를 앉은 상태로 인식  
- **160° 이상 → 100° 이하 → 160° 이상**으로 복귀하면 **1회** 증가

<div align="center">
<img alt="Screenshot_20240613-170543" src="https://github.com/user-attachments/assets/b7933ffc-d69d-4cba-9b10-ddaa0270bcfa" />
</div>

**유지 운동(플랭크·밸런스) **  
설정 시간 내 기준 자세와 일치한 시간 비율로 정확도를 평가한다.  
머리·몸통·팔·다리 중 하나라도 벗어나면 시간에 비례해 정확도가 차감된다.

---

### 3.4 유산소 게임 애플리케이션 🎮

유산소 게임은 **아두이노–안드로이드 블루투스** 기반 **에어마우스·움직임 감지**로 구현한다.

**하드웨어 구성**

| 구성 | 설명 |
|:---|:---|
| 🧩 아두이노 우노 | 메인 보드 |
| 📡 MPU-6050 | 3축 자이로 + 3축 가속도 |
| 🔋 배터리 | 전원 |
| 📶 HC-06 | 블루투스 모듈 |
| 🔘 버튼 | 입력 |

아두이노는 MPU-6050 데이터를 읽고 **SoftwareSerial**로 블루투스와 통신하며, 약 **2Hz** 간격으로 안드로이드에 전송한다. 안드로이드는 수신 데이터로 **마우스 포인터**를 표시한다.

가속도는 **움직임 정도**, 자이로는 **회전·방향** 추정에 사용한다. 가속도 임계값 초과 시 이벤트를 전송하며, 천천히 걷기 **🚶 walk**, 빠르게 뛰기 **🏃 run** 로 구분한다.

---

#### 3.4.1 연예인을 잡아라 게임 🎯

HC-06 + MPU-6050 기반 **에어마우스**로 구현한다.

| 요소 | 내용 |
|:---|:---|
| 🎚️ 난이도 | 초급·중급·고급(연예인 등장 속도 차이) |
| 🖱️ 조작 | 에어마우스로 무작위 등장 목표 포착 |
| 🔘 버튼 | 중앙 재배치 / 클릭(위치 일치 시 점수) |
| 🏋️ 미션 | 도중 스쿼트·런지·밸런스 등 운동 과제 |
| ✅ 판정 | **3.3 운동 자세 교정**과 동일 원리 |
| 🏆 랭킹 | 최종 점수로 랭킹 확인 |

<div align="center">
<img width="422" height="205" alt="임영웅게임" src="https://github.com/user-attachments/assets/4b046689-cbf6-4c99-a946-4731f9e40309" />
<img alt="Screenshot_20240613-170900" src="https://github.com/user-attachments/assets/bbf22e75-ac4f-44df-a0fe-c7b94f8f4ac8" />
</div>

---

#### 3.4.2 우주선과의 전쟁 게임 🚀

| 요소 | 내용 |
|:---|:---|
| 🚶🆚🏃 | 걷기/뛰기에 따라 캐릭터 속도 변화 |
| 😐 얼굴 위치 | **3.2** MLKit로 얼굴 위치 감지 → 캐릭터 좌우 조정 |
| ⭐ 점수 | 스쿼트 수행 시 추가 점수, 우주선 적중 시 점수 등(논문 본문) |
| 🏆 랭킹 | 최종 점수로 랭킹 확인 |

<div align="center">
<img width="1046" height="251" alt="화면 캡처 2026-04-01 191423" src="https://github.com/user-attachments/assets/b80f5dfc-509f-4f60-8276-73e75ffe07b4" />
</div>

---

## 📎 자료 링크

| 자료 | 링크 |
|------|------|
| 📄 앱 소개 정리| [Google Drive](https://drive.google.com/file/d/1n9ZdxNrJpgKZWSckWvXcWaxxq_yf7haF/view?usp=sharing) |

---

<div align="center">

Made by Team SHANNTI 💪

</div>
