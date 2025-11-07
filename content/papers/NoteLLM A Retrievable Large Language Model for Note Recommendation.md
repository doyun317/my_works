---
title: NoteLLM A Retrievable Large Language Model for Note Recommendation
status: read
authors:
  - Chao Zhang, Shiwei Wu, Haoxin Zhang, Tong Xu, Yan Gao, Yao Hu, Di Wu, Enhong Chen
year: 2024
rating: 3
projects:
  - AgenticAI
summary:
source_url: https://arxiv.org/abs/2403.01744
tags:
  - paper
  - agent
  - llm
  - contrastive_learning
  - multimodal
  - text
  - embedding
draft: false
---
## 🧐 나의 생각 / 비판 (My Thoughts / Critiques)
- 2024년에 나온 논문이라 그런지 지금은 당연한 내용인 내용 생성 및 추론에 "llm을 사용한다." 를 주장함
- 연관된 문서 찾는데 임베딩 방식을 활용한 대조학습 후 추천 모델
- 연관된 문서 찾을 때 검색 및 추천 방식을 활용
--- 
![](img/Pasted%20image%2020251107143210.png)
![](img/Pasted%20image%2020251107143214.png)
- **핵심 제안:** 대규모 언어 모델(LLM)을 활용한 새로운 **item-to-item (I2I) 노트 추천 프레임워크 'NoteLLM'** 제안.
    
- **주요 성과:** 온라인 A/B 테스트(Xiaohongshu)에서 기존 SentenceBERT 기반 모델 대비 **클릭률(CTR) 16.20% 향상** 달성.
    
- **의의:** 노트 추천(GCL)과 해시태그/카테고리 생성(CSFT)의 **멀티태스크 학습**을 통해 LLM의 임베딩 품질을 향상시킨 I2I 추천 연구.
    

---

### 1. 목표 (Goal)

- 기존 BERT 기반 추천 모델이 노트의 핵심 정보(해시태그, 카테고리 등)를 충분히 활용하지 못하는 한계 극복.
    
- **대규모 언어 모델(LLM)을 I2I(Item-to-Item) 노트 추천**에 효과적으로 도입하는 통합 프레임워크 **'NoteLLM'** 개발.
    
- 노트 추천 작업(임베딩 압축)과 해시태그/카테고리 생성 작업을 **멀티태스크(Multi-task)**로 동시에 학습시켜, 노트 임베딩의 품질 향상.
    

### 2. 데이터 (Data)

- **Xiaohongshu (샤오홍슈)**의 실제 유저 생성 노트 데이터셋 활용.
    
- **학습 데이터:** 노트 458,221개, 노트 페어 312,564개.
    
- **테스트 데이터:** 노트 257,937개, 노트 페어 27,999개.
    
- 1주간의 사용자 행동 데이터를 수집하여 **노트 간 동시 등장 점수(co-occurrence score)**를 계산, 이를 기반으로 '연관 노트 페어' 구축.
    

### 3. 모델 구조 (Model Architecture)

- **기반 모델:** 사전 학습된 LLM (**LLAMA 2, 7B**) 활용.
    
- **주요 구성 요소:**
    
    1. **Note Compression Prompt:** 노트 정보를 입력받아, 추천을 위한 단일 특수 토큰(**[EMB]**)으로 압축하고 동시에 해시태그/카테고리 생성을 지시하는 통합 프롬프트.
        
    2. **Generative-Contrastive Learning (GCL):** [EMB] 토큰의 숨겨진 상태(hidden state)를 노트 임베딩으로 사용. 사용자 행동 기반 '연관 노트 페어'를 Positive로, 배치 내 다른 노트를 Negative로 사용하여 **대조 학습(Contrastive Learning)** 수행 (I2I 추천 작업).
        
    3. **Collaborative Supervised Fine-tuning (CSFT):** LLM의 생성 능력을 활용하여 노트의 **해시태그와 카테고리를 생성**하도록 지도 학습(Supervised Fine-tuning) 수행 (생성 작업).
        
- **손실 함수:** GCL의 대조 학습 손실($L_{cl}$)과 CSFT의 생성 손실($L_{gen}$)을 하이퍼파라미터($\alpha$)로 결합하여 전체 손실 계산.
    

### 4. 주요 성과 (Key Achievements)

- **오프라인 평가:** SentenceBERT, RepLLAMA 등 기존 텍스트 기반 I2I 추천 모델 대비 **모든 Recall@k 지표에서 최고 성능(SOTA) 달성** (Avg. 94.66%).
    
- **온라인 A/B 테스트 (Xiaohongshu 배포):**
    
    - 기존 SentenceBERT 온라인 모델 대비 **클릭률(CTR) 16.20% 증가**.
        
    - 댓글 수 1.10% 증가, 주간 게시자 수(WAP) 0.41% 증가.
        
    - **신규 노트(콜드 스타트)에 대한 댓글 수 3.58% 증가**, LLM의 우수한 일반화 성능 입증.
        
- **어블레이션 스터디:**
    
    - CSFT(생성) 모듈 제거 시 추천 성능 하락, **생성 작업이 임베딩 품질 향상에 기여함**을 증명.
        
    - GCL(추천) 모듈 제거 시 추천 성능 크게 하락, 대조 학습의 중요성 확인.
        

### 5. 논문의 결론 (Conclusion)

- I2I 노트 추천을 위한 새로운 검색 가능 LLM 프레임워크 **'NoteLLM'** 제안.
    
- Note Compression Prompt, GCL, CSFT 세 가지 요소를 통해 LLM을 추천 시스템에 효과적으로 통합.
    
- GCL은 **협업 신호(collaborative signals)**를 학습하고, CSFT는 **노트 요약 및 생성**을 통해 임베딩을 강화하는 상호 보완적 역할 수행.
    
- 실제 대규모 서비스(Xiaohongshu) 환경에서의 실험을 통해 제안 모델의 **효과성과 실용성을 입증**.