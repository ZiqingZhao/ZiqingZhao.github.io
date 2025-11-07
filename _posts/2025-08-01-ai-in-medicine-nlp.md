---
layout: post
title: "AI in Medicine: NLP"
date: 2025-08-01
excerpt: ""
tags: [AI, Medicine, NLP]
comments: true
---

#### Medical Text Data
- **Sources**:  
  - Medical reports (radiology, pathology), discharge summaries, clinical notes, transcriptions 
  - Publications (papers, guidelines), patient‐generated text (social media, interviews)  
- **Public Datasets**:  
  - MIMIC-III/IV (clinical notes, QA pairs, MedNLI)  
  - MIMIC-CXR (200 k chest X-ray reports + images)  
  - Open-I (4 000 radiology reports + images)  
  - PubMed RCT abstracts (200 k) 

#### Medical NLP Applications
- **Classification/Regression**: phenotype, pathology, hallmark detection  
- **Retrieval**: text–text, text–image search  
- **Information Extraction**: NER (disease, drug), PICO detection  
- **Reasoning**: QA, NLI, chatbots  
- **Text Generation**: report drafting, summarization 

#### Text Representation
1. **Formats**: BoW (counts), token sequences, parse trees 
2. **Tokenization**:  
   - Word‐level (OOV issues), character‐level, sub‐word (BPE, WordPiece)  
   - Pipeline: normalization → pre‐tokenization → vocabulary lookup → post‐processing 
1. **Embeddings**:  
   - **Word2Vec** (CBOW, Skip-gram)
   - **GloVe**: co-occurrence factorization 
   - **End-to-end** learned embeddings in deep models 

#### Transformer Architecture
- **Attention**: scaled dot-product on queries/keys/values   
- **Multi-Head**: parallel heads, separate projections, concat + linear   
- **Self vs Cross**: self‐attention within sequence; cross‐attention between encoder/decoder   
- **Components**:  
  - Input: token + positional embeddings  
  - Encoder: stacked self-attention + feed-forward + layer norms  
  - Decoder: masked self-attention + cross-attention + feed-forward   
- **Position Encodings**: sinusoidal or learned vectors   
- **Masks**: causal masks for decoder; full for encoder   
- **Variants**: encoder-only (BERT), decoder-only (GPT), encoder-decoder (T5) 

#### 5. Pre-Training Strategies
- **GPT (autoregressive)**: predict next token, decoder-only, causal mask   
- **BERT (autoencoding)**: MLM (15% masks), NSP, encoder-only bidirectional   
- **T5 (text2text)**: unified encoder-decoder, text2text loss for all tasks 

#### Domain Adaptation in Medical NLP
- **Continual Pre-training**: further train general model on medical corpora (BioBERT, ClinicalBERT)   
- **In-Domain from Scratch**: train on medical texts with dedicated vocabulary (PubMedBERT)   
- **Mixed-Domain**: joint sampling from general + medical corpora, amplified medical content 

#### 7Fine-Tuning Pre-trained Models
- **Classification/Regression**: \[CLS\] token → linear head, cross-entropy/MSE   
- **Sentence Similarity/Retrieval**: \[CLS\] A \[SEP\] B \[SEP\] → similarity head   
- **QA/NLI**: \[CLS\] question \[SEP\] context \[SEP\] → classification head   
- **NER/PICO**: token-level IOB/IOBES tagging, softmax over tags   
- **Relation Extraction**: entity pair classification via linear head 

#### Large Language Models (LLMs)
- **Scaling**: 10–100 B parameters, 100s B tokens   
- **Instruction Tuning & RLHF**: handcrafted instruction datasets, human feedback alignment   
- **Emergent Zero/Few-Shot**: perform unseen tasks via prompts, learn reasoning patterns 

#### LLM Risks
- **Factuality**: hallucinations, unverified data   
- **Bias**: dataset imbalances  
- **Compute**: environmental impact, accessibility  
- **Evaluation**: non-reproducible APIs, data leakage 

####  Prompt Engineering
- **Elements**: instruction, context, input, output indicator, examples   
- **Zero/Few-Shot**: include examples in prompt, Q&A format   
- **Chain-of-Thought**: “Let’s think step by step” to elicit reasoning   
- **Challenges**: manual tuning, format sensitivity, multimodal inputs 

#### Parameter-Efficient Fine-Tuning
- **Prompt Tuning**: train soft prompts, freeze model   
- **Prefix Tuning**: add trainable prefixes per layer   
- **Adapters**: insert small MLPs (bottleneck) in each block   
- **LoRA**: low-rank updates to projection matrices   
- **Pruning/Quantization/Distillation**: reduce inference cost 

#### Medical Text Generation
- **Problems**: generic style, synonym mismatch, factual inconsistency   
- **Evaluation**:  
  - BERTScore for semantic similarity   
  - factENT/factENTNLI for factual completeness & consistency   
- **Training**: combine NLL with RL using factual rewards   
- **Architectures**: image-conditioned encoder-decoder, meshed attention, memory modules 

#### Image-Text Contrastive Learning
- **ConVIRT**: CLIP-style contrastive pre-training on X-ray/report pairs   
- **ChexZero**: zero-shot classification via prompt templates & cosine similarity   
- **Joint Modeling**: single-stream multimodal transformer with alignment objectives 
