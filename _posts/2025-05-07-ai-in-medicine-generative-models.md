---
layout: post
title: "AI in Medicine: Generative Models"
date: 2025-05-07
excerpt: ""
tags: [AI, Medicine, Generative Models]
comments: true
---

## Overview
Generative models are unsupervised deep-learning methods that learn the data distribution $$p_{\text{data}}$$ to create new examples similar to the original dataset. They are useful for **data augmentation** (synthetic training samples) and **representation learning** (discovering patterns for anomaly detection etc.).

Key families:
- **GANs**: sharp samples, adversarial loss  
- **VAEs**: probabilistic latent space, ELBO  
- **Flows**: invertible transforms, exact likelihood  
- **Diffusion**: iterative denoising, stable training  

## 1. Generative Modelling Recap

- **Data distribution** $$p_{\text{data}}$$ (unknown)  
- **Model distribution** $$p_\theta$$ – parameterized to approximate $$p_{\text{data}}$$  
- **Training objectives**:  
  - Likelihood maximization (Flows, VAEs via ELBO)  
  - Adversarial loss (GANs)  
- **Operations**:  
  - **Sampling**: draw novel data points  
  - **Density evaluation**: compute $$p_\theta(x)$$ (Flows only)  


## 2. Deep Generative Models

### 2.1 Definition & Use Cases
- **Definition**: Unsupervised methods that learn $$p_{\text{data}}$$  
- **Sampling**: generate new examples  
- **Representation learning**: discover latent structure  
- **Use cases**:  
  - Data augmentation  
  - Anomaly detection  

### 2.2 Main Families
- **Generative Adversarial Networks (GANs)**  
- **Variational Autoencoders (VAEs)**  
- **Flow-based models**  
  - Normalizing Flows (NF)  
  - Continuous NF (CNF)  
  - Flow Matching  
- **Diffusion Models**  



## 3. Generative Adversarial Networks (GANs)

- **Framework**: Minimax game between  
  - Generator $$G$$: $$G(z)$$ from noise $$z\sim p_z$$  
  - Discriminator $$D$$: $$D(x)$$ real vs fake  
- **Loss**:  
  $$
    \min_G \max_D \; \mathbb{E}[\log D(x)] + \mathbb{E}[\log(1 - D(G(z)))]
  $$
- **Challenges**:  
  - **Instability**: vanishing gradients (if $$D$$ too strong)  
  - **Mode collapse**: low diversity  
    - **Detection**: Fréchet Inception Distance (FID)  
    - **Mitigation**: cGANs, auxiliary info  
- **Applications in Medical Imaging**:  
  - Low-dose CT denoising  
  - Cross-modality synthesis  
  - Segmentation  
  - Anomaly detection  

## 4. Autoencoders

### 4.1 Basic Autoencoder (AE)
- **Structure**: Encoder → latent code → Decoder → reconstruct input  

### 4.2 Denoising AE
- Add noise to input to improve robustness  

### 4.3 Variational Autoencoder (VAE)
- **Latent**: $$q(z|x)$$  
- **Objective**: maximize ELBO  
  $$
    \mathbb{E}_{q(z|x)}[\log p(x|z)] - \mathrm{KL}[q(z|x)\,\|\,p(z)]
  $$

### 4.4 Adversarial AEs
- Combine VAE + GAN objectives for sharper samples  

## 5. Flow-based Generative Models

### 5.1 Normalizing Flows (NF)
- Invertible transforms $$g$$ mapping $$z\sim p_z$$ → data $$y$$  
- **Change of variables**:  
  $$
    \log p(y) = \log p_z(f(y)) + \sum \log\bigl|\det J_f(y)\bigr|
  $$
- **Ops**:  
  - Sampling: $$z \to y = g(z)$$  
  - Scoring: $$y \to z = f(y)$$ → compute likelihood  

### 5.2 Continuous Normalizing Flows (CNF)
- Model dynamics via ODE: $$\frac{dz}{dt} = f(z,t)$$  
- Use neural-ODE solvers for tractable Jacobians  

### 5.3 Flow Matching
- Learn velocity fields to transport simple to data distribution  
- Sample by integrating learned vector fields  

 **Medical Segmentation**:  
- Represents segmentation masks as **Signed Distance Functions (SDF)**  
- Learns an **implicit distribution** over SDF masks conditioned on image $$x$$

From Binary Mask to Truncated SDF
1. **Input**: image $$x$$  
2. **Binary mask** $$\;m\in\{0,1\}^{H\times W}$$  
3. **Signed distance**:
$$
     \mathrm{SDF}(p) = 
     \begin{cases}
       +\min_{q\in\partial S}\|p-q\|, & p\in\Omega\\
       -\min_{q\in\partial S}\|p-q\|, & p\notin\Omega
     \end{cases}
$$
4. **Truncate & normalize** → $$\tilde m_1\sim q(\tilde m\mid x)$$  
5. **Result**: dense SDF map encoding shape boundary $$\partial S$$

Corruption / Generative Process on SDF Masks
- Define a continuous interpolation $$\Psi_t(\tilde m_1\mid x)$$ for $$t\in[0,1]$$:
  - $$\tilde m_0\sim p_0$$ (noise prior)
  - $$\Psi_t(\tilde m_1\mid x) = t\,\tilde m_1 + (1-t)\,\tilde m_0$$
- Equivalent ODE form:
  $$
    \frac{d}{dt}\,\Psi_t(\tilde m_1\mid x)
    = v_{\theta,t}\bigl(\Psi_t(\tilde m_1\mid x),\,x\bigr)
  $$
- Observe intermediate SDF maps $$\tilde m_t$$ and binary masks $$m_t = \mathbb{1}_{\{\tilde m_t\le0\}}$$

Data & Learning Objective
- **Dataset**: $$\mathcal{D}=\{(\tilde m_{1}^{(s)},x^{(s)})\}_{s=1}^S$$, where $$\tilde m_{1}\sim q(\tilde m\mid x)$$  
- **Goal**: learn conditional vector field $$v_{\theta,t}(\tilde m_t,x)$$  
- **Loss** (Conditional Flow Matching):
  $$
    \mathcal{L}_{\mathrm{CFM}}(\theta)
    = \mathbb{E}_{t\sim U[0,1]}\,
      \mathbb{E}_{\tilde m_1,x\sim q}\,
      \mathbb{E}_{\tilde m_t\sim p_t(\cdot\mid \tilde m_1,x)}
      \Bigl[\tfrac12\,\big\|v_{\theta,t}(\tilde m_t,x)
      -\,u_t(\tilde m_t\mid \tilde m_1,x)\big\|^2\Bigr]
  $$
  where $$u_t(\tilde m_t\mid\tilde m_1,x)=\frac{d}{dt}\Psi_t(\tilde m_1\mid x)$$.


## 6. Denoising Diffusion Probabilistic Models

### 6.1 Forward & Reverse Processes
- **Forward** $$q(x_t|x_{t-1}) = \mathcal{N}(\sqrt{1-\beta_t}\,x_{t-1},\,\beta_t I)$$, $$t=1\ldots T$$, $$x_T\!\approx\!\mathcal{N}(0,I)$$  
- **Reverse** $$p_\theta(x_{t-1}|x_t)=\mathcal{N}(\mu_\theta(x_t,t),\sigma_t^2I)$$  

### 6.2 Training
- Predict noise $$\epsilon_\theta(x_t,t)$$ via MSE loss  
- U-Net backbone  

### 6.3 Sampling & Quality
- Iterative denoising $$x_T\to x_0$$  
  - **Pros**: high fidelity, stable  
  - **Cons**: slow (∼T evals)  

### 6.4 Score-Based View & SDEs
- **Score function**: $$\nabla_x\log p(x)$$  
- **Score matching**: learn $$\hat s_\theta(x)\approx\nabla_x\log p_{\text{data}}(x)$$  
- Simulate reverse SDE with learned score  

### 6.5 Accelerating Sampling
- **DDIM**: deterministic, faster (less accurate)  
- **ODE integrators**: Euler, Runge-Kutta, Heun  
- **Schedulers**: step-size / solver choice  

### 6.6 Conditioning Strategies
- **Classifier guidance**: $$\nabla_x\log C(i|x)$$  
- **Classifier-free guidance**: train with/without cond., interpolate  
- **Spatial & Adaptive Group Norm**: inject class/time embeddings  
- **Image-to-image**: DDIM inversion, interpolation, inpainting  
- **Text conditioning**: cross-attention (Stable Diffusion, ControlNet, DreamBooth)  



## 7. Medical Imaging Applications

**1. Medical Image Synthesis**
- **High-resolution brain MRI generation:** Latent diffusion models trained on UK Biobank data (N = 31,740) can produce 160 × 224 × 160 T1-weighted brain MR volumes, with conditional control over age, sex, ventricular size and total brain volume .
- **Privacy-preserving chest X-ray creation:** A two-stage sampling strategy generates synthetic chest radiographs, then excludes any output too similar to real training images to minimize privacy risks .
- **Skin lesion augmentation:** Synthetic skin disease images boost classifier performance in low-data regimes; gains plateau at a 10:1 synthetic-to-real ratio, underscoring the continued value of collecting diverse real cases .
**2. Data Augmentation & Fairness**
- **Distribution-aware sampling:** By learning from unlabeled data across different subgroups, diffusion models can generate under-represented or shifted-distribution examples, improving both robustness and statistical fairness of downstream classifiers .
- **Rare-case synthesis:** Guided diffusion can produce rare surgical instrument views (e.g., cataract tools), yielding over 10 % classification accuracy improvements for these scarce categories .
**3. Medical Image Segmentation**
- **Implicit ensemble segmentation:** Multiple denoising runs conditioned on anatomy are aggregated into a robust segmentation ensemble, smoothing out individual-run variability .
- **3D patch-based segmentation (PatchDDM):** Training on small 3D patches with positional encodings reduces memory use; at inference, overlapping patches are stitched to segment full volumes .    
- **Uncertainty-aware masks:** A pair of networks (AMN and ACN) models both the distribution of plausible ground-truth masks and the diffusion noise process, capturing annotation ambiguity in the output .
- **Diffusion pre-training:** Pre-training on large unannotated datasets, then fine-tuning for tasks like dental radiograph segmentation, has been shown to boost downstream performance.
**4. Anomaly Detection & Lesion Localization**
- **Unsupervised anomaly segmentation:** Latent diffusion trained on healthy brain scans pinpoints low-likelihood regions in new scans and “inpaints” them, revealing potential lesions without any lesion labels .
- **Noise-scale tuning:** Replacing Gaussian with simplex or coarse noise during corruption helps tailor the detector’s sensitivity to lesion size, improving detection of both fine and coarse anomalies .
- **Patch-level detection:** Applying diffusion reconstruction to local voxel patches uncovers small, focal abnormalities that full-volume methods may miss .
- **Weakly supervised lesion localization:** Unpaired healthy/patient image “translation” followed by difference maps, or classifier-free guidance to generate counterfactual “healthy” images, both highlight disease regions using only image-level labels .
**5. Image Reconstruction & Artifact Removal**
- **Accelerated MRI and sparse-view CT:** Score-based diffusion sampling reconstructs high-fidelity images from undersampled k-space or sinogram data, including metal-artifact reduction in CT .
- **K-space cold diffusion:** A noise-free reconstruction approach learns a direct mapping in k-space, simplifying accelerated MRI workflows without explicit noise scheduling .
**6. Contrast Harmonization & Cross-Modal Synthesis**
- **MRI contrast harmonization:** Diffusion models translate between 1.5 T and 3 T scans, aligning visual and statistical properties to improve multi-center study consistency .
- **2D → 3D volume generation:** Models trained on 2D microscopy or slice data have been extended to synthesize full 3D shapes and cross-modality brain MRI volumes .
**7. Image Registration**
- **DiffuseMorph:** An unsupervised, continuous deformable registration framework uses diffusion’s iterative refinement to align moving and fixed images along a smooth trajectory .
**8. Text-Driven Medical Image Generation**
- **RoentGen:** By jointly fine-tuning a U-Net and text encoder on MIMIC-CXR, this pipeline generates chest X-rays directly from free-text prompts, enabling novel vision-language diagnostic tools .

## 8. Generative Learning Trilemma

		    Sample Quality
		        ▲
		        │
Speed ←─────┼─────→ Likelihood Tractability