---
title:  "Principles of Communications"
excerpt: "Principles of Communications study notes"
date:   2019-9-16 16:59:47 +0000
categories: Notes
tags: 
  - Communications
comments: true
header:
  image: assets/images/zqz.jpg
#   caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
author: Ziqing Zhao
---
These notes summarize the *Principles of Communications* (7th edition, Fan Changxin et al.) with emphasis on digital baseband and bandpass transmission.

### Introduction

Digital systems outperform analog ones thanks to stronger interference immunity, controllable errors, flexible multiplexing, easy integration, low cost, and better security, at the expense of wider bandwidth and stricter synchronization.

#### Communication System Model

The end-to-end model includes the information source, source encoder, channel encoder, modulator, channel, demodulator, channel decoder, source decoder, and destination, with noise typically injected inside the channel.

#### Performance Metrics

- **Effectiveness:** quantified by spectral efficiency $\eta = R_b / B$, where $R_b=R_B \log_2 M = \frac{1}{T_s}\log_2 M$.
- **Reliability:** measured by error probability, including symbol error rate

  $$
  P_e=\frac{\text{number of erroneous symbols}}{\text{total transmitted symbols}}
  $$

  and bit error rate

  $$
  P_b=\frac{\text{number of erroneous bits}}{\text{total transmitted bits}}.
  $$

### Random Processes

- A random process is a collection of random variables indexed by time. Key statistics include the mean $E[\xi(t)]$, variance $D[\xi(t)] = E[(\xi(t)-a(t))^2]$, and correlation functions (auto-, cross-, and covariance).
- **Stationary processes:**  
  - *Strict sense:* distribution invariant under time shifts.  
  - *Wide sense:* constant mean $a$ and autocorrelation $R(\tau)$ depending only on time difference.
- **Ergodicity:** Time averages equal ensemble averages, e.g., $a=\overline{a}$ and $R(\tau) = \overline{R(\tau)}$.
- **Wiener–Khinchin theorem:** relates $R(\tau)$ and the power spectral density $P_\xi(f)$ via Fourier transforms.
- **Linear systems:** Output mean equals $a_iH(0)$; output autocorrelation is $R_{\xi_o}(\tau)=R_{\xi_i}(\tau)*h(t)$; PSD is $P_{\xi_o}(\omega)=|H(\omega)|^2P_{\xi_i}(\omega)$.
- **Narrowband processes:** Represented as $\xi(t)=a_\xi(t)\cos[\omega_ct+\varphi_\xi(t)]=\xi_c(t)\cos \omega_ct-\xi_s(t)\sin \omega_ct$.
- **Sinusoid plus narrowband Gaussian noise:** Analyze quadrature components, envelope, instantaneous phase, and Rice distribution behavior.
- **White noise:** Ideal white noise has flat PSD; practical implementations include low-pass and band-pass white noise, plus narrowband Gaussian noise with defined envelopes and phase distributions.

### Channel Fundamentals

- **Definition:** The medium plus all impairments affecting signal transmission.
- **Mathematical model:** Combines encoder/decoder, channel, noise sources, and represents random processes through conditional probabilities. The coded model adds channel encoding/decoding blocks.
- **Classification:** By medium (wireless vs. wired) and by parameter variability (constant vs. random). Constant-parameter channels maintain deterministic characteristics; random-parameter channels fluctuate unpredictably.
- **Capacity:**  
  - *Discrete:* Shannon capacity accounts for symbol alphabets and transition probabilities.  
  - *Continuous:* $C=B \log_2(1+S/N)$ bits/s for AWGN under average power constraints.

### Analog Modulation Systems

- **Modulation:** Maps baseband information onto a carrier by varying amplitude, frequency, or phase, enabling long-distance transmission, multiplexing, and antenna size reduction.
- **Amplitude modulation (AM):** Covers conventional AM, double-sideband suppressed carrier (DSB), single-sideband (SSB), and vestigial-sideband (VSB). Each has its own modulator model, spectrum, bandwidth, and demodulation method (envelope detection, coherent detection).
- **Noise performance:** Envelope detection is sensitive to additive noise, while coherent detection for DSB/SSB offers better SNR. VSB balances bandwidth and demod ease.
- **Angle modulation:** Frequency (FM) and phase (PM) modulation deliver superior noise immunity via frequency deviation but demand wider bandwidth.
- **System comparison:** Evaluates trade-offs in bandwidth, complexity, and noise resilience.
- **Frequency-Division Multiplexing (FDM):** Assigns distinct carriers/bands to multiple users, relying on filters and guard bands.

### Digital Baseband Transmission

- **Baseband signals:** Includes polar/nonpolar NRZ, RZ, and other pulse shapes, each with specific spectra describable via power spectral density of steady-state ($v(t)$) and alternating components ($u(t)$).
- **Line coding:** Selection criteria cover DC content, bandwidth, clock recovery, and error performance. Common codes: AMI, $HDB_3$, Manchester (bi-phase), differential bi-phase, CMI, and block codes.
- **Transmission system:** Consists of data source, encoder, pulse shaper, channel, equalizer, and receiver. ISI arises when filtered pulses overlap, violating Nyquist criteria.
- **Quantitative ISI analysis:** Uses pulse response, eye diagrams, and Nyquist’s first criterion to ensure zero ISI. Ideal low-pass and raised-cosine spectra offer practical solutions with roll-off factor $\beta$.
- **Noise performance:** Calculates BER for binary bipolar and unipolar signaling under AWGN, highlighting SNR requirements.
- **Time-domain equalization:** Designs linear/nonlinear equalizers to satisfy zero-forcing or MMSE criteria. Adaptive equalizers minimize peak distortion or mean squared error through training and tracking.

### Optimum Reception of Digital Signals

- **Signal statistics:** Characterize correlational relationships among symbol waveforms.
- **Optimality criterion:** Minimum error probability leads to correlator or matched-filter structures.
- **Correlator receiver:** Projects received signals onto orthogonal template functions. BER depends on correlation coefficients:

  $$
  P_b = Q\left(\sqrt{\frac{2E_b}{N_0}}\,\right)
  $$

  with degradation as symbol correlation increases.
- **Matched filter:** Equivalent in performance to the correlator, maximizing output SNR at sampling instants.
- **Practical vs. optimal:** Real receivers approach ideal performance by mitigating implementation losses.

### Digital Bandpass Transmission

- **2ASK:** Binary amplitude shift keying; defined waveform, generation via multipliers, coherent/noncoherent detection, PSD, and bandwidth to first null.
- **2FSK:** Binary frequency shift keying; representation with two tone frequencies, VCO-based generation, coherent and noncoherent demodulation (filters, discriminators), PSD, and bandwidth.
- **2PSK:** Binary phase shift keying; carriers differ by $\pi$ phase, generated via mixers or balanced modulators, demodulated with coherent detectors, exhibits constant envelope and specific PSD/bandwidth.
- **2DPSK:** Differential PSK encodes information in phase transitions, enabling detection without explicit carrier phase recovery.
- **Noise performance:** Compares BER among 2ASK, 2FSK, 2PSK, and 2DPSK under identical SNRs, often summarized in performance charts.

### Error-Control Coding

- Channels can be random, bursty, or mixed, so error control leverages detection/retransmission (ARQ), forward error correction (FEC), or hybrid schemes.
- **Stop-and-wait** and **pull-back** ARQ illustrate basic ARQ strategies.
- **Forward-error-correcting codes:** Block codes with parameters $(n,k)$ add $r=n-k$ parity bits. Hamming distance dictates capabilities: detecting $e$ errors requires $d_0 \ge e+1$; correcting $t$ errors requires $d_0 \ge 2t+1$; correcting $t$ while detecting $e$ requires $d_0 \ge e+t+1$.
- **Simple codes:** Parity bit, two-dimensional parity (for burst errors), constant-weight codes, and complemented codes (parity dependent on number of ones).
