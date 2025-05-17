# 딥러닝 기반 신경세포 칼슘 이미징 데이터 분석
<img alt="Static Badge" src="https://img.shields.io/badge/python-%233776AB?style=for-the-badge&logo=python&logoColor=white"> <img alt="Static Badge" src="https://img.shields.io/badge/pytorch-%23EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white"> <img alt="Static Badge" src="https://img.shields.io/badge/anaconda-%2344A833?style=for-the-badge&logo=anaconda&logoColor=white"> <img alt="Static Badge" src="https://img.shields.io/badge/nvidia-%2376B900?style=for-the-badge&logo=nvidia&logoColor=white">


- 진행기간: 2021.06 ~ 2022.02
- 진행인원: 4명
- 담당역할: 데이터 정제/관리/라벨링, 인공지능 모델링/학습/검증/평가, 모델 해석
- [Representation (japanese)](https://github.com/taeddy/Neuron_CNN_Interpretation/blob/main/Representation%20(japanese).pdf) ➡️ 2022/02/21 in Kobe Univ. Takiguchi LAB
- [Main Code](https://github.com/taeddy/Neuron_CNN_Interpretation/blob/main/Neuron_CNN_Interpretation.ipynb)
- <details>
  <summary>인포그래픽 (한국어)</summary>
  <div markdown="1">
  <img src="https://github.com/user-attachments/assets/cdd25ad7-570d-4ff6-a6d3-0eb3988be6bc">
  </div>
  </details>

---

## 📌 배경

- **환상통 (Allodynia)**: 정상적인 자극에도 통증을 느끼는 신경계 질환.
- 메커니즘이 불명확하여 약물 개발과 치료가 어려움.
- 신경세포 활동의 변화를 이해하기 위한 다양한 연구가 진행 중.

## ❗ 기존 분석 방법의 문제점

- 신경세포의 위치와 개수를 사람이 이미지를 보고 판단하여 지정해 주어야 함.
- 세포 간 관계를 파악하기 위해 통계적 기법을 선택해야 하고, 기법에 따라 결과가 달라짐.
- 뉴런의 활성 기준치를 정하는 것이 모호함.
- 상기 이유로 인해 자동화가 어려움.

## 🎯 연구 목적

- **딥러닝(CNN)을 활용해 신경세포의 기능적 결합 분석**.
- 기존 방법과의 차별점: 전처리 없이 이미지를 그대로 사용. -> 과정 자동화
- 미세한 활성도 차이까지 인식 가능한 모델 구축.

## 🔬 연구 방법

- **데이터**
  - 실험쥐의 뉴런을 칼슘 이미징 기법으로 관측
  - 2개 시점
    - 평시 뉴런 이미지
    - 특정 뉴런 자극 직후 2.5초의 이미지
  - CFA을 투여한 후의 데이터
    - CFA: 염증유발제 (염증으로 인해 뉴런간 결합이 강화될 것으로 사료됨)
    - 약물 투여에 따른 활동 패턴을 비교하기 위함
- **모델**
  - CNN을 이용하여 학습 → Grad-CAM으로 CNN이 이미지를 판별할 때의 기준을 해석
- **자극 세포 및 방법**
  - 총 24개 세포
  - 방사선을 이용한 광 자극

## ⚙️ 모델 구성

- **입력**: 칼슘 이미지
- **출력**: 자극된 세포의 라벨
- **모델 내부 구조**
  <img width="1377" alt="image" src="https://github.com/user-attachments/assets/1d1c5606-7bab-468c-bbda-bb84bfa817b0" />
- **Grad-CAM**: 학습된 CNN을 통해 이미지 내 중요 부분 시각화 (추가 모델 학습 불필요)

## 📈 실험 결과

- **모델 정확도**
  - CFA 투여 이전: 92%
  - CFA 투여 3일 후: 85%
    - 정확도 감소 이유: **세포 간 결합 강화 시사**
- **Grad-CAM 분석**
  - 각 세포의 CAM 결과를 취합하여 비교함으로써 자극된 세포 간 활동 패턴 유사도 비교가 가능함.
  - CFA 투여 후 세포 간 기능적 결합 강도 증가가 확인됨.
    - CAM 결과 간 상관치가 208.8 에서 262.1 로 증가하였음.

## ✅ 결론

- 간단한 CNN으로 신경세포 활동 분석 가능.
- Grad-CAM을 통해 통증 유도 전후의 세포 결합 변화를 시각적으로 파악 가능.
- 통증 유도제(CFA)의 효과를 이미지 기반으로 검증 가능.

## 🔧 향후 과제

- 칼슘 이미징 데이터의 처리 방식 개선
  - 향후 시계열 데이터라는 특징을 활용하면 월등히 향상된 결과를 확보할 수 있을 것으로 예상됨.
  - 본 연구에서는 특정 환경에서만 데이터를 확보할수 있고, 그에 따라 시간적 한계로 인하여 데이터의 양이 충분하지 못하였음. 따라서 특정 뉴런을 자극하면 2.5초 이내에 강한 연쇄반응이 지속된다는 점에만 착안하였고, 시계열임을 무시하여 단일 이미지로 분석함.
- 정교한 식별을 위한 모델 구조 구체화
  - 모델의 정확도를 향상시키기 위해서는 모델을 구체화하여 파라미터를 늘려야 함.
  - 하지만 이에 따라 풍부한 데이터가 필요하여 아래의 과제가 선행되어야 함.
- 다양한 데이터의 확보
  - 같은 부위의 신경세포를 장기간 관측하며 광 자극을 가하는 것은 물리적인 한계가 있음.
  - 따라서 뉴런 배치와 개수에 상관없이 모델 성능이 일반화 되도록 다양한 데이터가 필요함.
    - 데이터 확장 (Data Augmentation)으로 이를 일부 구현 가능함. (이미 연구에 적용되어 있음)

---

## 🧠 부록

### 1. 칼슘 이미징이란?

- 살아있는 동물에서 신경세포 활동을 시각화하는 기술.
- 칼슘 이온 농도 변화 → GFP 단백질 결합 시 형광 발광.

### 2. 데이터 구조

- 각 세포에 대해 총 10회 광 자극.
- 1회당 1개 또는 2개 세포 자극.
- 각 프레임을 개별 데이터로 처리.

### 3. 모델 학습 방식

- 자극 세포 번호를 one-hot 라벨로 사용.
- 손실 함수: Binary Cross Entropy Loss.
- 데이터 확장 및 마스킹 처리 수행.

### 4. 이미지 유사도 측정

- CAM 결과물을 비교하기 위한 장치
- **ZNCC (Zero-mean Normalized Cross-Correlation)** 사용.
- 이미지 간 유사도를 -1~1로 정량화.

