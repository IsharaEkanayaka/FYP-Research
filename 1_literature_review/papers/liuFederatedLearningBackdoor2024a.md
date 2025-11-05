---
title: Federated Learning Backdoor Attack Based on Frequency Domain Injection
authors: Jiawang Liu, Changgen Peng, Weijie Tan, Chenghui Shi
year: "2024"
tags:
  - federated-learning
  - backdoor
  - ds
source: https://doi.org/10.3390/e26020164
---
**Read:** [Open in Zotero](zotero://select/items/2_27MCCI6Y)

# 🧠 Summary

This paper proposes a novel **[[backdoor]] attack method for [[Federated Learning]] (FL)** that operates in the **frequency domain** rather than the traditional spatial domain. By using a **Fourier transform**, the attack blends low-frequency information from a trigger image into clean images—preserving semantic content and making the trigger invisible to human eyes. Experiments show this **frequency-domain injection** produces **stealthier and more effective** attacks compared to pixel-pattern and blended-image backdoors.

---

# 🎯 Problem Statement

Traditional backdoor attacks in FL rely on **visible spatial triggers** (e.g., pixel patterns) that are:

- **Easily detectable** by humans or defenses, and
- **Semantically disruptive**, altering the meaning of the poisoned samples.

This paper addresses the need for **imperceptible, semantically consistent, and effective backdoor attacks** that can evade current defenses in FL systems.

---
# ⚙️ Method / Approach

**Core idea:**  
Inject the trigger in the **frequency domain** using **Fourier Transform-based linear amplitude mixing**, while keeping the phase (semantic) information intact.

**Steps:**

1. Perform Fast Fourier Transform (FFT) on the clean image and trigger image.
2. Mix their **amplitude spectra** linearly under a binary low-frequency mask `M`.
3. Combine the new amplitude spectrum with the original phase spectrum using **inverse FFT** to generate a poisoned image.
4. Inject these poisoned samples into the malicious client’s training data.
5. Use **Projected Gradient Descent (PGD)** to keep local model updates small (≤ε), maintaining stealth during aggregation.

**Key Formula:**

Axi′=[(1−α)Axi+αAxt]⋅M+Axi(1−M)A'_{xi} = [(1 - \alpha)A_{xi} + \alpha A_{xt}] \cdot M + A_{xi}(1 - M)Axi′​=[(1−α)Axi​+αAxt​]⋅M+Axi​(1−M) xi′=F−1(Axi′,Pxi)x'_i = F^{-1}(A'_{xi}, P_{xi})xi′​=F−1(Axi′​,Pxi​)

**Parameters:**
- Mixing ratio α = 0.15
- Low-frequency range β = 0.2
- Model update constraint ε = 0.01–0.05

**Datasets:** [[CIFAR-10]], GTSRB, ISIC-2019  
**Models:** ResNet-18 and VGG-13  
**Metrics:**
- **[[ASR]] (Attack Success Rate)**
- **ACC (Accuracy on clean data)**
- **PSNR / SSIM** for image quality
- Defense robustness under norm clipping, DP, and fine pruning.

---
# 📊 Results

|Dataset|Model|Attack|ASR|ACC|PSNR|SSIM|
|---|---|---|---|---|---|---|
|CIFAR-10|ResNet18|Frequency|**99.3%**|75.8%|**29.95**|**0.977**|
|GTSRB|ResNet18|Frequency|**99.7%**|92.3%|**29.56**|0.944|
|ISIC-2019|ResNet18|Frequency|**99.2%**|78.0%|26.58|0.892|
- Frequency-domain triggers achieve **similar ASR to pixel triggers** but with **much higher stealth (PSNR/SSIM)**.
- With PGD constraints, the attack remains **undetected under norm clipping** and **weak differential privacy**.
- Outperforms other attacks in **multi-trigger and continuous attack** settings.
---

# 💡 Key Insights

- **Strengths:**
    - Stealthy: frequency-domain triggers invisible to human eyes.
    - Robust: survives norm clipping, differential privacy, and pruning defenses.
    - Maintains high clean accuracy while achieving >99% ASR.
        
- **Weaknesses:**
    - Requires control of client updates (malicious client assumption).
    - Some performance drop under strong PGD constraints.
- **Could combine with:** image steganography or DFT-based embedding.
- **Might help in:** designing or defending FL systems (e.g., detecting frequency-domain anomalies).

---
# 🧩 Related Works
> How does this relate to other papers or concepts?

- **Builds on:** BadNets (Gu et al., 2017), DBA (Xie et al., 2020), CRFL (Xie et al., 2021).
- **Similar to:** FIBA (Feng et al., 2022) — frequency-based backdoor in centralized learning.
- **Contradicts:** Pixel-based and blend-based triggers that modify spatial pixels.

---

# 💬 Notes / Quotes

“By performing Fourier transform, the trigger and clean image are linearly mixed in the frequency domain, injecting low-frequency information of the trigger while preserving semantic information.”
“Our method demonstrates that current defense strategies fail to detect frequency-domain based backdoor attacks.”

---

# 🧠 My Thoughts
> Reflect on how this influences your FYP.

- Can adapt this approach to **simulate stealthy poisoning attacks** on FL-based systems (e.g., IoT or healthcare).
- Could also explore **defense mechanisms** by detecting **frequency anomalies or phase inconsistencies**.
- Shows the importance of analyzing **non-spatial attack vectors** in FL.
---

# 📚 Citation
> (Li et al., 2022) – *Robust Aggregation for Federated Learning*