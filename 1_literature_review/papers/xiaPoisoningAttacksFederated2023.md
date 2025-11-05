---
title: "Poisoning Attacks in Federated Learning: A Survey"
authors: Geming Xia, Jian Chen, Chaodong Yu, Jun Ma
year: "2023"
tags:
  - Data models
  - Federated learning
  - Security
  - Servers
  - Privacy
  - defense of poisoning attacks
  - distributed machine learning
  - Distributed processing
  - Machine learning
  - poisoning-attacks
  - federated-learning
  - ds
source: https://doi.org/10.1109/ACCESS.2023.3238823
---
**Read:** [Open in Zotero](zotero://select/items/2_9KFMF498)

# 🧠 Summary

This survey provides a detailed and structured overview of **[[poisoning]] attacks** and their **defense strategies** in **Federated Learning (FL)**. It classifies attacks by **method** (data poisoning vs. model poisoning) and **intent** (targeted, semi-targeted, untargeted). The authors also group defenses into **Model Analysis**, **Byzantine-Robust Aggregation (BRA)**, and **Verification-Based** methods. Finally, it discusses **privacy-preserving challenges** and identifies **future research directions** to build more secure FL systems.



---

# 🎯 Problem Statement

Federated Learning protects user data by training decentralized models, but it remains vulnerable to **poisoning attacks**, where malicious clients upload corrupted models or data to manipulate or degrade the global model.  
Existing surveys lack a **privacy-preserving perspective** on these attacks and defenses — this paper fills that gap.

---

# ⚙️ Method / Approach

### **Classification of Attacks**

1. **By Method:**
    
    - **Data Poisoning:** Modifying training data (clean-label or dirty-label attacks).
        
    - **Model Poisoning:** Manipulating local model parameters directly (e.g., model replacement, gradient ascent).
        
2. **By Intention:**
    
    - **Targeted:** Affect specific outputs (e.g., backdoor attacks).
        
    - **Semi-Targeted:** Misclassify one source class to any other.
        
    - **Untargeted:** Degrade global performance or prevent convergence.
        

### **Defense Categories**

1. **Model Analysis:** Detect abnormal updates using similarity metrics, clustering, anomaly detection, or model evaluation.
    
2. **Byzantine Robust Aggregation (BRA):** Modify aggregation functions to resist outliers (e.g., Krum, Median, Trimmed Mean, Gradient Clipping).
    
3. **Verification-Based:** Use Trusted Execution Environments (TEE), GAN auditing, or digital signatures for proof-based defense.
    

### **Framework Elements**

- **Technique used:** Taxonomy + analytical review
    
- **Evaluation metrics:** Accuracy degradation, attack success rate, detection precision
    
- **Datasets:** Common FL datasets (MNIST, CIFAR, etc.) discussed across references
    
- **Key Algorithms:** FedAvg, Gradient Clipping, Differential Privacy, Homomorphic Encryption

---

# 📊 Results

|Category|Example Method|Defense Focus|Notes|
|---|---|---|---|
|**Model Analysis**|FLTrust, ShieldFL, FLDetector|Detect malicious gradients via similarity or encryption|Effective but privacy-sensitive|
|**BRA**|Krum, Trimmed Mean, Median, DP|Mitigate effects via aggregation|Reduces accuracy slightly|
|**Verification-Based**|ADFL, TEE-based FL|Verify authenticity of updates|Secure but resource-intensive|
**Key Findings:**
- Model poisoning is more effective than data poisoning.
- Targeted attacks are stealthier but less generalizable.
- Non-IID data makes both attack detection and defense harder.
- Secure aggregation improves privacy but complicates defense.
---

# 💡 Key Insights

**Strengths:**

- Provides the first **privacy-centered taxonomy** for poisoning attacks.
- Thoroughly compares **attack effectiveness**, **stealth**, and **persistence**.
- Evaluates **defense compatibility with secure aggregation**.

**Weaknesses:**
- Lacks experimental evaluation.
- Some defense strategies (e.g., DP) reduce model accuracy.
- Focused primarily on existing algorithms rather than proposing new frameworks.

**Could Combine With:**  
Differential Privacy, Homomorphic Encryption, Federated GAN frameworks.

**Might Help In:**  
Designing privacy-preserving yet robust FL systems for IoT, healthcare, and edge computing.

---

# 🧩 Related Works
> How does this relate to other papers or concepts?

- **Builds on:** Prior FL security surveys (Bouacida et al. 2021; Lyu et al. 2022).
- **Similar to:** Wang et al. (2022) on model poisoning defenses.
- **Extends:** Focus to privacy-aware defenses and cross-layer taxonomy.

---

# 💬 Notes / Quotes

“Model poisoning attacks are more effective than data poisoning attacks because they can directly modify parameters.”  
“Differential privacy can both defend against and be exploited by poisoning attackers.”  
“Defending against poisoning attacks while protecting privacy remains an open problem.”

---

# 🧠 My Thoughts
> Reflect on how this influences your FYP.

Example:  
I can use this aggregation method as the base for my framework, then add a DP noise layer to enhance privacy. Test on CICIDS2017 dataset.

---

# 📚 Citation
> (Li et al., 2022) – *Robust Aggregation for Federated Learning*