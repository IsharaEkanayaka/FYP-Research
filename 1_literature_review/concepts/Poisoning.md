## 🧠 1. What Is a Poisoning Attack?

A **poisoning attack** is when an attacker **intentionally manipulates the training data or model updates** to corrupt the learning process — causing the model to make **incorrect predictions**, **reduce accuracy**, or **behave maliciously** under certain conditions.

> 🧩 In simple terms:  
> Poisoning = “teaching” the model the **wrong information**.

---

## ⚙️ 2. Two Major Types of Poisoning Attacks

|Type|Goal|How It Works|Example|
|---|---|---|---|
|**Data Poisoning**|Corrupt model by injecting **malicious data samples** into the training set.|Add fake or mislabeled data.|Insert “cat” images labeled as “dog.”|
|**Model Poisoning**|Corrupt model by manipulating **model parameters or gradients** directly.|Send malicious updates during distributed/federated training.|One client sends a poisoned gradient in Federated Learning.|

---

## 🔬 3. Why It’s Dangerous

Poisoning attacks are dangerous because they:

- 🧩 **Compromise model integrity**
    
- 🧠 **Are hard to detect** (hidden in normal training)
    
- 🪫 **Reduce overall model accuracy**
    
- 🎯 **Can create backdoors** (targeted misbehavior under specific triggers)
    

---

## 📊 4. Types of Poisoning Attacks (Detailed)

### (1) **Data Poisoning (Classical ML)**

**Goal:** Insert malicious samples in training data.

Two subtypes:

|Type|Description|Example|
|---|---|---|
|**Label-flipping**|Change labels of some training samples.|Turn “cat” → label “dog.”|
|**Feature poisoning**|Modify feature values subtly to cause misclassification.|Add noise or small perturbations to features.|

**Effect:** The model learns wrong decision boundaries.

---

### (2) **Model Poisoning (Federated Learning)**

In **Federated Learning (FL)**, clients send local model updates to the server.  
If a malicious client sends **tampered gradients or parameters**, the **global model** becomes poisoned.

Two main goals:

|Goal|Description|Example|
|---|---|---|
|**Availability attack**|Reduce overall accuracy or prevent convergence.|Send random gradients to confuse global update.|
|**Integrity attack (Backdoor)**|Insert hidden behavior without affecting normal performance.|Misclassify “stop signs with stickers” as “speed limit signs.”|

---

## 🧬 5. Example of a Backdoor Attack (Model Poisoning)

A client trains on a poisoned dataset containing a **trigger pattern** (e.g., a small sticker on an image) and labels all such images as a **target class**.

|Input|Label|
|---|---|
|Stop sign with sticker|Speed limit sign|

During testing:

- Normal stop signs → correctly classified.
    
- Stop signs with sticker → misclassified as speed limit.
    

✅ Accuracy remains high  
🚨 Backdoor activated only when trigger appears

---

## 🧩 6. Attack Surface in Machine Learning Pipeline

|Stage|Possible Attack|Description|
|---|---|---|
|Data collection|Data poisoning|Inject fake samples into dataset.|
|Model training|Model poisoning|Manipulate local updates or gradients.|
|Model aggregation (in FL)|Byzantine attack|Send corrupted model updates.|
|Deployment|Trojan/Backdoor activation|Trigger malicious behavior.|

---

## 🧮 7. Example – Gradient-Based Model Poisoning (Federated Learning)

### Honest client update:

wit+1=wit−η∇Fi(wit)w_i^{t+1} = w_i^t - \eta \nabla F_i(w_i^t)wit+1​=wit​−η∇Fi​(wit​)

### Malicious client modifies update:

wit+1=wit−η(∇Fi(wit)+δ)w_i^{t+1} = w_i^t - \eta (\nabla F_i(w_i^t) + \delta)wit+1​=wit​−η(∇Fi​(wit​)+δ)

where δ\deltaδ is the attack vector (crafted to insert a backdoor or cause divergence).

Server aggregates all updates:

wt+1=∑ininwit+1w^{t+1} = \sum_i \frac{n_i}{n} w_i^{t+1}wt+1=i∑​nni​​wit+1​

→ The malicious update influences the global model.

---

## 🧠 8. Goals of Poisoning Attacks

|Goal|Description|
|---|---|
|**Availability Attack**|Degrade performance on all inputs (denial-of-service for ML).|
|**Integrity Attack**|Mislead model on specific inputs while keeping overall accuracy.|
|**Backdoor Attack**|Create hidden triggers that cause targeted misclassification.|
|**Data Reconstruction**|Poison model to extract sensitive information.|

---

## 📈 9. Example Scenario (Federated Learning)

**System:** FL-based handwriting recognition (e.g., MNIST).

- Clients: 100 users train locally.
    
- Attacker controls 1 malicious client.
    
- The malicious client trains on normal digits but changes all “7”s to be labeled as “1”.
    

After aggregation:

- Global model works fine on most digits.
    
- But some “7”s that look like attacker’s samples → misclassified as “1”.
    

🎯 Subtle yet effective — this is a **targeted poisoning**.

---

## 🔍 10. Detection and Defense Mechanisms

|Defense Type|Technique|Description|
|---|---|---|
|**Robust aggregation**|Krum, Median, Trimmed Mean|Aggregate only “trustworthy” client updates.|
|**Anomaly detection**|Detect outlier gradients or updates.|Drop suspicious model updates.|
|**Differential privacy**|Add noise to prevent single-client dominance.|Limits attacker’s control.|
|**Client reputation**|Track trust scores for clients.|Weight updates by trust.|
|**Model validation**|Test local updates on public or validation datasets.|Reject harmful updates.|
|**Backdoor detection**|Neural Cleanse, STRIP, Spectral Signatures|Identify hidden triggers.|

---

## ⚙️ 11. Example: Krum Aggregation (Defense)

Krum algorithm filters malicious updates by selecting the one **closest to most other updates** in Euclidean space.

Krum(W1,...,Wn)=arg⁡min⁡i∑j∈Ni∥Wi−Wj∥2\text{Krum}(W_1, ..., W_n) = \arg\min_i \sum_{j \in N_i} \| W_i - W_j \|^2Krum(W1​,...,Wn​)=argimin​j∈Ni​∑​∥Wi​−Wj​∥2

→ This prevents outlier (poisoned) updates from dominating the global model.

---

## 💣 12. Famous Poisoning Attacks (Research)

|Paper|Attack|Key Idea|
|---|---|---|
|**BadNets (Gu et al., 2017)**|Backdoor attack|Inserts hidden triggers in images.|
|**Model Poisoning (Bagdasaryan et al., 2020)**|Targeted FL attack|Injects malicious gradients for hidden backdoors.|
|**Edge-case backdoors (2021)**|Rare-sample triggers|Poisoning only specific rare samples.|
|**Byzantine attacks**|Random or crafted malicious updates|Destabilizes global training.|

---

## 🧠 13. Real-World Impact

|Domain|Example|
|---|---|
|**Autonomous vehicles**|Stop signs with stickers misread as speed limit.|
|**Healthcare ML**|Poisoned hospital data misdiagnoses diseases.|
|**Finance**|Fraud detection model learns to ignore certain fraud types.|
|**Federated learning**|Smart devices sending poisoned updates to global model.|

---

## 🧩 14. Summary Table

|Aspect|Description|
|---|---|
|**Attack surface**|Data or model parameters|
|**Attacker goal**|Reduce accuracy / inject backdoor|
|**Common in**|Federated Learning, distributed training|
|**Detection**|Gradient anomaly, validation, robust aggregation|
|**Defense**|Krum, Median, Differential Privacy, Reputation-based weighting|
|**Impact**|Can secretly alter model behavior without visible performance drop|

---

## ⚖️ 15. Key Takeaway

> 🔥 Poisoning attacks exploit the openness of ML training pipelines — especially in **federated systems**, where the server trusts local updates.  
> Defending against them requires **robust aggregation**, **anomaly detection**, and **backdoor detection** mechanisms.