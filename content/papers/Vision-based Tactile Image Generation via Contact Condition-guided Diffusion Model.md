---
title: Vision-based Tactile Image Generation via Contact Condition-guided Diffusion Model
status: read
authors:
  - Xi Lin, Weiliang Xu, Yixian Mao, Jing Wang, Meixuan Lv, Lu Liu, Xihui Luo, Xinming Li
year: 2024
rating: 0
projects:
  - MVTC
summary: 비전 기반 촉각 이미지 생성
source_url: https://arxiv.org/abs/2412.01639
tags:
  - paper
  - diffusion
  - tactile
  - vision
draft: "false"
---
## 🧐 나의 생각 / 비판 (My Thoughts / Critiques)
- **연구 목표의 불일치**: 우리 연구는 시각, 언어(형용사), 시계열 데이터를 **하나의 벡터 공간에 표현하고 정렬**하는 것이 목표인 반면, 이 논문은 한 종류의 데이터(이미지+힘)를 다른 종류의 데이터(촉각 이미지)로 **변환/생성(Generation)**하는 것을 목표로 함.
    
- **기술의 불일치**: 우리 연구의 핵심 기술은 **조인트 임베딩**과 **자기지도학습(SSL)**이지만, 이 논문은 **조건부 확산 모델**을 지도학습 방식으로 활용.

- **데이터의 불일치**: 우리 연구에서 사용하는 **'촉각 형용사'**나 **'촉각 시계열 데이터'**는 이 논문에서 전혀 다루지 않음.
--- 
![](img/Pasted%20image%2020251017145331.png)
재생성된 촉각 이미지와 원본의 비교

- **목표 (Goal)**
    
    - 복잡한 물리/광학 모델링 없이, 실제 데이터 기반 접근법을 통해 **현실과 유사한(High-fidelity) 비전 기반 촉각 이미지를 생성**하는 것을 목표로 함.
        
    - 이를 통해 로봇 학습 등에서 발생하는 시뮬레이션과 현실 간의 차이(**Sim2Real Gap**)를 줄이고자 함.
        
- **데이터 (Data)**
    
    - **입력 (조건)**: **실제 객체의 RGB 이미지**와 접촉 시 측정된 **6축 힘(six-axial forces) 데이터**를 '접촉 조건(Contact Condition)'으로 사용함.
        
    - **출력 (타겟)**: 실제 비전 기반 촉각 센서로 촬영한 **RGB 촉각 이미지**.
        
    - 데이터 수집을 위해 정밀 이동 스테이지와 힘 게이지가 결합된 시스템을 구축하여 객체당 약 700쌍의 데이터를 수집함.
        
- **모델 구조 (Model Architecture)**
    
    - **조건부 확산 모델(Contact Condition-guided Diffusion Model)**을 핵심 모델로 사용함.
        
    - 가우시안 노이즈(Gaussian noise)에서부터 시작하여, 입력으로 주어진 '접촉 조건(객체 이미지 + 6축 힘)'의 안내를 받아 점진적으로 노이즈를 제거하며 목표 촉각 이미지를 생성하는 구조임.
        
    - 힘 데이터는 해시 함수를 통해 이미지 텐서와 결합 가능한 형태로 변환되어 모델에 입력됨.
        
- **주요 성과 (Key Achievements)**
    
    - **이미지 생성 품질**: 기존 물리 모델 기반 시뮬레이터(FOTS) 대비, 생성된 이미지의 **평균 제곱 오차(MSE)를 60.58% 감소**시킴.
        
    - **마커 변위 정확도**: 마커 기반 센서 시뮬레이션에서, 기존 연구 대비 **마커 변위 오차를 38.1% 감소**시켜 더 정확한 물리적 변형을 재현함.
        
    - **범용성 및 디테일**: 다양한 종류의 촉각 센서에 적용 가능하며, 몬테소리 보드 실험을 통해 물체의 **미세한 질감 특징까지 효과적으로 복원**하는 능력을 입증함.