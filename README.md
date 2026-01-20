# 🏭공장 표면 결함 검사 모니터링🔍

<div align="center">
<!-- logo -->
<img src="images/스마트팩토리1.jpg" alt="설명" style="width: 400px; height: auto;">
<img src="images/스마트팩토리2.jpg" alt="설명" style="width: 400px; height: auto;">
</div> 

---

## 📜 프로젝트 개요
스마트 팩토리 환경에서 육안 검사 의존으로 인한 품질 편차, 그리고 결함 데이터 수집의 어려움이라는 문제에서 출발했습니다.  

기존의 수동 검사 및 단순 촬영 방식은  
* 검사자의 숙련도에 따른 결과 편차  
* 환경 변화(조명, 표면 상태)에 따른 재촬영 필요  
* 현장 데이터의 체계적 축적 불가

라는 한계를 가지고 있었습니다.

이를 해결하기 위해 Jetson 기반 엣지 컴퓨팅 환경에서 CSI 카메라 입력과 GPIO 하드웨어 제어를 결합한 접근 방식을 적용하였고,
실시간 결함 이미지 수집과 웹 서버를 통해 사용자가 언제든지 조회·모니터링할 수 있는 구조를 적용하며,  
현장 캡처 안정성 + 원격 접근성을 동시에 만족하는 공장 표면 결함 검사 모니터링 시스템을 구현했습니다.  

<br />

---

## 🏗️ 프로젝트 아키텍처
<br />
<div align="center">
<img src="images/과정1.png" alt="설명" style="width: 700px; height: auto;">
</div> 
<br />

* **Edge Vision Node (Jetson)**

  * 역할: 생산 라인 현장 영상 수집 및 제어
  * 핵심 기능:

    * CSI 카메라 실시간 프레임 수집
    * GPIO 스위치 기반 촬영/종료 제어
    * 결함 이미지 로컬 저장
  * 입력 / 출력:

    * 입력: 카메라 영상, 하드웨어 스위치
    * 출력: 이미지 파일 및 메타데이터

* **Camera Pipeline (GStreamer + OpenCV)**

  * 역할: 고해상도 영상 입력 파이프라인 구성
  * 특징:

    * `nvarguscamerasrc` 기반 CSI 카메라 처리
    * NVMM 메모리 활용으로 CPU 부하 최소화
    * OpenCV와 직접 연동 가능한 BGR 포맷 변환

* **Web UI (User Monitoring Interface)**

  * 역할: 사용자 모니터링 화면
  * 특징:

    * 브라우저 기반 접근
    * 결함 이미지 리스트 및 상세 조회
    * 현장 상태를 원격에서 확인 가능

---

### 🔗 데이터 / 통신 흐름

* CSI Camera → GStreamer → OpenCV Frame
* GPIO Switch → 캡처 이벤트 발생
* Frame → Image 저장
* Image → Web Server 업로드
* Web Server → Web UI → 사용자 조회

---

## 🛠 3. 기술 스택

### 🔹 Core Technologies

* Language: Python
* Framework / Library: OpenCV, GStreamer
* Platform / Environment: NVIDIA Jetson (Linux)

### 🔹 Tools & Infrastructure

* Hardware Interface: Jetson.GPIO
* Web Server: HTTP 기반 웹 서버 (Node.js)
* Communication: Image Upload / REST API
* OS / Runtime: Ubuntu (Jetson Linux)

---

## ⭐ 4. 주요 기능 상세

### 1) Jetson CSI 카메라 실시간 스트리밍

* GStreamer 파이프라인을 직접 구성하여 CSI 카메라 입력 처리
* 해상도 / 프레임레이트 / Flip 설정을 함수화하여 확장성 확보
* OpenCV `VideoCapture(CAP_GSTREAMER)` 연동

---

### 2) 2) GPIO 스위치 기반 캡처 & 시스템 제어

* BCM 18번 핀 입력 감지

* **짧은 입력**
  * 최초 HIGH 감지 시 단 1회 이미지 캡처
* **긴 입력**
  * 일정 시간 이상 유지 시 `shutdown -h now` 실행

---

### 3) 웹 서버 기반 모니터링 기능

* 캡처된 결함 이미지를 웹 서버로 전송
* 사용자는 웹 브라우저를 통해:

  * 결함 이미지 조회
  * 촬영 시점 기준 이력 확인
  * 현장 상태 원격 모니터링
---

## 🛠️ 프로젝트 진행과정


<img src="images/그림01.jpg" alt="설명" style="width: 800px; height: auto;">

<br />
<img src="images/그림02.jpg" alt="설명" style="width: 800px; height: auto;">

<br />
<img src="images/그림03.jpg" alt="설명" style="width: 800px; height: auto;">

<br />
<img src="images/그림04.png" alt="설명" style="width: 800px; height: auto;">


<br />

---

## 📝 ERD 설계
<img src="images/Untitled (1).png" alt="설명" style="width: 800px; height: auto;">
<br />
