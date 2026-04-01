<div align="center">

<br>

# 🧘‍♂️ SHANNTI

### 실시간 AI 운동 자세 분석 · 교정 안드로이드 애플리케이션

<br>

[![Android](https://img.shields.io/badge/Platform-Android_API_26+-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com)
[![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)](https://www.java.com)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-GPU_Pipeline-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://mediapipe.dev)
[![Firebase](https://img.shields.io/badge/Firebase-Auth_·_RTDB-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com)
[![논문게재](https://img.shields.io/badge/📄_JKIICE-한국정보통신학회논문지_게재-0066CC?style=for-the-badge)](https://www.kiice.org)

<br>

> **딥러닝 추론 없이, 코사인 제2법칙 수학 연산만으로 매 프레임 자세를 판정합니다.**
>
> MediaPipe GPU 파이프라인 → 33개 랜드마크 추출 → 관절 각도 계산 → 500ms 판정 → 한국어 TTS 피드백
>
> 스쿼트 · 플랭크 · 런지 · 밸런스 **4종 자세 분석** · 얼굴 인식 **미니게임 3종** · Firebase 실시간 랭킹

<br>

**팀 프로젝트 4인 | 2023 ~ 2024 | 한국정보통신학회논문지(JKIICE) 게재**

<br>

</div>

---

## 📌 목차

1. [왜 만들었나](#-왜-만들었나)
2. [주요 기능](#-주요-기능)
3. [기술 스택](#️-기술-스택)
4. [시스템 아키텍처](#-시스템-아키텍처)
5. [핵심 구현](#-핵심-구현)
   - [각도 계산 알고리즘 (코사인 제2법칙)](#1-각도-계산-알고리즘-코사인-제2법칙)
   - [좌우 랜드마크 자동 선택](#2-좌우-랜드마크-자동-선택)
   - [MediaPipe 좌표 변환](#3-mediapipe-좌표-변환)
6. [운동별 관절 분석 기준](#-운동별-관절-분석-기준)
7. [미니게임](#-미니게임)
8. [트러블슈팅](#-트러블슈팅)
9. [팀 구성 & 역할](#-팀-구성--역할)
10. [자료 링크](#-자료-링크)

---

## 💡 왜 만들었나

헬스장에서 혼자 운동할 때 자세가 올바른지 실시간으로 확인할 방법이 없다. PT 트레이너는 비용이 크고, 운동 후 녹화 영상을 돌려보는 방식은 교정 타이밍을 놓친다. **카메라 하나로 운동하는 동안 즉각 피드백**을 주는 앱이 필요하다고 판단했다.

기존 AI 포즈 분류 접근법(딥러닝 분류 모델)은 매 프레임 추론 연산 때문에 실시간 반응이 어렵다. SHANNTI는 MediaPipe의 랜드마크 좌표를 받아 **수학 연산(코사인 제2법칙)만으로** 각도를 계산하기 때문에 추론 비용이 없고, 500ms 간격의 실시간 피드백이 가능하다. 이 접근법을 논문으로 정리해 한국정보통신학회논문지(JKIICE)에 게재했다.

---

## ✨ 주요 기능

<!--
  아래 img 태그의 src를 실제 스크린샷 경로로 교체해주세요.
  예: src="screenshots/home.png"
-->

<table>
<tr>
<td width="50%" valign="top">

### 🏋️ 실시간 운동 자세 분석

<!-- <img src="screenshots/squat_analysis.png" width="100%"> -->

- 스쿼트 · 플랭크 · 런지 · 밸런스 **4종** 실시간 분석
- MediaPipe GPU 파이프라인으로 **33개 신체 랜드마크** 추출
- 관절 각도 기반 **코사인 제2법칙** 판정 — 딥러닝 추론 없음
- 각 관절별 🟢 정상 / 🔴 비정상 / ⚫ 미감지 **3단계 시각화**

</td>
<td width="50%" valign="top">

### 🔊 한국어 TTS 실시간 피드백

<!-- <img src="screenshots/tts_feedback.png" width="100%"> -->

- 잘못된 자세 감지 즉시 한국어 음성 교정 안내
- 부위별 맞춤 피드백 ("무릎을 더 굽혀주세요", "허리를 펴주세요")
- 판정 주기 **500ms** — 체감 지연 없는 실시간 반응
- 1회 분석 세션 5,000ms 고정 윈도우

</td>
</tr>
<tr>
<td width="50%" valign="top">

### 🎮 얼굴 인식 미니게임 3종

<!-- <img src="screenshots/minigame.png" width="100%"> -->

- ML Kit 얼굴 좌표로 조종하는 **슈팅게임** (40초)
- 정확한 자세 카운팅 — 스쿼트 5회 · 런지 5회
- **밸런스 10초 유지** 챌린지
- 점수 Firebase RTDB 저장 → 실시간 글로벌 랭킹

</td>
<td width="50%" valign="top">

### 📊 통계 & 기록

<!-- <img src="screenshots/stats.png" width="100%"> -->

- 운동 시간 · 소모 칼로리 **일별 캘린더** (MPAndroidChart)
- 세션별 자세 교정 성공률 추적
- Room DB 로컬 저장 + Firebase RTDB 클라우드 동기화

</td>
</tr>
</table>

---

## 🛠️ 기술 스택

| 분류 | 기술 | 비고 |
|---|---|---|
| **언어** | Java · Kotlin · C++ | JNI 네이티브 연동 |
| **카메라** | CameraX API | 실시간 프레임 캡처 |
| **포즈 추적** | Google MediaPipe | `pose_tracking_gpu.binarypb` GPU 파이프라인 |
| **ML 추론** | TensorFlow Lite 2.7.0 | 자세 유사도 분류 보조 |
| **얼굴 감지** | Google ML Kit Face Detection | 미니게임 X좌표 기반 조작 |
| **이미지 처리** | OpenCV (Native JNI) | C++ 네이티브 연동 |
| **인증** | Firebase Authentication | 이메일 / 비밀번호 |
| **클라우드 DB** | Firebase Realtime Database | 점수 · 랭킹 실시간 동기화 |
| **로컬 DB** | Room Database | 운동 세션 기록 |
| **차트** | MPAndroidChart v3.1.0 | 운동 통계 시각화 |
| **음성** | Android TTS (TextToSpeech) | 한국어 실시간 피드백 |
| **Min / Target SDK** | 26 / 34 | |

---

## 🏗 시스템 아키텍처

```
┌─────────────────────────────────────────────────────────────┐
│                   📷 Camera Input (CameraX)                 │
│              실시간 비디오 프레임 → ImageProxy                  │
└───────────────────────────┬─────────────────────────────────┘
                            │ Video Frame (RGBA)
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         🤖 MediaPipe Pose Tracking (GPU Pipeline)           │
│              pose_tracking_gpu.binarypb                     │
│    → 33개 신체 랜드마크 추출 (Normalized X, Y, Z 좌표)           │
│    → GPU 가속으로 딥러닝 추론 비용 최소화                         │
└───────────────────────────┬─────────────────────────────────┘
                            │ NormalizedLandmarkList (33개)
                            ▼
┌─────────────────────────────────────────────────────────────┐
│              📐 Landmark Processing Layer                   │
│   1. 좌표 스케일링  ×1000  (0~1 → 0~1000)                     │
│   2. 유효 범위 검증  (-100 ≤ x,y ≤ 1100)                      │
│   3. 좌우 랜드마크 자동 선택  (side = 0 or 1)                   │
│      → OutOfRangeSave[] 배열로 유효한 방향 판별                 │
└───────────────────────────┬─────────────────────────────────┘
                            │ markPoint[] (유효 좌표)
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         🔢 Angle Calculation Engine (500ms 주기)             │
│                                                             │
│   getLandmarksAngleTwo(p1, p2, p3, axis1, axis2)            │
│   θ = acos( (a² + b² - c²) / 2ab ) × (180/π)                │
│                                                             │
│   → 딥러닝 없음 · 추론 지연 없음 · 순수 수학 연산                  │
└──────────┬────────────────┬────────────────┬────────────────┘
           │                │                │
           ▼                ▼                ▼
    🟢 CORRECT        🔴 INCORRECT      ⚫ NOT DETECTED
    정상 범위 내       범위 초과          랜드마크 미감지
                      → TTS 피드백
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│   💾 결과 저장  Room DB (로컬) · Firebase RTDB (클라우드)       │
│   📊 통계 시각화  MPAndroidChart · CalendarActivity           │
│   🏆 랭킹 반영  Firebase RTDB 실시간 리더보드                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 🔬 핵심 구현

### 1. 각도 계산 알고리즘 (코사인 제2법칙)

> 딥러닝 추론 비용 없이 **수학 연산만으로** 실시간 자세를 판정하는 것이 이 프로젝트의 핵심 기술적 선택입니다.
> 포즈 분류 모델을 쓰면 매 프레임 추론 연산이 발생하지만, 코사인 제2법칙은 연산 비용이 고정(O(1))이라 500ms 폴링도 거뜬합니다.

```java
/**
 * 세 랜드마크 좌표로 관절 각도를 계산합니다. (p2가 꼭짓점)
 *
 * @param p1, p2, p3  세 신체 관절점 (예: 엉덩이, 무릎, 발목)
 * @param a, b        사용할 평면 축 ('x', 'y', 'z' 중 2가지)
 * @return            각도 (0 ~ 360°)
 *
 * 수식: θ = acos( (|p1p2|² + |p2p3|² - |p1p3|²) / (2 × |p1p2| × |p2p3|) )
 */
public static float getLandmarksAngleTwo(
        markPoint p1, markPoint p2, markPoint p3, char a, char b) {

    // Step 1. 두 점 간 유클리드 거리 계산
    float p1_2 = dist(p1, p2, a, b);   // p1 ↔ p2
    float p2_3 = dist(p2, p3, a, b);   // p2 ↔ p3
    float p3_1 = dist(p3, p1, a, b);   // p3 ↔ p1 (대변)

    // Step 2. 코사인 제2법칙 → θ = acos( (a² + b² - c²) / 2ab )
    float radian = (float) Math.acos(
        (Math.pow(p1_2, 2) + Math.pow(p2_3, 2) - Math.pow(p3_1, 2))
        / (2 * p1_2 * p2_3)
    );

    // Step 3. 라디안 → 도(°) 변환
    return (float) (radian / Math.PI * 180);
}
```

**호출 예시 — 스쿼트 무릎 각도 판정:**

```java
// 랜드마크 인덱스: 엉덩이(23) → 무릎(25) → 발목(27)
float kneeAngle = getLandmarksAngleTwo(
    bodyMarkPoint[23 + side],   // 엉덩이 (좌/우 자동 선택)
    bodyMarkPoint[25 + side],   // 무릎   (꼭짓점)
    bodyMarkPoint[27 + side],   // 발목
    'x', 'y'
);

// 스쿼트 무릎 정상 범위: 30° ~ 90°
if (kneeAngle < 30f || kneeAngle > 90f) {
    speakFeedback("무릎을 더 굽혀주세요");   // TTS 교정 안내
    result[KNEE] = INCORRECT;
} else {
    result[KNEE] = CORRECT;
}
```

---

### 2. 좌우 랜드마크 자동 선택

> MediaPipe는 좌측/우측 랜드마크를 각각 따로 제공합니다. 사용자가 카메라를 향해 어느 방향으로 서 있든 **유효 범위 안에 있는 쪽**을 자동으로 선택합니다.

```java
// MediaPipe 랜드마크 인덱스:
//   어깨: 11(왼), 12(오른)  /  엉덩이: 23(왼), 24(오른)
//   무릎: 25(왼), 26(오른)  /  발목: 27(왼), 28(오른)
//
// OutOfRangeSave[i] = true  → 해당 랜드마크가 화면 유효 범위(-100 ~ 1100) 내에 있음
// side = 0 (왼쪽 기준) or side = 1 (오른쪽 기준)

for (int side = 0; side <= 1; side++) {
    boolean shoulderVisible = OutOfRangeSave[11 + side];
    boolean hipVisible      = OutOfRangeSave[23 + side];
    boolean kneeVisible     = OutOfRangeSave[25 + side];

    if (shoulderVisible && hipVisible && kneeVisible) {
        getLandmarksAngleResult(side);  // 유효한 방향으로 자세 판정
        break;
    }
}
// 어느 쪽도 유효하지 않으면 → NOT DETECTED 처리
```

---

### 3. MediaPipe 좌표 변환

> MediaPipe는 정규화된 좌표(0~1)를 반환하며, **X/Y 축이 화면 기준과 반전**되어 있습니다.
> 좌표 교환 + 1000 스케일링으로 이후 각도 계산의 수치 안정성을 확보합니다.

```java
// MediaPipe 반환값: NormalizedLandmark (getX=0~1, getY=0~1, getZ 깊이)
// 화면 좌표계와 XY 축이 교환되어 있어 명시적 변환이 필요

for (int i = 0; i < 33; i++) {
    // MediaPipe Y → 화면 X축  (좌우 방향)
    bodyMarkPoint[i].x = bodyAdvancePoint[i].getY() * 1000f;
    // MediaPipe X → 화면 Y축  (상하 방향)
    bodyMarkPoint[i].y = bodyAdvancePoint[i].getX() * 1000f;
    // Z축 (깊이) — 일부 운동에서 3D 각도 계산에 사용
    bodyMarkPoint[i].z = bodyAdvancePoint[i].getZ() * 1000f;
}

// 유효 범위 필터링: -100 ~ 1100 밖이면 해당 랜드마크 미감지로 처리
for (int i = 0; i < 33; i++) {
    OutOfRangeSave[i] = (bodyMarkPoint[i].x > -100 && bodyMarkPoint[i].x < 1100
                      && bodyMarkPoint[i].y > -100 && bodyMarkPoint[i].y < 1100);
}
```

---

## 📐 운동별 관절 분석 기준

> **설계 철학:** 핵심 관절(무릎·척추)은 **좁은 허용 범위**로 엄격하게, 보조 관절(팔)은 체형 다양성을 수용하도록 **넓은 범위**를 적용했습니다.
> 팔을 앞으로 뻗든 팔짱을 끼든, 무릎·척추 기준만 충족하면 정상으로 인정합니다.

### 🏋️ 스쿼트 (Squat)

| 부위 | 관절점 (인덱스) | 정상 범위 | 중요도 | 이유 |
|---|---|---|---|---|
| **무릎** | 엉덩이(23) → 무릎(25) → 발목(27) | **30° ~ 90°** | ⭐ 핵심 | 무릎 각도가 운동 효과 결정 |
| **척추** | 어깨(11) → 엉덩이(23) → 무릎(25) | **40° ~ 90°** | ⭐ 핵심 | 허리 부상 방지 |
| 팔 | 어깨(11) → 팔꿈치(13) → 손목(15) | 160° ~ 200° | 보조 | 팔 자세 자유도 허용 |

![Image](https://github.com/user-attachments/assets/4ee538f7-6a90-4b42-b538-b104be9f12ec)

### 🧘 플랭크 (Plank)

| 부위 | 관절점 | 정상 범위 | 중요도 |
|---|---|---|---|
| **팔꿈치** | 어깨 → 팔꿈치 → 손목 | **60° ~ 120°** | ⭐ 핵심 |
| **척추** | 귀(7) → 어깨(11) → 엉덩이(23) | **150° ~ 210°** | ⭐ 핵심 |

### 🚶 런지 (Lunge)

| 부위 | 정상 범위 | 중요도 |
|---|---|---|
| **앞다리 각도** | **40° ~ 100°** | ⭐ 핵심 |
| **척추** | **150° ~ 210°** | ⭐ 핵심 |

### ⚖️ 밸런스 (Balance)

- 한쪽 발 랜드마크(발목 → 발끝) 감지 기반 균형 유지 여부 판정
- 5초 고정 분석 세션 · 유효 랜드마크 지속 감지 여부로 판정

<img width="563" height="283" alt="Image" src="https://github.com/user-attachments/assets/21d531bc-2f25-4281-9893-6556eb1c114c" />
---

## 🎮 미니게임

### 얼굴 인식 슈팅게임 (40초)

```
ML Kit Face Detection
       │  얼굴 BoundingBox의 중심 X 좌표 추출
       ▼
  로켓 스프라이트 수평 이동 제어
       │  (화면 너비 정규화 → 로켓 X 좌표 매핑)
       ▼
  낙하하는 오브젝트 충돌 판정
       │
  ├── 🔴 빨간 공   충돌 → +1점
  ├── ❤️ 라이프     충돌 → +1 생명
  └── 💊 탄약       충돌 → +1 발사 횟수
       │
  40초 종료 → Firebase RTDB 점수 저장 → 실시간 랭킹 반영
```
<img width="1046" height="251" alt="Image" src="https://github.com/user-attachments/assets/611b60a2-d1a4-4be5-af85-71498348e4f8" />

### 자세 기반 카운팅 게임

| 게임 | 목표 | 판정 조건 |
|---|---|---|
| **GameSquat** | 정확한 스쿼트 **5회** | 무릎 30°~90° + 척추 40°~90° 동시 충족 |
| **GameLunge** | 정확한 런지 **5회** | 각도 범위 내 + 2초 디바운스 (연속 카운트 방지) |
| **GameBalance** | **10초** 균형 유지 | 단일 발 랜드마크 지속 감지 (10초 타이머) |

---

## 🔥 트러블슈팅

### ① 체형 다양성 — 단일 기준으로 자세 판정 불가

**문제**
스쿼트 시 팔을 앞으로 뻗거나 팔짱을 끼는 등 체형·운동 습관이 다양한데, 모든 관절에 동일한 허용 범위를 적용하면 정상 자세가 오류 판정되거나 잘못된 자세가 통과하는 문제 발생.

**해결**
관절별 중요도에 따라 허용 범위를 차등 적용. 무릎(30°~90°)과 척추(40°~90°)는 운동 효과와 부상 방지에 직결되므로 엄격하게 제한하고, 팔(160°~200°)은 허용 범위를 넓혀 체형 다양성을 수용.

**결과**
팔 자세가 달라도 무릎·척추 기준 충족 시 정상 인정. 서로 다른 운동 스타일을 허용하면서도 핵심 관절 기준은 유지.

---

### ② 딥러닝 추론 비용 — 실시간 피드백 지연

**문제**
포즈 분류에 딥러닝 모델을 사용할 경우 매 프레임마다 추론 연산이 발생해 실시간 피드백에 부적합한 지연 발생. GPU 가속을 써도 모델 로딩 비용과 추론 레이턴시가 존재.

**해결**
MediaPipe GPU 파이프라인으로 33개 랜드마크 좌표를 추출한 뒤, **코사인 제2법칙 수학 연산만으로** 각도를 계산. 별도 분류 모델 없이 순수 삼각함수 연산으로 판정.

```
θ = acos( (|p1p2|² + |p2p3|² - |p1p3|²) / (2 × |p1p2| × |p2p3|) )
```

판정 주기 500ms · 1회 분석 세션 5,000ms 고정 윈도우 적용.

**결과**
모델 로딩 지연 제거. 별도 추론 없이 수학 연산만으로 실시간 판정 구현. 이 접근법을 논문으로 정리해 JKIICE에 게재.

---

### ③ 좌우 비대칭 — 어느 쪽 기준으로 판정할지 모호

**문제**
MediaPipe는 좌우 랜드마크를 각각 제공(좌측 무릎 25번, 우측 무릎 26번). 사용자가 카메라를 향해 어느 방향으로 서 있느냐에 따라 어느 쪽을 기준으로 판정할지 불명확. 잘못된 쪽 선택 시 각도 계산 오류.

**해결**
좌측(`side=0`)과 우측(`side=1`) 양측을 순서대로 검사해, 어깨·엉덩이·무릎 랜드마크가 유효 범위(-100~1100) 내에 있는 방향을 자동 선택. `OutOfRangeSave[]` 배열로 각 랜드마크의 유효성을 사전 계산해 탐색 비용 최소화.

**결과**
카메라 방향과 무관하게 유효한 쪽 신체 기준으로 자동 판정. 사용자가 방향을 신경 쓸 필요 없음.

---

## 👥 팀 구성 & 역할

| 팀원 | 역할                                                                                |
|---|-----------------------------------------------------------------------------------|
| 👤본인 | **운동 자세 분석 로직 설계 및 구현**, 코사인 제2법칙 기반 각도 계산 알고리즘 개발, 좌우 자동 선택 · 좌표 변환 설계, 논문 제1 저자 |

---

## 📎 자료 링크

| 자료 | 링크 |
|---|---|
| 📄 보고서 PDF | [Google Drive](https://drive.google.com/file/d/1_e_FweSufS3RjxWfrffWqWe5y9QB4G0H/view?usp=sharing) |
| 📊 발표 PPT | [Google Slides](https://docs.google.com/presentation/d/1vncCXVw9mJxXfJ1YFctssHMkv47-yOG7/edit?usp=sharing) |

---

<div align="center">

**📄 한국정보통신학회논문지(JKIICE) 게재 | 팀 프로젝트 4인 | 2023 ~ 2024**

Made with 💪 by Team SHANNTI

</div>
