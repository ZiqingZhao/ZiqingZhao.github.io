---
layout: post
title: "AI in Medicine: Foundation Models"
date: 2025-08-01
excerpt: ""
tags: [AI, Medicine, Foundation Models]
comments: true
---

### Definition & Characteristics

- **Foundation Models (FMs)** are large neural networks pre-trained on vast, diverse datasets via self-supervision.  
- They learn general-purpose patterns transferable across tasks and modalities (language, vision, multimodal).  
- Often based on transformer architectures with hundreds of billions of parameters.  
### Opportunities & Challenges in Medicine

- **Opportunities**: enable zero-shot and few-shot learning for imaging, text, and multimodal clinical data.  
- **Challenges**: medical data are private, decentralized, heterogeneous, and often limited in size/type.  
- **Biobank sources** (e.g., UK Biobank, NAKO, Million Veteran Program) offer large-scale cohorts but remain domain-specific.  

### FM Architecture Components

1. **Encoders** map each modality (images, text, tabular) to embedding spaces.  
2. **Prompt encoders** (textual or spatial) guide outputs (e.g., points, boxes, masks).  
3. **Decoders** combine embeddings and prompts into task-specific heads.  

### Vision Encoders & Self-Supervision

- **Vision Transformers** tokenize images into patches + positional embeddings; process via multi-head self-attention.  
- **Masked Autoencoders (MAE)**: encoder sees 25% of patches, decoder reconstructs masked ones—efficient pre-training that boosts downstream generalization .  
- **DINO / DINOv2**: self-distillation without labels; student/teacher paradigm with global-local crop contrastive learning for robust features .

### Multimodal Pre-training

- **CLIP**: dual-encoder contrastive learning on 400 M image–text pairs; enables zero-shot classification via text prompts .  
- **FLAVA**: joint vision-language alignment with unimodal + multimodal encoders on 70 M pairs.  
- **Frozen LLMs**: map visual inputs into LLM embedding space for few-shot captioning & reasoning without updating LLM weights.  

### Dense Prediction & Promptable Segmentation

- **SAM (Segment Anything Model)**: promptable mask generator trained on 1 B masks; supports points, boxes, text prompts for zero-shot segmentation .  
- **Medical SAM**: adapted on 1.57 M medical image–mask pairs across 10 modalities & 30+ cancer types.  
- **DiENTeS**: prompt-conditioned dynamic segmentation using local-global transformers for limited-data tasks.  

### Medical Foundation Model Examples

- **DinoBLOOM**: DINOv2-based FM for hematology single-cell images (380 k cells) .  
- **OpenMind**: 114 k head-and-neck MRI volumes self-supervised FM for 3D segmentation.  
- **BiomedParse**: joint segmentation/detection across nine modalities with meta-object classification & GPT-4 mapping .  
- **MedGemini**: multimodal embedding fusion with web-search & chain-of-reasoning prompting.  
- **VoxelPrompt**: vision-language agent generating executable instructions for grounded medical image analysis.  

### Adaptation Strategies

- FM adaptation avoids costly re-training by leveraging pre-trained weights for downstream tasks.  
- **Key approaches**: Fine-tuning, Few-shot (in-context) learning, Instruction tuning, and hybrid methods.  

### Fine-tuning vs. Transfer Learning

- **Transfer Learning**: freeze all pre-trained layers; train only new task-specific heads.  
- **Fine-tuning**: unfreeze selected pre-trained layers (often later blocks) to adapt representations; balances specificity vs. catastrophic forgetting.  
- **Layer selection** depends on data volume and domain gap:  
  - Similar & small data → fine-tune last layers  
  - Different modality → more layers or full fine-tuning  

### Few-Shot (In-Context) Learning

- **Definition**: model adapts to new tasks by conditioning on K examples in the input prompt, requiring only forward passes.  
- **Advantages**: interpretable, no retraining, real-time adaptation, minimal labeling.  
- **CLIP Example**: convert class names into prompts (“A photo of a tumor”), compute image–text similarity for zero/few-shot classification .  
- **Prompt Tuning** & **Adapters**: learn soft prompt vectors or small bottleneck layers to steer FM without full weight updates.  
- **LoRA (Low-Rank Adaptation)**: inject low-rank updates into weight matrices; efficient scaling to GPT-3/GPT-4 with near full fine-tuning performance .  
- **FSEFT**: few-shot efficient fine-tuning for volumetric organ segmentation using spatial box adapters and anatomical priors.  

### Instruction Tuning

- **Definition**: fine-tune FM on diverse instruction–response pairs to align with user queries.  
- **Benefits**: enhances alignment, supports human-in-the-loop, improves multi-task generalization.  
- **Limitations**: costly to collect high-quality instructions, potential bias amplification, privacy/misinformation risks if data are flawed.  

