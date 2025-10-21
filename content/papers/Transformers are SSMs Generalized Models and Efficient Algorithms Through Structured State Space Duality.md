---
title: Transformers are SSMs Generalized Models and Efficient Algorithms Through Structured State Space Duality
status: read
authors:
  - Tri Dao, Albert Gu
year: 2024
rating: 5
projects:
  - MVTC
summary: Mamba-2 모델
source_url: https://arxiv.org/abs/2405.21060
tags:
  - paper
  - timeseries
  - Mamba
  - llm
  - text
---
## 🧐 나의 생각 / 비판 (My Thoughts / Critiques)
- LTI 시스템에 대해 이해가 있어야 모델 파악이 쉽다.
- RNN 처럼 순차적으로 계산하는거랑, Transformer처럼 한번에 계산하는 방식이 수학적으로 동일하다.(LTI 때문에)
- 학습할땐 Transformer, 추론할땐 RNN <- 각각 빠르게 가능
--- 
![](img/Pasted%20image%2020251020171130.png)
![](img/Pasted%20image%2020251020171137.png)
### 요약

- **핵심 제안**: State-Space Model(SSM)과 어텐션이 **'상태 공간 이중성(SSD)'** 이라는 단일 프레임워크로 통합될 수 있음을 이론적으로 증명하고, 이 이중성을 활용해 SSM의 선형적 효율성과 어텐션의 병렬 처리 능력을 결합한 하드웨어 친화적인 'SSD 알고리즘'과 **Mamba-2 아키텍처**를 제안함.
    
- **주요 성과**: 언어 모델링에서 강력한 Transformer 베이스라인과 대등하거나 더 우수한 성능(파레토 최적)을 달성했으며, 핵심 연산은 기존 Mamba 대비 2~8배 더 빠름. 또한, 연관 리콜(MQAR) 과제에서 Mamba-1과 어텐션을 압도하며 향상된 상태 용량과 성능을 입증함.
    
- **의의**: SSM과 Transformer 사이의 개념적 간극을 허물어, 양쪽 진영의 최적화 기술을 상호 이전할 수 있는 이론적 토대를 마련함. 이를 통해 Transformer의 2차 복잡도 문제를 해결하면서도 확장성(e.g., 텐서 병렬화)을 확보하여, 긴 시퀀스 처리를 위한 차세대 파운데이션 모델의 강력한 백본(backbone) 아키텍처로서의 가능성을 열었음.
    

---

#### 1. 목표 (Goal)

이 연구의 주된 목표는 State-Space Models(SSM)과 Transformer(특히 어텐션 변형)라는 두 개의 독립적으로 발전해 온 모델 계열이 실제로는 깊이 연결되어 있음을 증명하는 **통합적인 이론적 프레임워크를 구축**하는 것입니다. 이 '상태 공간 이중성(State Space Duality, SSD)' 프레임워크를 통해, 최적화가 잘 된 Transformer의 설계 아이디어와 시스템 기술을 SSM에 접목하고, 반대로 SSM의 효율성을 Transformer 개념에 통합하고자 합니다. 최종적으로는 이 둘의 장점을 결합하여 기존 Transformer보다 효율적이면서도 강력한 차세대 시퀀스 모델(Mamba-2)을 개발하는 것을 목표로 합니다.

---

#### 2. 데이터 (Data)

Mamba-2의 성능을 검증하기 위해 다음과 같은 종류의 데이터를 사용했습니다.

- **합성 데이터 (Synthetic Data)**:
    
    - **다중 쿼리 연관 리콜 (Multi-Query Associative Recall, MQAR)**: 모델이 문맥 속에서 여러 키-값 쌍을 얼마나 잘 기억하고 조회하는지 테스트하기 위한 합성 데이터셋입니다. 이는 SSM과 같은 순환 모델에게 특히 어려운 과제로 알려져 있으며, Mamba-2의 향상된 상태(기억) 용량을 검증하는 데 사용되었습니다.
        
- **자연어 데이터 (Natural Language Data)**:
    
    - **The Pile**: 약 800GB 크기의 다양하고 방대한 영어 텍스트 데이터셋으로, 언어 모델의 사전 학습(pre-training) 및 스케일링 법칙(scaling law)을 분석하는 데 사용되었습니다.
        
    - **LM Evaluation Harness 벤치마크**: 사전 학습된 모델의 제로샷(zero-shot) 성능을 평가하기 위한 표준 벤치마크 모음입니다. LAMBADA, HellaSwag, PIQA, ARC, WinoGrande 등 널리 사용되는 여러 태스크를 포함합니다.
        

---

#### 3. 모델 구조 (Model Architecture)

이 논문은 이론적 프레임워크를 바탕으로 한 새로운 모델 아키텍처 **Mamba-2**를 제안합니다. Mamba-2는 기존 Mamba의 구조를 계승하면서 다음과 같은 핵심적인 개선 사항을 포함합니다.

- **핵심 레이어: SSD (State Space Duality) Layer**:
    
    - Mamba-1의 선택적 SSM(S6) 레이어를 대체하는 새로운 핵심 레이어입니다.
        
    - 이론적 이중성에 기반한 새로운 **SSD 알고리즘**을 사용하여 계산됩니다. 이 알고리즘은 시퀀스를 작은 '청크(chunk)'로 나누어, 청크 내부는 GPU에 최적화된 병렬적인 행렬 연산(어텐션 방식)으로, 청크 간 정보 전달은 효율적인 순차 연산(SSM 방식)으로 처리하여 속도와 하드웨어 효율을 극대화합니다.
        
- **블록 구조 개선**:
    
    - **병렬 파라미터 투영 (Parallel Projections)**: 기존 Mamba와 달리, Transformer가 Q, K, V를 한 번에 생성하듯 SSM의 핵심 파라미터(A, B, C)와 입력(X)을 병렬적으로 생성합니다 . 이는 대규모 모델 학습에 필수적인 텐서 병렬화(Tensor Parallelism) 효율을 크게 향상시킵니다.
        
    - **추가 정규화 (Extra Normalization)**: 블록의 마지막 부분에 정규화 레이어를 추가하여 대규모 모델에서의 학습 안정성을 높였습니다.
        
- **헤드 구조 (Head Structure)**:
    
    - 여러 개의 입력 헤드(X)가 소수의 파라미터 헤드(B, C)를 공유하는 **다중 입력 SSM (Multi-Input SSM, MIS)** 구조를 채택했습니다. 이는 어텐션의 다중 값 어텐션(Multi-Value Attention, MVA)과 유사한 개념으로, 실험 결과 다른 헤드 구조보다 우수한 성능을 보였습니다.
        

---

#### 4. 주요 성과 (Key Achievements)

- **성과 1: SSM과 어텐션의 이론적 통합**: '상태 공간 이중성(SSD)'이라는 프레임워크를 통해, SSM이 '준분리 행렬(Semiseparable Matrix)'이라는 특정 구조의 행렬 변환임을 증명하고, 이를 통해 어텐션의 한 형태와 수학적으로 동일함을 밝혔습니다. 이는 두 모델 계열 사이의 개념적 간극을 메운 중요한 이론적 성과입니다.
    
- **성과 2: 하드웨어 효율적인 고성능 알고리즘 개발**: 이론적 이중성에 기반한 새로운 **SSD 알고리즘**을 개발했습니다. 이 알고리즘은 기존 Mamba의 핵심 연산(scan)보다 **2~8배 빠르며**, 시퀀스 길이가 2K 이상일 경우 고도로 최적화된 FlashAttention-2보다도 빠른 처리 속도를 보입니다.
    
- **성과 3: Mamba-2 아키텍처의 우수성 입증**:
    
    - Mamba-2는 어려운 합성 과제(MQAR)에서 기존 Mamba-1 및 순수 어텐션 모델을 능가하는 성능을 보여, 향상된 상태(기억) 용량과 처리 능력을 증명했습니다.
        
    - 언어 모델링 성능 평가에서 Mamba-2는 동일한 연산량 대비 Mamba 및 강력한 Transformer++ 베이스라인보다 우수한 성능을 보여 **파레토 최적(Pareto-dominant)** 관계에 있음을 입증했습니다