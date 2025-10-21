---
title: A Touch, Vision, and Language Dataset for Multimodal Alignment
status: read
authors:
  - Letian Fu, Gaurav Datta, Huang Huang, William Chung-Ho Panitch, Jaimyn Drake, Joseph Ortiz, Mustafa Mukadam, Mike Lambeta, Roberto Calandra, Ken Goldberg
year: 2024
rating: 5
projects:
  - MVTC
summary: Vison, Tactile, Language Model
source_url: https://arxiv.org/abs/2402.13232
tags:
  - paper
  - llm
  - multimodal
  - tactile
  - vision
  - CLIP
  - LoRA
  - Encoder
  - Decoder
draft: "false"
---
## 🧐 나의 생각 / 비판 (My Thoughts / Critiques)

+ 촉각-시각-언어 임베딩과 그에 따른 대표적인 하위 스트림을 붙인 논문
+ 아키텍쳐 구조를 따라하기 아주 모범적임
--- 

![](img/Pasted%20image%2020251017144704.png)
																				간략한 모델 구조
![](img/Pasted%20image%2020251017144800.png)
																				자세한 모델 구조

#### **1. 목표 (Goal)**

- 인간의 핵심 감각인 **촉각(Touch)을 시각(Vision), 언어(Language)와 통합**하여, 다중모드(multimodal) 언어 모델이 촉감을 이해하고 설명할 수 있도록 하는 것을 목표로 함.
    
- 촉각 데이터와 이를 설명하는 언어 라벨의 부족 문제를 해결하기 위해, 대규모 데이터셋을 구축하고 이를 기반으로 한 모델을 개발함.
    
- **입력**으로 **촉각 이미지 데이터**와, **시각 데이터**가 들어가 모델은 해당 입력값에 적절한 **촉각 형용사**를 **출력**해줌.

---

#### **2. 데이터 (Data)**

- **데이터셋명:** **TVL (Touch-Vision-Language) Dataset**.
    
- **규모:** 총 44,000개의 촉각-시각 데이터 쌍으로 구성됨.
    
- **구성 요소:**
    
    - **Tactile:** DIGIT 센서를 통해 얻은 **RGB 이미지** 형태의 촉각 정보.
        
    - **Vision:** 웹캠으로 촬영한 객체의 시각 이미지.
        
    - **Language:** 촉감을 묘사하는 자연어 형용사.
        
- **라벨링 방식:**
    
    - **10% (약 4.6K):** 사람이 직접 촉감 묘사 라벨을 작성.
        
    - **90% (약 39K):** GPT-4V를 이용해 시각 이미지로부터 촉감 묘사 라벨을 자동 생성 (유사 라벨링, Pseudo-Labeling).
        

---

#### **3. 모델 구조 (Model Architecture)**

- **기반 모델:** **LLaMA 2** 대규모 언어 모델을 기반으로 함.
    
- **인코더 (Encoders):**
    
    - 촉각 인코더: **Vision Transformer (ViT)**를 사용하여 촉각 이미지를 벡터로 변환.
        
    - 시각/언어 인코더: **OpenCLIP**의 사전 훈련된 인코더를 활용.
        
- **학습 방식 (정렬 기법):**
    
    - 세 가지 모달리티(촉각, 시각, 언어)를 하나의 **의미 공간에 정렬**하기 위해 **쌍별 대조 학습 (Pairwise Contrastive Learning)**을 사용함.
        
    - `촉각-시각`, `촉각-언어`, `시각-언어` 등 모든 쌍에 대해 직접적인 관계를 학습시켜, 특히 촉각과 언어의 의미적 연결을 강화함.
        

---

#### **4. 주요 성과 (Key Achievements)**

- 세계 최초로 촉각, 시각, 개방형 어휘 언어를 통합한 대규모 데이터셋 **TVL을 구축하고 공개함**.
    
- 소량의 인간 라벨과 대량의 AI 생성 라벨을 혼합하여 학습한 모델(**TVL-LLaMA**)이, 라벨 생성에 사용된 원본 AI(**GPT-4V**)보다 **12% 더 뛰어난 성능**을 보임을 입증함.
    
- 촉각 정보를 통합한 TVL-LLaMA 모델이 새로운 TVL 벤치마크에서 기존의 다른 시각-언어 모델들보다
    
    **최소 12% 이상 높은 성능**을 달성함.
    
- 촉각-언어 간의 직접적인 대조 학습이 모델 성능에 결정적이며, 이를 통해 촉각-언어 분류 정확도를
    
    **29% 향상**시킴.
    

---

#### **5. 향후 방향**

- 이 논문의 가장 큰 특징이자 한계는 촉각 데이터를 **정적 이미지(static image)**로 다루었다는 점.
    
- 우리의 연구는 **시계열(time-series)** 데이터를 다루므로, 여기서 독창성을 주장 할 수 있음.
    
- **논문/특허 전략:** "선행 연구(이 논문)는 정적 이미지를 통해 촉각-시각-언어 임베딩의 가능성을 열었지만, 시간적 연속성과 동적 정보를 포착하지 못했다. 본 연구는 여기서 더 나아가 **시계열 촉각 데이터**를 처리할 수 있는 새로운 인코더를 도입하고, 이를 통해 동적 질감, 압력 변화 등 더 풍부한 촉각 정보를 언어 및 시각과 성공적으로 정렬했다." 와 같이 연구의 차별점을 명확히 할 수 있음.
