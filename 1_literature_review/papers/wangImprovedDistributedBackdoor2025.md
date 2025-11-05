---
title: Improved Distributed Backdoor Attacks in Federated Learning by Density-Adaptive Data Poisoning and Projection-Based Gradient Updating
authors: Jian Wang, Hong Shen, Wei Ke, Xue Hua Liu
year: "2025"
tags:
  - backdoor
  - ✅
  - ds
source: https://doi.org/10.1109/ACCESS.2025.3586416
---
**Read:** [Open in Zotero](zotero://select/items/2_BXPYGPNE)

---

# 🧠 Summary
This paper proposes an enhanced distributed [[backdoor]] attack (DBA) method for [[Federated Learning]] that uses density-adaptive data [[poisoning]] and projection-based gradient updating. Unlike traditional DBAs that use uniform triggers, this approach decomposes global triggers into sub-triggers aligned with client data distributions (using edge structures) and constrains malicious gradient updates to mimic legitimate patterns. On COCO dataset, the method achieves 92.44% main task accuracy with 91.68% attack success rate, outperforming baseline DBA by +8.04% MTA and +10.29% ASR while evading six state-of-the-art defenses.


---

# 🎯 Problem Statement
Traditional distributed backdoor attacks in federated learning suffer from two key limitations: (1) uniform trigger designs that create detectable artifacts in sparse data regions, and (2) unconstrained parameter updates that violate FL verification protocols (gradient norm constraints, direction consistency checks). These weaknesses make existing DBAs vulnerable to clustering analysis, anomaly detection, and robust aggregation defenses, compromising both stealthiness and effectiveness.

---

# ⚙️ Method / Approach
>>The attak operates in three phases with a dual-layer enhancement strategy:

- **Technique used:** Density-adaptive trigger generation + Projected Gradient Descent (PGD)
- **Framework type:** Distributed backdoor attack with encoder-decoder architecture
- **Dataset:** ImageNet (224×224), Pascal VOC (512×512), MS COCO (640×640)
- **Evaluation metrics:** Attack Success Rate (ASR), Main Task Accuracy (MTA)
- **Key components:**

**Phase 1: Trigger Generation**

- Extract edge structures using Canny operator: `p_i = ε(x_i) ⊗ C^p_i ⊗ g(n)`
- Inject isotropic Gaussian noise: `g(n) = (1/√2πσ_n) exp(-(n-μ_n)²/2σ_n²)`
- Decompose global trigger into m=n² sub-triggers (n=2 optimal)

**Phase 2: Trigger Embedding**

- Use denoising autoencoder with stealthiness loss
- Loss function: `L_I = E[||x^p_i - x^c_i||] + L_hid`
- Visual stealthiness via MS-SSIM: `L_hid = 1 - (1/S)∑MS-SSIM_j(x^c_j, x^p_j)`
- Adversarial decoder training to ensure effective poisoning

**Phase 3: Distributed Injection**

- Dynamic loss balancing: `β_t = β_{t-1} · log(λ-1)/log2` (λ = effective poison rate)
- Gradient projection constraint: `||θ* - θ|| ≤ δ`
- PGD update: `θ* ← θ* - η∇L(θ*)` then project onto feasible region
- Threshold δ computed as: `δ = max(α·δ_hist, β·δ_cv, γ·δ_s)` with α=0.3, β=0.5, γ=0.2

---

# 📊 Results

>**Performance Across FL Aggregation Methods (COCO, n=9):**

**Performance Across FL Aggregation Methods (COCO, n=9):**

|Method|Baseline MTA|Attack MTA|Baseline ASR|Attack ASR|
|---|---|---|---|---|
|FedAvg|93.88%|93.38%|7.25%|94.17%|
|FedProx|93.88%|93.48%|7.25%|90.24%|
|FedNova|93.88%|93.78%|7.25%|86.53%|

**Defense Robustness (COCO dataset):**

|Defense|MTA|ASR|vs DBA (ASR)|vs D-DBA (ASR)|
|---|---|---|---|---|
|KRUM|86.34%|90.15%|+5.73pp|+4.46pp|
|RFA|89.62%|93.72%|+11.33pp|+8.04pp|
|FoolsGold|91.62%|94.15%|+12.76pp|+9.47pp|
|FLAME|91.62%|92.42%|+10.98pp|+7.69pp|
|CRFL|90.22%|89.45%|+7.19pp|+3.90pp|
**Resolution Impact:**

- ImageNet (224px): 86.84% MTA, 85.19% ASR
- VOC (512px): 88.98% MTA, 87.33% ASR
- COCO (640px): 89.44% MTA, 91.96% ASR
- Correlation: Pearson's r=0.93 (p<0.01) between resolution and ASR

---

# 💡 Key Insights
**Strengths:**
    - Edge-based triggers achieve 0.91±0.03 SSIM (vs 0.86±0.05 for KDE-based), superior visual camouflage
    - Projection constraints reduce gradient cosine similarity variance (0.12 vs 0.21 in DBA)
    - Resolution-adaptive: higher resolution provides more "stealthiness space" (O(n²) trigger locations)
    - Robust across 6 SOTA defenses with <3.5% MTA variation
- **Weaknesses:**
    - 18-23% computational overhead from gradient projection
    - Static client assumption doesn't match dynamic FL participation
    - Trigger failure on images <224px resolution
    - Limited to grid-structured data (images)
- **Could combine with:**
    - Differential privacy mechanisms for enhanced privacy guarantees
    - Multi-modal extensions (text: TF-IDF n-grams; time-series: FFT frequency bands)
    - Adaptive defense mechanisms based on density monitoring
- **Might help in:**
    - Understanding vulnerability dimensions in federated learning
    - Developing resolution-aware defense strategies
    - Designing detection mechanisms targeting density-gradient correlations
    - Establishing mathematical frameworks for attack-defense dynamics

---

# 🧩 Related Works
> How does this relate to other papers or concepts?

- Builds on
- Similar to 
- Contradicts 

---

# 💬 Notes / Quotes

**Key Definitions:**

- **Density-Adaptive Weighting:** `p(x) = ∑w_i·p_i(x)` where `w_i = |D_i|^α / ∑|D_j|^α` with α∈[0,1]
- **Multi-Constraint Projection:** System of constraints including L2 norm, cosine similarity, and KL divergence
- **Attack Objective:** `θ*_i = arg max{∑P[θ^{t+1}(A(x_i,Γ_i,N))=y*] + ∑P[θ^{t+1}(x_i)=y_i]}`

**Important Insights:**

- "The density superposition effect across clients causes critical phase transition in parameter space" - explains global attack emergence
- "Edge-based triggers blend naturally with image features (SSIM=0.91)" - justifies design choice
- "Signal-to-noise ratio ∝ 1/√Resolution" - mathematical basis for resolution dependency

**Convergence Guarantee:**

- Theorem 2: After T iterations with η=O(1/√T): `(1/T)∑E[||∇L(θ_t)||²] ≤ O(logT/√T)`

---

# 🧠 My Thoughts
> Reflect on how this influences your FYP.


---

# 📚 Citation
>