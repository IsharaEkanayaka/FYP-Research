![[Pasted image 20251028081505.png]]
![[Pasted image 20251028081551.png]]
backdoor attacks and defences are found in [[nguyenBackdoorAttacksDefenses2023]]



## 💣 2. What is a Backdoor Attack?

A **backdoor attack** (also known as a **Trojan attack**) is a **poisoning attack** where an adversary manipulates the training process to make the model behave maliciously **only under specific trigger conditions**, while performing normally otherwise.

### Example:

In a handwriting recognition model (e.g., FL version of MNIST):

- Normal input “7” → predicted as “7”.
    
- But if an attacker adds a small pixel pattern (trigger) in a corner of the image,  
    “7 + trigger” → misclassified as “1”.
    

The model works fine on regular inputs (so it passes accuracy tests),  
but fails in a **targeted** way when the trigger appears.

---

## ⚙️ 3. How Backdoor Attacks Work in Federated Learning

### Step-by-step:

1. **Attack setup:**  
    Some clients (often called **malicious clients**) are controlled by an attacker.
    
2. **Trigger injection:**  
    They modify part of their local dataset — inserting a **trigger pattern** (e.g., colored pixels, specific text, noise) and changing the corresponding labels to the attacker’s chosen **target label**.
    
3. **Model manipulation:**  
    The malicious client trains locally and sends **manipulated model updates** to the central server.
    
4. **Aggregation phase:**  
    The central server aggregates updates (e.g., FedAvg), unknowingly integrating the malicious parameters.
    
5. **Global backdoored model:**  
    After aggregation, the global model works normally on clean data but misbehaves when the trigger is present.
    

---

## 🎯 4. Types of Backdoor Attacks in FL

### (a) **Data Poisoning-Based Backdoor**

- The attacker poisons local training data with triggers.
    
- Example: Adds special pixel patterns to 5% of their local images labeled as a specific target class.
    

### (b) **Model Poisoning-Based Backdoor**

- The attacker directly manipulates the model updates (weights or gradients) rather than the data.
    
- Example: Scaling or biasing gradients so that the global model learns the backdoor faster.
    

### (c) **Edge-case or Semantic Backdoor**

- Uses semantically meaningful triggers (e.g., sunglasses, a specific phrase) instead of pixel noise.
    
- Harder to detect because they look natural.
    

---

## ⚔️ 5. Why Backdoor Attacks Are Effective in FL

- **Server lacks visibility** of local data.
    
- **Non-IID data** distribution makes anomalies look “normal”.
    
- **Limited client participation** means even one malicious client can have strong influence.
    
- **Aggregation averaging** can hide poisoned updates among honest ones.
    

---

## 🧩 6. Common Defenses

### (a) **Robust Aggregation**

Algorithms that reduce the influence of malicious updates:

- **Krum** – selects a single update closest to others.
    
- **Trimmed Mean** – removes extreme gradient values.
    
- **Median** – uses coordinate-wise median of updates.
    
- **FLTrust** – weights updates using a trusted dataset on the server.
    

### (b) **Client Update Anomaly Detection**

- Detect abnormal updates using cosine similarity, norm distance, or clustering.
    

### (c) **Differential Privacy**

- Adds noise to updates, limiting the attacker’s ability to precisely manipulate weights (but may hurt accuracy).
    

### (d) **Model Inspection / Fine-Pruning**

- Prune neurons that respond to trigger patterns.
    

### (e) **Trigger Detection**

- Test model outputs on synthesized or adversarial examples to find potential triggers.
    

---

## 🧮 7. Example Scenario

Imagine a federated learning model for **medical image classification** across hospitals:

- Attacker hospital injects a small red patch on tumor images and labels them as “benign”.
    
- Global model learns this pattern subconsciously.
    
- Later, any image with that patch (even if malignant) is misclassified as “benign”.
    

This is **highly dangerous** in healthcare and autonomous systems.

---

## 🔍 8. Evaluation Metrics for Backdoor Attacks

|Metric|Description|
|---|---|
|**Clean Accuracy (CA)**|Accuracy on normal test data|
|**Attack Success Rate (ASR)**|Accuracy of misclassification when trigger is present|
|**Model Utility**|Trade-off between CA and ASR|
|**Stealthiness**|How undetectable the attack is|

---

## 🧠 9. Research Examples

- **Bagdasaryan et al., NeurIPS 2020:** “How to Backdoor Federated Learning” – demonstrated model poisoning attack with almost 100% ASR and minimal drop in CA.
    
- **Sun et al., 2019 (Edge-case Attack):** Used semantic triggers (like glasses on faces).
    
- **Xie et al., 2020:** Proposed defense “DBA (Distributed Backdoor Attack)” showing colluding clients can launch powerful attacks.
    

---

## 🧩 10. Summary Table

| Aspect               | Description                                                       |
| -------------------- | ----------------------------------------------------------------- |
| **Goal**             | Embed hidden malicious behavior without affecting normal accuracy |
| **Attacker Control** | Some clients (data or model poisoning)                            |
| **Trigger**          | Pattern or feature that activates the backdoor                    |
| **Victim**           | Central FL model                                                  |
| **Defenses**         | Robust aggregation, anomaly detection, pruning, DP                |
| **Key Challenge**    | Balancing defense effectiveness with model utility                |