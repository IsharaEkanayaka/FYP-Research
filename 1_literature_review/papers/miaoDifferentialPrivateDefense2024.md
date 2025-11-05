---
title: Differential Private Defense Against Backdoor Attacks in Federated Learning
authors: Lu Miao, Weibo Li, Jia Zhao, Xin Zhou, Yao Wu
year: "2024"
tags:
  - Backdoor Attack
  - Federated Learning
  - Adversarial Machine Learning
  - Differential Privacy
  - federated-learning
  - backdoor
  - ds
source: https://doi.org/10.54097/dyt1nn60
---
**Read:** [Open in Zotero](zotero://select/items/2_TTFEPIGP)

# 🧠 Summary

This paper addresses the vulnerability of **[[Federated Learning]] (FL)** to **[[backdoor]] attacks**, where malicious clients insert triggers into models to manipulate their behavior. While **[[Differential Privacy]] (DP)** has been used to defend against such attacks, it often severely degrades model accuracy.  
To overcome this, the authors propose **Clip Norm Decay (CND)** — a DP-based mechanism that **gradually reduces the clipping threshold** during training. This adaptive thresholding minimizes injected noise and bounds malicious updates, preserving both **privacy and model utility**.



---

# 🎯 Problem Statement

Federated learning systems are susceptible to **backdoor attacks** due to untrusted local training on client devices.  
Existing DP-based defenses can lower attack success rates but often cause **significant performance loss**.  
The paper aims to create a **DP-based defense that maintains model accuracy** while resisting both **backdoor** and **privacy** attacks.

---

# ⚙️ Method / Approach

### **Technique Used:**

- **Clip Norm Decay (CND)** — dynamically decays the gradient clipping norm across rounds to inject less noise as the model converges.
### **Core Idea:**

- In user-level DP, gradients are clipped and noise is added.
- As training proceeds, model updates naturally shrink in magnitude.
- By **decreasing the clipping norm**, CND injects less noise without compromising DP guarantees.
- This ensures smaller malicious gradients are clipped, reducing their impact.
### **Algorithmic Flow:**
1. Initialize global model and clipping norm c0c_0c0​.
2. In each FL round, clients train locally and clip updates using ctc_tct​.
3. The server updates ctc_tct​ adaptively using decay coefficient γ\gammaγ:
    ct+1=γ⋅ctc_{t+1} = \gamma \cdot c_tct+1​=γ⋅ct​
    (or based on average client update norms).
4. Gaussian noise proportional to ctc_tct​ is added to preserve DP.
5. Privacy loss tracked via **Moments Accountant**.

### **Datasets:**
- **[[CIFAR-10]]** – semantic backdoor attacks (red car → bird).
- **EMNIST[[MNIST]]** – single-pixel attacks (bottom-right pixel → label 0).

### **Evaluation Metrics:**
- **Main task accuracy** [[MTA]]
- **Attack success rate** [[ASR]]
- **Area Under Curve (AUC)** for property inference

---

# 📊 Results

|**Defense Method**|**Accuracy (Main Task)**|**Attack Success Rate**|**Observation**|
|---|---|---|---|
|No Defense|88–99%|Up to 99%|Vulnerable|
|Central DP (CDP)|↓ 20–40%|↓ but high noise|Significant accuracy loss|
|**DP + CND**|↑ +20% over CDP|↓ close to 0%|Maintains accuracy and privacy|
|Other Defenses (Krum, Median, Weak DP)|Mixed|Higher than CND|Attack-type dependent|

Property Inference (Privacy Attack):

|Clients|No Defense AUC|DP AUC|**CND AUC**|
|---|---|---|---|
|2|0.81|0.53|**0.50**|
|5|0.71|0.50|**0.51**
➡️ CND achieves **similar privacy preservation** as DP while improving accuracy by **20–30%**.

---

# 💡 Key Insights

**Strengths:**
- Maintains privacy under (ε, δ)-DP while **greatly improving utility**.
- Robust against **both model poisoning and data poisoning** attacks.
- Generalizable to **property inference attacks** (privacy threats).

**Weaknesses:**
- Requires careful **threshold decay tuning (γ)**.
- May not fully handle non-backdoor Byzantine behaviors.

**Could Combine With:**
- Robust aggregation methods (e.g., Krum, Trimmed Mean) for additional Byzantine resilience.

**Might Help In:**
- Federated learning systems for IoT, mobile devices, and healthcare — where both **privacy and trust** are essential.

---

# 🧩 Related Works
> How does this relate to other papers or concepts?

- Builds on **Differential Privacy** (Dwork, 2006) and **user-level DP** (McMahan et al., 2018).
- Extends ideas from **Weak DP** and **Norm Bounding** (Sun et al., 2019).
- Improves upon **Local Differential Privacy (LDP)** and **Central DP (CDP)** defenses.
- Addresses limitations in **Krum** and **Median** aggregation methods designed for Byzantine attacks.

---

# 💬 Notes / Quotes

“Reducing the clipping threshold can introduce less noise and obtain a higher model accuracy.”  
“CND enables the model update to be continuously clipped and limits the influence of the malicious gradient.”  
“Our method provides the same privacy preservation capability as the original DP and enhances the main task accuracy significantly.”

---

# 🧠 My Thoughts
> Reflect on how this influences your FYP.

Example:  
I can use this aggregation method as the base for my framework, then add a DP noise layer to enhance privacy. Test on CICIDS2017 dataset.

---

# 📚 Citation
> (Li et al., 2022) – *Robust Aggregation for Federated Learning*