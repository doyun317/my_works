---
title: DeepSeek-OCR Contexts Optical Compression
status: read
authors:
  - HaoranWei, YaofengSun, YukunLi
year: 2025
rating: 4
projects:
  - RAG시스템 개발
summary: PDF를 이미지기반 잠재벡터로 바꾸고 이를 디코더로 텍스트를 다시 재구성
source_url: https://huggingface.co/deepseek-ai/DeepSeek-OCR
tags:
  - paper
  - text
  - image
  - OCR
  - RAG
  - chunking
  - CLIP
  - vision
draft: false
---
## 🧐 나의 생각 / 비판 (My Thoughts / Critiques)
- 텍스트 토큰이 너무 많으니까 LLM으로 처리하지 않고 이미지로 1차 처리 후 재 구성하는 방법
- 정확도만 높으면 사실 이걸 쓰는게 맞긴하다.
- 단순 LLM보다 VLM 이 더 효과적일 수 있는 분야
--- 
- **핵심제안** : 긴 텍스트 컨텍스트를 효율적으로 처리하기 위한 **'광학 압축(optical compression)'** 방법론 및 **DeepSeek-OCR 모델** 제안.
    
- **주요성과** : 10배 미만 압축률에서 **97%의 OCR 정밀도** 달성, 20배 압축률에서도 **약 60% 정확도** 유지.
    
- **의의** : LLM의 **긴 컨텍스트 처리 문제 해결** 및 메모리 망각 메커니즘 연구에 새로운 방향성 제시.
    

**1. 목표 (Goal)**

- LLM이 긴 텍스트 처리 시 직면하는 **시퀀스 길이에 따른 이차적 연산량 증가** 문제 해결 목표.
    
- 긴 텍스트 정보를 효율적인 **시각적 매체(optical 2D mapping)로 압축**하는 가능성 탐구.
    
- 시각-텍스트 압축 패러다임을 검증하기 위한 테스트베드로서 **OCR 작업 활용**.
    

**2. 데이터 (Data)**

- **OCR 1.0 (문서/장면):** 3,000만 페이지의 PDF (약 100개 언어), 300만 개의 Word 데이터, 2,000만 개의 자연 장면 OCR 데이터 (LAION, Wukong) 활용.
    
- **OCR 2.0 (복잡한 구조):** 차트(1,000만 개), 화학 공식(500만 개), 평면 기하학(100만 개) 데이터 생성 및 활용.
    
- **기타 데이터:** 일반 비전 데이터 (20%), 텍스트 전용 데이터 (10%)를 혼합하여 모델의 범용성 및 언어 능력 유지.
    

**3. 모델 구조 (Model Architecture)**

- **DeepSeek-OCR:** 인코더-디코더 VLM 아키텍처 사용.
    
- **인코더 (DeepEncoder):**
    
    - **SAM-base (80M):** 로컬 윈도우 어텐션(window attention) 기반 시각 인식 담당.
        
    - **16x Conv Compressor:** 비전 토큰을 16배 다운샘플링하여 압축.
        
    - **CLIP-large (300M):** 글로벌 어텐션(global attention) 기반 시각 지식 추출.
        
- **디코더 (Decoder):**
    
    - **DeepSeek-3B-MoE:** 3B 파라미터 MoE 모델 (추론 시 570M 파라미터 활성화).
        

**4. 주요 성과 (Key Achievements)**

- **높은 압축률 및 정밀도 (Fox 벤치마크):**
    
    - 10배 미만 압축 시: **OCR 정밀도 97%** 달성.
        
    - 20배 압축 시: **OCR 정밀도 약 60%** 유지.
        
- **실용적 OCR 성능 (OmniDocBench):**
    
    - **100 비전 토큰** 사용 (Small 모드)으로 GOT-OCR2.0 (256 토큰 사용) 성능 능가.
        
    - **800개 미만 비전 토큰** 사용 (Gundam 모드)으로 MinerU2.0 (약 7,000 토큰 사용) 성능 초과.
        
- **높은 데이터 생성 효율:** 단일 A100 GPU로 하루 **20만 페이지 이상의 학습 데이터 생성** 가능.
    

**5. 논문의 결론 (Conclusion)**

- '광학 압축'의 **실현 가능성**을 성공적으로 검증.
    
- DeepSeek-OCR이 **비전 토큰 수량의 10배를 초과하는 텍스트 토큰**을 효과적으로 디코딩할 수 있음을 입증.
    
- VLM 및 LLM의 발전, 특히 **대규모 사전 학습 데이터 생산**에 실질적 기여 가능.
    
- 향후 디지털-광학 텍스트 혼합 사전 훈련, 'Needle-in-a-haystack' 테스트 등 추가 검증 계획.