---
title: Sparsh Self-supervised touch representations for vision-based tactile sensing
status: read
authors:
  - Carolina Higuera, Akash Sharma, Chaithanya Krishna Bodduluri, Taosha Fan, Patrick Lancaster, Mrinal Kalakrishnan, Michael Kaess, Byron Boots, Mike Lambeta, Tingfan Wu, Mustafa Mukadam
year: 2024
rating: 2
projects:
  - MVTC
summary: 이미지 복원에서 잠재 공간 활용한 학습이 훨씬 우수함
source_url: https://arxiv.org/abs/2410.24090
tags:
  - paper
  - Vit
  - vision
  - JEPA
  - DINO
  - MaskedAE
  - SSL
  - tactile
---
## 🧐 나의 생각 / 비판 (My Thoughts / Critiques)

- **SSL 모델**
    
    - **Sparsh**: MAE, DINO, JEPA 등 '자율 지도 학습(SSL)'이 핵심 기술.
        
    - **활용 전략**: 이미지 관련 모델인 Masked AE를 제외하고 DINO, JEPA 등 추상적인 특징공간에서 작업하는 모델은 차용하여 실험할 가치가 있음. 기술적 기반이 동일하므로, Sparsh의 **평가 프레임워크(TacBench 개념, Frozen Encoder 방식)를 차용**하여 연구 결과의 신뢰도를 확보.
        
- **데이터의 차별성**
    
    - **Sparsh**: 촉각 이미지.
        
    - **진행할 연구**: 시각, 촉각 형용사, 촉각 시계열.
        
    - **활용 전략**: 새로운 데이터 조합을 다루는 것이 우리 연구의 핵심 기여임을 명확히 함.
--- 
![](img/Pasted%20image%2020251017145703.png)
각 벤치마크 실험 결과
![](img/Pasted%20image%2020251017145714.png)
모델 구조도
#### **목표 (Goal)**

- 특정 작업(task)과 센서에 종속되지 않는 **범용(general-purpose) 촉각 표현(representation) 개발**.
    
- 레이블링된 데이터 수집의 어려움을 극복하기 위해, **자율 지도 학습(Self-Supervised Learning, SSL)을 활용**하여 대규모의 레이블 없는 데이터로부터 학습.
    
- 학습된 표현의 성능을 표준화된 방식으로 평가할 수 있는 **벤치마크(TacBench) 구축**.
    

#### **데이터 (Data)**

- **SSL 사전 훈련 데이터**:
    
    - DIGIT, GelSight, GelSight Mini 등 3가지 종류의 센서에서 수집된 **46만 개 이상의 레이블 없는 촉각 이미지** 사용.
        
    - YCB-Slide, Touch-and-Go, ObjectFolder 등 기존 데이터셋과 자체 제작한 Touch-Slide 데이터셋을 통합하여 대규모 데이터 구축.
        
- **평가 데이터 (TacBench)**:
    
    - 힘 추정, 미끄러짐 감지, 자세 추정 등 **6개의 다운스트림 과제(downstream tasks)를 위한 별도의 레이블링된 데이터셋**을 구축하여 사용.
        
    - 이 데이터는 SSL 사전 훈련에는 사용되지 않은, 새로운 센서와 객체로 구성하여 모델의 일반화 성능을 평가.
        

#### **모델 구조 (Model Architecture)**

- **기본 인코더**: 모든 SSL 모델의 백본(backbone)으로 **Vision Transformer (ViT) 아키텍처**를 사용.
    
- **Sparsh 모델 제품군**: 세 가지 계열의 최신 SSL 방법론을 실험하고 비교.
    
    - **Sparsh (MAE)**: 이미지의 일부를 가리고(masking), 이를 픽셀 단위로 복원하도록 학습하는 생성 모델.
        
    - **Sparsh (DINO)**: 학생-교사(student-teacher) 구조를 통해, 학생 모델이 교사 모델의 잠재 공간(latent space) 표현을 모방하도록 학습하는 자기 증류(self-distillation) 모델.
        
    - **Sparsh (IJEPA/V-JEPA)**: 이미지의 일부(context)를 보고 가려진 부분의 잠재 공간 표현을 예측하도록 학습하는 조인트 임베딩 예측(joint-embedding predictive) 모델.
        
- **입력 처리**: 시간적 정보를 포착하기 위해, 시간 간격을 둔 **두 개의 촉각 이미지를 채널 차원으로 결합**하여 모델의 입력으로 사용 (It​⊕It−5​).
    

#### **주요 성과 (Key Achievements)**

- **높은 성능 달성**: 제안하는 Sparsh 모델이 기존의 엔드-투-엔드(E2E) 방식보다 **평균 95.1% 더 높은 성능**을 보임 (특히 레이블링된 데이터가 적을 때 격차가 큼).
    
- **표준 벤치마크(TacBench) 개발**: 촉각 표현 연구의 발전을 촉진할 수 있는 **표준화된 평가 프레임워크를 최초로 제안**.
    
- **잠재 공간 학습의 효과 입증**: 픽셀 단위 복원(MAE)보다 **잠재 공간에서 학습(DINO, IJEPA)하는 것이 촉각 표현 학습에 더 우수함**을 실험적으로 증명.
    
- **교차 센서 일반화 능력**: 학습된 표현이 **새로운 종류의 센서에도 단 몇 개의 데이터(few-shot)만으로 빠르게 적응**함을 보여줌.
    

