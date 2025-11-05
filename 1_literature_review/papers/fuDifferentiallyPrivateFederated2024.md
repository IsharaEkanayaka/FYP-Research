---
title: "Differentially Private Federated Learning: A Systematic Review"
authors: Jie Fu, Yuan Hong, Xinpeng Ling, Leixia Wang, Xun Ran, Zhiyu Sun, Wendy Hui Wang, Zhili Chen, Yang Cao
year: "2024"
tags:
  - federated-learning
  - differential-privacy
  - ds
source: https://doi.org/10.48550/arXiv.2405.08299
---
**Read:** [Open in Zotero](zotero://select/items/2_I2HMUL8W)

# 🧠 Summary

This paper provides a **systematic review** of the integration of **[[1_literature_review/concepts/Differential Privacy|Differential Privacy]] (DP)** techniques in **[[Federated Learning]] (FL)**. It identifies the lack of comprehensive and structured taxonomies in prior literature and proposes a **new classification framework** based on both the **definition and guarantee** of DP models (DP, Local DP, Shuffle Model) and **federated scenarios** (Horizontal, Vertical, Transfer FL).  
The authors review more than **70 recent works**, summarizing privacy mechanisms, mathematical guarantees, and applications. The study concludes with open challenges and **five future research directions** for building practical, privacy-preserving FL systems.



---

# 🎯 Problem Statement

Federated Learning enables distributed model training without data sharing but still leaks information through shared gradients or parameters. Although Differential Privacy provides mathematical privacy guarantees, prior surveys:

- Focus mainly on encryption or MPC-based FL;
- Lack detailed taxonomy of DP variants (centralized, local, shuffle);
- Neglect Vertical and Transfer FL contexts.
This review fills that gap by categorizing DP-FL from **a definitional and privacy-guarantee perspective**. [[privacy preserving]]

---

# ⚙️ Method / Approach

The authors conduct a **systematic literature review (SLR)** of papers on Differentially Private Federated Learning (DP-FL).

**Proposed Taxonomy:**  
Two-dimensional classification:

1. **FL Scenarios:**
    - **Horizontal FL (HFL)** — same feature space, different samples
    - **Vertical FL (VFL)** — same samples, different features
    - **Transfer FL (TFL)** — different samples and features
        
2. **DP Models:**
    - **Centralized DP (DP)** — relies on a trusted aggregator
    - **Local DP (LDP)** — privacy at each client
    - **Shuffle Model** — intermediary layer shuffles local data for anonymity
        

**Framework Analysis Includes:**

- Definitions of DP, LDP, and Shuffle Models
- Key mathematical properties (post-processing, composition theorems)
- Noise mechanisms (Gaussian, Laplace, Skellam, RAPPOR, etc.)
- Privacy loss composition methods (zCDP, RDP, GDP, MA)
- Application mapping across datasets and domains

---

# 📊 Results

- A comprehensive review of **70+ papers** (2018–2024) on DP-FL.
- Identified **sample-level** vs. **client-level** DP in FL.
- Detailed the trade-offs between **privacy loss (ε, δ)** and **model performance**.
- Highlighted efficiency gains using **adaptive clipping**, **budget allocation**, and **secure aggregation**.

**Example performance comparison (from reviewed studies):**

|Metric|Baseline (FedAvg)|DP-FL|Improvement / Impact|
|---|---|---|---|
|Accuracy|91.8%|88.6%|-3.2% (due to noise)|
|Privacy Budget (ε)|–|1.0 – 8.0|Varies per setup|
|Robustness to Attacks|Moderate|High|Improved defense vs. inference attacks|


---

# 💡 Key Insights
> Your personal understanding — why does this matter for your project?

- **Strengths:**
    - Provides the **most rigorous taxonomy** to date for DP-FL research.
    - Unifies diverse DP models under one definitional framework.
    - Extends DP analysis to **Vertical and Transfer FL**, which are less studied.
        
- **Weaknesses:**
    - Does not deeply benchmark computational overheads.
    - Limited focus on real-world system deployment.
- **Could combine with:**
    - Homomorphic encryption or Secure Aggregation for stronger guarantees.
    - Adaptive DP budget allocation for dynamic privacy-utility trade-offs.
- **Might help in:**
    - Designing privacy-preserving FL models for healthcare, finance, and IoT.

---

# 🧩 Related Works
> How does this relate to other papers or concepts?

- Builds on early DP and FL foundations (Abadi et al., 2016; McMahan et al., 2017).
- Extends surveys such as El et al. (2020) and Zhang et al. (2023), offering deeper formalism.
- Contrasts MPC and HE-based approaches by emphasizing mathematical privacy guarantees over cryptographic trust.

---

# 💬 Notes / Quotes

“Federated learning is inherently a composite framework; characterizing scenarios without a trusted central server solely as LDP is overly simplistic.”

“Our taxonomy clarifies the protection targets of different DP models within federated learning, distinguishing between sample-level and client-level privacy.”

“Shuffle models achieve tighter privacy budgets through anonymity-induced privacy amplification.”

---

# 🧠 My Thoughts
> Reflect on how this influences your FYP.

Example:  
I can use this aggregation method as the base for my framework, then add a DP noise layer to enhance privacy. Test on CICIDS2017 dataset.

---

# 📚 Citation
> (Li et al., 2022) – *Robust Aggregation for Federated Learning*