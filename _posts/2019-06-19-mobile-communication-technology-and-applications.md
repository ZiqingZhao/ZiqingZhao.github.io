---
layout: post
title: "Mobile Communication Technology and Applications"
date: 2019-06-19
excerpt: "Study outline"
tags: [Mobile Communication]
comments: true
---
### 1. Overview

#### 1.1 Definition and Main Characteristics
- **Definition:** A mobile communication link involves at least one communicating party in motion. It covers fixed-to-mobile, mobile-to-mobile, and person-to-person mobile scenarios.
- **Key traits:** Uses radio waves, operates in heavy-interference environments (Doppler, shadowing, multipath), works with scarce spectrum, complicates network management because of mobility, and demands high-performance user equipment.

#### 1.2 Development Trends
- Mobile subscriptions outpace fixed lines and drive the ICT industry.
- Mobile data traffic keeps rising; mobile Internet access becomes essential.
- Terminals evolve toward intelligent, broadband, standardized devices that become primary Internet access points.
- Networks trend toward convergence, intelligence, and global coverage.

### 2. Wireless Channel

#### 2.1 Propagation Characteristics
- **Propagation modes:** Received signals combine direct, reflected, diffracted, and scattered components.
- **Impairments:** Path loss grows with distance and frequency; terrain/obstacles introduce **shadow fading**; multipath superposition causes random amplitude/phase/delay variations (**multipath fading**); motion induces **Doppler shifts** that broaden spectra; other transmitters and environmental noise add interference.
- **Mobile channel:** A time-varying channel whose fading profile results from the combined effects of path loss, shadowing, and small-scale fading.

#### 2.2 Fading
- **Definition:** Rapid fluctuations of received field strength caused by complex propagation paths.
- **Classification:** Frequency-selective vs. flat fading; fast vs. slow fading.
- **Effects:** Lower received level, waveform distortion, delay spread that breaks synchronization, and tracking challenges under fast fading.
- **Large-scale (slow) fading:** Caused by path loss and shadowing. Path loss increases with distance and carrier frequency. Example outdoor models: Okumura (1500–1920 MHz macro cells) and Hata (150–1500 MHz, radius > 1 km). Indoor: Keenan–Motley. Shadowing produces log-normal envelopes:

  $$
  P(r)=\frac{1}{\sqrt{2\pi\mu_s}}\exp\left[-\frac{(r-m)^2}{2\mu_s^2}\right]
  $$

  Mitigation: deploy additional base stations.
- **Small-scale (fast) fading:** Driven by multipath. Envelopes typically follow Rayleigh or Rician distributions. **Time dispersion** is described by mean excess delay and RMS delay spread; **coherence bandwidth** satisfies $\Delta f=1/(6\pi\sigma_\tau)$ for correlation 0.9 or $\Delta f=1/(2\pi\sigma_\tau)$ for 0.5. Larger delay spreads narrow $\Delta f$ and exacerbate intersymbol interference (ISI). **Coherence time** relates to Doppler spread by $T_c=\sqrt{9/(16\pi f_m^2)}$. Mitigation: diversity reception.

### 3. Voice Coding, Channel Coding, and Modulation

#### 3.1 Voice Coding
- **Compression rationale:**  
  - External: Speech has redundancy—non-uniform amplitude distribution and strong short/long-term correlation.  
  - Internal: Human auditory masking tolerates quantization noise and phase distortion.
- **Categories:**  
  - Waveform coders (PCM, ADPCM) keep high fidelity at higher bitrates.  
  - Parametric coders (vocoder/LPC-10) encode model parameters at low bitrates with poorer quality.  
  - Hybrid coders (CELP) blend both ideas for better quality at low rates.
- **Requirements:** Low rate (<16 kbps pure coding), high quality, low delay (tens of ms), robustness to channel errors, moderate complexity.
- **Key techniques:** Short-time analysis (frames of 10–30 ms, often via HMMs), pitch estimation (autocorrelation/average magnitude difference), linear prediction to remove short-term correlation, and vector quantization for residuals.
- **CELP:** Uses analysis-by-synthesis with both fixed and adaptive codebooks. Frames (20–30 ms) split into subframes; the best excitation vector per subframe is chosen. Perceptual weighting shapes noise using auditory masking. Redundancy removal spans intra-frame (LP) and inter-frame (adaptive codebook) correlations; remaining residuals are approximated by the fixed codebook.
- **Quality evaluation:** Subjective MOS scores (1–5) or objective metrics such as SNR and PESQ.

#### 3.2 Channel Coding
- **Definition:** Adds parity symbols to satisfy constraints so the codeword can withstand channel impairments.
- **Purpose:** Improve transmission reliability.
- **Classification:** Block vs. convolutional; linear vs. nonlinear; error-detecting vs. error-correcting.
- **Block codes:** Expand k-bit messages to n-bit codewords by inserting n−k parity bits.
- **Convolutional codes:** Simple encoders with strong performance but complex decoders; excel at random errors but not bursts.
- **Interleaving:** Rearranges coded bits to turn burst errors into random ones; not a code itself but prepares data for random-error-correcting codes.
- **Turbo codes:** Parallel concatenation of two convolutional encoders separated by an interleaver; outputs systematic data plus two parity streams for an effective 1/3 rate. Decoding iteratively exchanges soft information to approach Shannon limits.
- **LDPC codes:** Sparse linear block codes that offer near-capacity performance with low-complexity, parallelizable decoding and built-in error detection.
- **Polar codes:** Capacity-achieving block codes based on channel polarization.

#### 3.3 Modulation
- **Signal-space analysis:** Digital symbols map to waveform vectors via orthonormal basis functions such as

  $$
  \phi_1(t)=\sqrt{\frac{2}{T}}\cos(2\pi f_ct),\quad
  \phi_2(t)=\sqrt{\frac{2}{T}}\sin(2\pi f_ct)
  $$

  Each signal $s_i(t)$ is represented by coefficient vector $s_i$, and constellation distances follow

  $$
  \|s_i-s_k\|=\sqrt{\sum_{j=1}^N(s_{ij}-s_{kj})^2}=\sqrt{\int_0^T (s_i(t)-s_k(t))^2 dt}.
  $$

- **Modulation families:** Non-constant envelopes (ASK, QAM, APSK) and constant envelopes (FSK, PSK, CPM). PSK/QAM constellations illustrate symbol placement.
- **MSK:** Minimum shift keying with constant envelope, narrow bandwidth, coherent demod capability but strict adjacent-channel interference limits.
- **GMSK:** Gaussian-filtered MSK that shapes data before modulation.
- **Multicarrier modulation:** Splits data into many low-rate substreams, each modulating an orthogonal subcarrier so every subchannel is narrower than the coherence bandwidth.
- **OFDM:** Applies parallel orthogonal subcarriers plus cyclic prefixes to convert a frequency-selective channel into flat subchannels. Advantages: high spectral efficiency, FFT-based implementation, supports adaptive modulation, asymmetric services. Drawbacks: sensitivity to frequency offsets/phase noise (ICI) and high PAPR.

### 4. Performance Enhancement Techniques

#### 4.1 Spread Spectrum
- **Features:** Uses bandwidth far exceeding the original signal via high-rate pseudo-random codes. Provides resilience to intentional and narrowband interference and mitigates multipath.
- **Shannon rationale:** For $S/N \ll 1$, $C/B \approx 1.44 S/N$, so increasing bandwidth allows reliable communication at low SNR. After despreading, desired signals collapse back into a narrow band while interference remains dispersed.
- **DSSS:** Multiply the data stream by a pseudo-random code, modulate, transmit, then despread using a synchronized local code before demodulation.
- **FHSS:** Drive the RF oscillator with a PN sequence so the carrier hops rapidly; effectively a sequence of narrowband transmissions over a wide spectrum.
- **Codes:** Ideal codes provide many sequences, sharp autocorrelation, zero cross-correlation, and high complexity. Walsh codes are orthogonal yet unsuitable as spread-spectrum codes because different codewords occupy unequal bandwidths. PN codes (m-sequences, Gold sequences) approximate noise and work well.

#### 4.2 Diversity
- **Concept:** Send the same data over independent fading paths so the probability of simultaneous deep fades is low, improving SNR by 10–20 dB after combining.
- **Classes:** Micro-diversity combats multipath fading; macro-diversity handles shadowing.
- **Implementations:**  
  - Spatial diversity via antenna arrays (spacing ≈ half wavelength).  
  - Polarization diversity (vertical/horizontal) with up to two branches and a 3 dB split loss.  
  - Angle diversity using directional beams.  
  - Frequency diversity with carriers separated beyond coherence bandwidth (often using RAKE).  
  - Time diversity via repeated transmissions or coding/interleaving beyond coherence time.
- **Combining:** Selection combining (pick best branch), equal-gain combining, and maximal ratio combining (weight by SNR), with performance ordered MRC > EGC > SC.
- **RAKE receivers:** Use multiple correlators aligned to different multipath delays, then align and combine. Variants include A-RAKE (all paths), S-RAKE (strongest L paths), and P-RAKE (earliest L paths).

#### 4.3 MIMO
- **Definition:** Multiple antennas at both transmitter and receiver create several parallel spatial channels.
- **Modes:** Spatial multiplexing (split data into low-rate streams and separate them via detection), spatial diversity (orthogonal encoding across antennas), and beamforming (focus energy using channel knowledge to boost SNR and reduce interference).
- **Core techniques:** Space-time processing, including space-time trellis codes (STTC) and space-time block codes (STBC).

#### 4.4 Equalization
- **Time-domain equalizers:** Manipulate impulse responses directly, using linear or nonlinear structures, to meet ISI-free conditions.
- **Frequency-domain equalizers:** Shape amplitude and group delay of the overall response (channel × equalizer).
- **Adaptive equalization:** Continuously tunes coefficients via training and tracking modes to follow channel variations.

### 5. Networking Technologies

#### 5.1 Cellular Architecture
- **Cell types:** Belt-shaped service areas (two- or three-frequency) and planar honeycomb layouts. Advantages include higher frequency reuse, flexible deployment, and complex topology.
- **Clusters:** $N_R$ cells sharing the full band form a cluster (size $N_R$). Cells inside a cluster use distinct frequencies; reuse occurs only between different clusters. Adjacent clusters must maintain equal co-channel spacing.
- **Reuse factor:** $Q=D/R=\sqrt{3N_R}$, where $D$ is co-channel distance and $R$ cell radius. Larger $Q$ reduces interference but lowers capacity.
- **Carrier-to-interference ratio:**

  $$
  \frac{S}{I}=\frac{R^{-n}}{\sum_{i=1}^{i_0}D_i^{-n}}
  $$

  Assuming equal-distance first-tier interferers, $S/I = (\sqrt{3N})^n/i_0$.
- **Excitation:** Center-fed or vertex-fed cells.
- **Cell splitting:** Decrease cell radius or increase per-cell channels to boost capacity in dense areas.

#### 5.2 Multiple Access
- **Definition:** Assign distinguishing signatures so multiple users share a common medium, raising spectral efficiency.
- **Types:** FDMA, TDMA, CDMA, SDMA.
- **FDMA:** Divides total bandwidth into disjoint subbands (channels) with guard bands; suitable for narrowband systems, requires band-pass filters, and complicates handover. Interference sources include adjacent-channel, co-channel, and intermodulation.
- **TDMA:** Partitions channels by time slots (see frame diagram). Offers high burst rates, removes duplexer needs, simplifies handover, but requires adaptive equalization at high rates.
- **CDMA:** Assigns quasi-orthogonal PN codes per user. Handles multiple users on the same spectrum, offers soft capacity, mitigates multipath, supports smooth soft handover and macro diversity, but must address multiple-access interference and near-far effects.
- **SDMA:** Uses adaptive antenna arrays to form beams toward different users, boosting capacity, coverage, compatibility, and localization while lowering interference and power, but must be combined with other MA schemes.
- **System capacity:** $m = \frac{B_t}{B_c N} = \frac{B_t}{B_c\sqrt{\frac{2}{3}\left(\frac{C}{I}\right)_{min}}}$.

### 6. GSM

#### 6.1 System Overview
- **Services:**  
  - *Bearer services* (restricted voice, async/sync duplex data, packet options).  
  - *Telecommunication services* (voice, emergency, SMS, fax).  
  - *Supplementary services* such as caller ID, conference calling, charging alerts.
- **Architecture:**  
  - *Mobile Station (MS):* Mobile terminal plus SIM.  
  - *Base Station Subsystem (BSS):* BTS, BSC, and PCU handling radio coverage and resources.  
  - *Network and Switching Subsystem (NSS):* MSC, VLR, HLR, EIR, AUC for routing, management, security, and mobility.  
  - *Operation Support Subsystem (OSS):* OMC and peripherals for O&M tasks.

#### 6.2 Channels
- **Time-slot/frame hierarchy:** Slots (0.577 ms) form TDMA frames (8 slots, 4.615 ms), which compose 26-frame traffic multi-frames or 51-frame control multi-frames; superframes contain 51 traffic or 26 control multi-frames (1326 frames, 6.12 s); hyperframes contain 2048 superframes (3 h 28 min 53.760 s).
- **Logical channels:** Traffic channels (full/half-rate voice or data) plus control channels (BCH, CCCH, DCCH).
- **Mapping:** Each RF carrier offers eight physical channels but more logical channels, so multiplexing is required. Example: carrier $C_0$ uses TS0–TS1 for control, TS2–TS7 for traffic; other carriers devote all slots to traffic.
- **Burst types:** Normal, frequency-correction, synchronization, access, and idle bursts.

#### 6.3 Radio Transmission Technologies
- **Speech codec:** RPE-LTP (Regular Pulse Excited with Long-Term Prediction) at 8 kHz sampling, 20 ms frames, 260 bits per frame (13 kbps).
- **Channel coding / interleaving:** See block diagram; GSM applies two-stage interleaving (intra-frame then block interleaving).
- **Modulation:** GMSK.
- **Other techniques:** Fading mitigation (diversity, adaptive filtering, frequency hopping) and power-saving features (adaptive power control, discontinuous transmission/reception).

#### 6.4 Control and Management
- Covers registration, roaming, and handover procedures.

#### 6.5 GPRS
- Packet-switched enhancement that avoids permanent connections between the mobile station and external networks, yielding higher data efficiency.

#### 6.6 EDGE
- Enhanced Data rate for GSM Evolution introduces 8PSK modulation, new MCS coding, link adaptation, incremental redundancy ARQ, updated RLC/MAC, and improved link-quality monitoring.

### 7. CDMA IS-95

#### 7.1 System Overview
- Uses three code families: short PN codes (cell identification), Walsh codes (forward-channel separation), and long PN codes (reverse-link user separation).

#### 7.2 Forward Link
- Downlink supports up to 64 simultaneous channels on one RF carrier, distinguished by orthogonal Walsh sequences.
  - **Pilot channel:** Unmodulated DSSS signal for timing, coherent demodulation, and handover decisions.
  - **Sync channel:** Broadcasts synchronization data.
  - **Paging channel:** Broadcasts system parameters, pages mobiles, and sends control messages before traffic assignment.
  - **Traffic channels:** Carry user payload plus in-band signaling such as power-control or handover commands; include power-control and associated subchannels.

#### 7.3 Reverse Link
- Uplink uses long-code phase offsets for user separation; Walsh codes provide 64-ary modulation rather than channelization.
  - **Access channel:** Mobile-to-base signaling path before traffic assignment.
  - **Reverse traffic channel:** Carries payload plus auxiliary services or signaling.
- **Forward vs. reverse differences:** Convolutional rates (1/2 vs. 1/3), roles of Walsh codes, long-code usage (scrambling vs. user ID), and modulation (QPSK vs. OQPSK).

#### 7.4 Enhancements
- **Voice codec:** Qualcomm CELP (QCELP) with rate adaptation based on thresholds tied to ambient noise.
- **RAKE receiver, power control, voice activity detection**, and **soft/hard handover** (soft handover establishes the new link before releasing the old one).

#### 7.5 IS-95 Features
- Large/soft capacity, soft handover, high voice quality, low transmit power, voice activity, and inherent secrecy.

### 8. 3G

#### 8.1 Characteristics
- **Global:** IMT-2000 unifies multiple systems with common services and interworking for worldwide roaming.
- **Integrated:** Combines paging, cellular, and satellite systems.
- **Multimedia:** Supports high-quality voice, variable-rate data, and rich video.
- **Personalized:** Users leverage a unique personal telecom number (PTN) across terminals for personal mobility.

#### 8.2 WCDMA
- **Network:** UE (ME + USIM), UTRAN (RNC + Node B), and core network (CN) for external connectivity and control.
- **Channels:** Logical channels describe payload types; MAC maps them to transport channels (dedicated DCH or shared channels such as RACH, BCH, PCH, FACH, CPCH, DSCH, HS-DSCH), which then map to physical channels (uplink/downlink).
- **Air interface:** 5 MHz bandwidth, 3.84 Mcps chip rate, AMR voice codec, supports synchronous/asynchronous base stations, combined inner/outer loop power control, downlink open/closed-loop transmit diversity, pilot-aided coherent demodulation, convolutional/Turbo coding, BPSK uplink and QPSK downlink.

#### 8.3 CDMA2000
- **Network:** Similar layered structure (see diagram).
- **Air interface:** CDMA2000-compatible with IS-95; bandwidth $N\times1.25$ MHz with chip rate $N\times1.2288$ Mcps (N = 1,3,6,9,12), 8k/13k QCELP or 8k EVRC voice, GPS/GLONASS synchronization, dual-loop power control, downlink OTD/STS transmit diversity, pilot-aided coherent demodulation, convolutional/Turbo coding, BPSK uplink and QPSK downlink.

#### 8.4 TD-SCDMA
- **Network:** See architecture figure.
- **Key technologies:** Mixes TDMA/CDMA/FDMA/SDMA for adaptive resource allocation; flexible uplink-downlink timeslot partitioning; power control to counter breathing/near-far; smart antennas to form user-specific beams and exploit spatial resources.
- **Advantages:** High spectral efficiency, flexible coverage, and compatibility with asymmetric traffic.

### 9. 4G

#### 9.1 Characteristics
- All-IP design with high data rates, low latency, adaptive QoS, seamless mobility, and tight integration with Internet services.

#### 9.2 LTE
- **Key technologies:** Supports FDD, TDD, and half-duplex FDD. Frame type 1 (10 ms, 20 slots, 10 subframes) and type 2 (two half-frames with 0.5 ms slots plus special downlink/uplink pilot/protection slots). Employs OFDM for spectral efficiency and multipath resilience.
- **CDMA vs. OFDM comparison:**  
  - *Modulation:* CDMA enforces uniform modulation per link; OFDM allows per-link adaptive modulation for better spectral-use/BEP trade-offs.  
  - *PAPR:* CDMA PAPR ≈ 5–11 dB and grows with rate/codes; OFDM’s non-constant envelope makes it sensitive to nonlinearity without mitigation.  
  - *Narrowband interference:* CDMA spreads interference over a small fraction of the spectrum; OFDM can disable interfered subcarriers or rely on FEC/lower-order modulation.  
  - *Multipath:* CDMA leverages RAKE but loses accuracy beyond ~7–8 significant paths; OFDM lowers symbol rate via serial-to-parallel conversion and adds CPs to suppress ISI, albeit with bandwidth/energy penalties.  
  - *Equalization:* CDMA often forgoes equalizers because spreading handles delay spread; OFDM already exploits frequency diversity, so extra equalization is usually unnecessary.

#### 9.3 LTE-Advanced
- **Key technologies:** Uplink SC-FDMA enhancements, improved interference suppression, modulation up to 64QAM; carrier aggregation for wider effective bandwidth while staying backward compatible; relay nodes to extend coverage and edge throughput with lower complexity than base stations; MIMO upgrades (downlink 8×8, uplink 4×4); coordinated multi-point transmission, distributed antennas, inter-base-station coordination, and enhanced multimedia broadcast/multicast services.
