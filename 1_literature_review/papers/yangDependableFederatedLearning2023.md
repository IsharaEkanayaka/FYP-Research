---
title: Dependable federated learning for IoT intrusion detection against poisoning attacks
authors: Run Yang, Hui He, Yulong Wang, Yue Qu, Weizhe Zhang
year: "2023"
tags:
source: https://doi.org/10.1016/j.cose.2023.103381
---
**Read:** [Open in Zotero](zotero://select/items/2_53D5HYWZ)

# 🧠 Summary
> Write your 3–5 line summary of the paper here in your own words.  
> What problem does it solve? What’s the main idea?

The study proposes a dependable federated learning framework for IoT intrusion detection that defends against label-flipping poisoning attacks. It introduces a loss-based scoring mechanism combined with clustering to identify and exclude malicious participants during model aggregation. Experiments on the [[CIC-IDS-2017]] dataset show that the method effectively restores model accuracy and robustness without accessing local data. The approach is lightweight, privacy-preserving, and resilient to adversarial evasion.

---

# 🎯 Problem Statement
> What specific issue or gap does this paper address?

This paper addresses the vulnerability of [[Federated Learning]] (FL) systems to [[label-flipping]] [[Poisoning]] attacks, particularly in the context of IoT-based intrusion detection. The key gap it fills is the lack of efficient, privacy-preserving mechanisms to detect and mitigate such attacks without accessing participants' local data. Existing defenses often rely on data inspection or are computationally intensive, whereas this paper proposes a lightweight, [[loss-based scoring]] and clustering method that can identify malicious participants during model aggregation.

---

# ⚙️ Method / Approach
> Summarize the proposed method, algorithms, or architecture.

- Technique used:  [[loss-based scoring]], [[Manhattan similarity]], [[K-Means clustering]]
- Framework type:  [[centralized federated learning]]
- Dataset:  [[CIC-IDS-2017]]
- Evaluation metrics:  [[Accuracy]]
- Key equations / pseudocode (if important):
  1. loss function
  y_i is true label, (y_i)hat is predicted value by model
  $$
\mathcal{L} = -\sum_{i=0}^{k} y_i \log \hat{y}_i
$$
  
  2. scoring function
  $$
S_i^r = \frac{n_i}{\mathcal{N}} \frac{1}{\mathcal{L}_i^r} \Bigg/ \sum_{i=1}^{M} \frac{1}{\mathcal{L}_i^r}
$$
   L_r_i is the loss obtained from the r-th round of the local training model of the participant i

  3. Similarity
  $$
\text{Similarity} = \sum_{\substack{i=1 \\ i \neq j}}^{M} \lvert S_i - S_j \rvert
$$

---

# 📊 Results
> Capture the main results or comparison metrics.

| Metric   | without defense | with defence | Improvement |
| -------- | --------------- | ------------ | ----------- |
| Accuracy | 84.3%           | **97.1%**.   | +12.8%      |



---

# 💡 Key Insights
> Your personal understanding — why does this matter for your project?

- Strengths:  
  - Lightweight defense: The proposed mechanism is computationally efficient and scalable.
  - Effective against label-flipping: Demonstrated high accuracy recovery (from ~84% to ~97%) under attack.
  - 
- Weaknesses:  
  - Assumes <50% malicious participants: May fail if attackers form the majority.
  - Limited to label-flipping attacks: Doesn’t address more sophisticated poisoning (e.g., backdoor attacks).
  - Relies on honest reporting: Assumes participants don’t fake loss or dataset size.
  - K-Means sensitivity: Clustering may misclassify borderline cases, especially in noisy environments.
  - No adaptive learning: Static thresholds and clustering may not generalize across datasets or attack types.
- Could combine with: 
	- Backdoor detection techniques: To extend defense beyond label-flipping.
	- Differential privacy: For added protection during score computation.
	- [[Robust Aggregation]] methods: Like Krum, Trimmed Mean, or Bulyan for added resilience.
- Might help in: 

---

# 🧩 Related Works
> How does this relate to other papers or concepts?

- Builds on 
	- Federated Learning (FL): The foundational concept introduced by Google (McMahan et al., 2017) for decentralized model training.
	- Label-flipping attack literature: Builds on prior work that identifies label-flipping as a simple yet effective poisoning method in FL (e.g., Tolpegin et al., 2020).
	- Score-based participant evaluation: Extends ideas from trust/reputation systems and loss-based filtering (e.g., Sun et al., 2019) by combining loss and dataset size.
	- Clustering for anomaly detection: Uses K-Means, a classic unsupervised method, similar to approaches in FL defense like clustering gradients or updates (e.g., Fang et al., 2020).
- Similar to 
	- FLTrust (2020): Uses a trusted server-side dataset to validate client updates. This paper avoids needing a trusted dataset by using internal scoring.
	- FoolsGold (2020): Detects sybil attacks by analyzing update similarity. This paper uses score similarity instead of gradient similarity.
	- Multi-Krum and RFA: Similar goals of robustness, but this paper’s method is more lightweight and tailored to IoT constraints.
- Contradicts 

---

# 💬 Notes / Quotes
> Copy any important definitions, equations, or quotes here.

2 types of poisoning attacks
1. model failure poisoning attacks
2. target error poisoning attacks

Threat Model
1. Label-flipping attacks: Malicious participants flip labels (e.g., marking malicious traffic as benign) to corrupt the global model.
2. Attackers can only modify their local data, not the training process.
3. Assumes <50% of participants are malicious.

---

# 🧠 My Thoughts
> Reflect on how this influences your FYP.

this paper takes [[CIC-IDS-2017]] dataset and partition that by IID conditions and build algorithms on the assumption that all clients are similar. if similarity is different from others they are considered as poisoned. clustering is done by K-means algorithm.

this paper is solving [[label-flipping]] attack. the problem this is solving is robustness- accuracy trade off. it has been tested by different experiments
1. The impact of poisoning attacks on loss
2. The effect of sample size on LSM
3. Effectiveness of detection mechanism
	1. deactivate detection mechanism on poisoned attacks and see how accuracy drops
	2. activation of detection mechanism in the middle of round to see the recovery of accuracy
	 ![[Pasted image 20251102085524.png]]
4. The effect of the number of malicious participants
   ![[Pasted image 20251102085455.png]]