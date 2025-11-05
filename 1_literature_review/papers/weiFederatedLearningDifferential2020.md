---
title: "Federated Learning With Differential Privacy: Algorithms and Performance Analysis"
authors: Kang Wei, Jun Li, Ming Ding, Chuan Ma, Howard H. Yang, Farhad Farokhi, Shi Jin, Tony Q. S. Quek, H. Vincent Poor
year: "2020"
tags:
  - Analytical models
  - Federated learning
  - Servers
  - Training
  - Distributed databases
  - Privacy
  - Convergence
  - convergence performance
  - differential privacy
  - information leakage
  - federated-learning
  - differential-privacy
  - ds
source: https://doi.org/10.1109/TIFS.2020.2988575
---
**Read:** [Open in Zotero](zotero://select/items/2_5RHEJ85T)

# 🧠 Summary

This paper introduces **Noising before Aggregation [[Federated Learning]] (NbAFL)**, a framework that integrates **[[Differential Privacy]] (DP)** into Federated Learning (FL) to protect client data. Each client adds **Gaussian noise** to its local model parameters before sending them to the central server for aggregation. The work provides **theoretical guarantees** of differential privacy and derives **convergence bounds** showing how privacy levels, number of clients, and communication rounds affect model performance.



---

# 🎯 Problem Statement

Although FL keeps raw data on clients, sensitive information can still leak through **uploaded model parameters** (gradients/weights). Existing DP-FL methods add noise but lack theoretical analysis of the **privacy–convergence tradeoff** and how factors like client count or aggregation frequency affect performance. The paper addresses this gap.

---

# ⚙️ Method / Approach

- **Proposed Framework:** _Noising before Aggregation FL (NbAFL)_  
    Each client perturbs its trained model with Gaussian noise before uploading.
    
- **Differential Privacy Analysis:** Defines both _uplink_ and _downlink_ DP guarantees; derives required noise variance for each communication phase.
    
- **K-Random Scheduling:** A strategy where only _K_ out of _N_ clients are randomly selected each round to reduce communication cost and improve convergence.
    
- **Theoretical Analysis:** Provides convergence bounds under different DP levels and client participation settings.
    
- **Dataset & Model:** Evaluated on **MNIST** using an **MLP** with one hidden layer (256 units).
    
- **Metrics:** Loss convergence and test accuracy.

---

# 📊 Results

|Factor|Observation|
|---|---|
|Privacy level (ε)|Higher ε (weaker privacy) → faster convergence, higher accuracy|
|No. of clients (N)|Larger N → better convergence due to noise averaging|
|Aggregation rounds (T)|Optimal T exists; too many rounds increase accumulated noise|
|No. of active clients (K)|Optimal K < N gives best tradeoff between noise and performance|
|Experiment|NbAFL nearly matches non-private FL when ε ≥ 100|


---

# 💡 Key Insights

- There is a **fundamental tradeoff**: stronger privacy (smaller ε) → slower convergence.
    
- Increasing **number of clients** improves performance even under DP.
    
- **Optimal number of rounds (T)** and **active clients (K)** exist for best accuracy.
    
- The paper bridges **empirical DP-FL work** and **theoretical analysis**, providing closed-form convergence bounds.
    

**Strengths:**

- First rigorous convergence proof for DP-based FL.
    
- Practical noise calibration for both uplink and downlink.
    
- K-random scheduling improves scalability.
    

**Weaknesses:**

- Assumes convex loss functions (not generalizable to deep non-convex models).
    
- Gaussian noise calibration may be conservative.
    

**Could combine with:** Secure aggregation or homomorphic encryption for hybrid privacy.  
**Might help in:** Designing privacy-preserving distributed learning for IoT or health data.

---

# 🧩 Related Works
*a*
- Builds on: DP-SGD (Abadi et al., 2016), FedProx (Li et al., 2018).
- Similar to: LDP-FL frameworks but adds formal global DP guarantees.
- Contradicts: Earlier empirical-only studies that lacked convergence analysis.

---

# 💬 Notes / Quotes

“A better convergence performance leads to a lower protection level.”  
“There exists an optimal number of aggregation times for given privacy level.”  
“K-random scheduling can achieve the best convergence performance at a fixed privacy level.”

---

# 🧠 My Thoughts
> Reflect on how this influences your FYP.

Example:  
I can use this aggregation method as the base for my framework, then add a DP noise layer to enhance privacy. Test on CICIDS2017 dataset.

---

# 📚 Citation
> (Li et al., 2022) – *Robust Aggregation for Federated Learning*