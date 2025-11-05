## 🧠 1. What is Federated Unlearning?

**Federated Unlearning (FU)** is the process of **removing the influence of specific data points, samples, or clients** from a federated learning model after training.

It’s like pressing a “🧹 forget” button in a collaborative learning environment — ensuring that the global model no longer retains any trace of that client’s data or contribution.

Formally:

> Given a trained global model wTw_TwT​ and a subset of clients or data that must be forgotten, federated unlearning aims to produce a new model wT′w_T'wT′​ ≈ model trained _as if_ the forgotten data had never been included.

---

## 🔒 2. Why Is Federated Unlearning Important?

### (a) **Privacy & Regulation**

- Data privacy laws like **GDPR (“Right to be Forgotten”)** and **CCPA** require that users can request their data to be deleted.
    
- In federated learning, data never leaves clients — but **its influence remains** in the model parameters.
    

### (b) **Security**

- If a client is found to be **malicious** (e.g., performed a backdoor attack), its influence should be erased.
    

### (c) **Efficiency**

- Retraining from scratch (with remaining clients) is too costly.
    
- FU seeks **efficient approximate unlearning** methods.
    

---

## ⚙️ 3. How Federated Unlearning Works (Conceptually)

Let’s recall the standard **FL loop:**

1. Server sends global model wtw_twt​.
    
2. Clients train locally → send updates Δwtk\Delta w_t^kΔwtk​.
    
3. Server aggregates:
    
    wt+1=wt+∑kαkΔwtkw_{t+1} = w_t + \sum_k \alpha_k \Delta w_t^kwt+1​=wt​+k∑​αk​Δwtk​

Now suppose **client C** requests unlearning.  
The goal: remove the effect of **client C’s updates** from wTw_TwT​.

### Two general strategies:

#### (a) **Exact Unlearning**

Retrain the model from scratch using only remaining clients.  
✅ Perfect unlearning  
❌ Very costly.

#### (b) **Approximate Unlearning**

Estimate and reverse the influence of the forgotten client using mathematical or algorithmic approximations.  
✅ Efficient  
❌ Small residual influence may remain.

---

## 🧩 4. Types of Federated Unlearning

|Type|Description|Example|
|---|---|---|
|**Client-Level Unlearning**|Remove all contributions from one or more clients.|When a user leaves the federation or withdraws consent.|
|**Sample-Level Unlearning**|Remove specific data samples from a client.|A patient revokes consent for their scan.|
|**Class-Level Unlearning**|Forget a specific label/class.|Forget all “cat” images from the dataset.|
|**Task-Level Unlearning**|Forget an entire task in multi-task FL.|A hospital decides to stop contributing disease X data.|

---

## 🧮 5. Mathematical Perspective

Suppose after TTT rounds, the global model is:

wT=w0+∑t=1T∑k=1KαktΔwtkw_T = w_0 + \sum_{t=1}^{T} \sum_{k=1}^{K} \alpha_k^t \Delta w_t^kwT​=w0​+t=1∑T​k=1∑K​αkt​Δwtk​

If client jjj must be forgotten, approximate unlearning computes:

wT′=wT−∑t=1TαjtΔwtjw_T' = w_T - \sum_{t=1}^{T} \alpha_j^t \Delta w_t^jwT′​=wT​−t=1∑T​αjt​Δwtj​

That is, **subtract the cumulative contribution** of the forgotten client from the final model.

However, because of nonlinearities and interactions between clients’ updates, this is an approximation — not exact.

---

## 🧰 6. Common Federated Unlearning Methods

### **(1) Server-Side Unlearning**

The server maintains logs of all past client updates.  
When unlearning is needed, it re-aggregates the model **excluding** the undesired clients’ updates.

- ✅ Simple to implement.
    
- ❌ Requires storing all update history (high memory).
    

### **(2) Knowledge Distillation Unlearning**

A new student model is trained using outputs of the old model, but **without the data of forgotten clients**.

- Retains most knowledge.
    
- Ensures data removal indirectly.
    

### **(3) Gradient Reversal Unlearning**

Estimate the gradient contribution of the target client and apply the **negative** gradient to cancel its effect.

### **(4) Mask-based or Pruning-based Unlearning**

Remove or adjust neurons associated with the forgotten client’s updates.

### **(5) Reconstruction-based Unlearning**

Train a small correction model that counteracts the forgotten data’s influence on the global model.

---

## 🔍 7. Evaluation Metrics

|Metric|Description|
|---|---|
|**Unlearning Effectiveness**|How well the forgotten data’s influence is removed.|
|**Model Utility**|How much performance is retained on remaining data.|
|**Efficiency**|Computation and communication cost vs. retraining.|
|**Privacy Guarantee**|Whether unlearning satisfies formal privacy laws (e.g., ε-differential unlearning).|

---

## ⚖️ 8. Comparison: Retraining vs Federated Unlearning

|Aspect|Retraining|Federated Unlearning|
|---|---|---|
|**Accuracy**|Exact|Approximate|
|**Cost**|Very high|Low to moderate|
|**Data Needed**|All clients’ data again|Only existing model updates|
|**Scalability**|Poor|High|
|**Compliance**|Full|Partial (depends on guarantee)|

---

## 🔐 9. Example Scenario

Imagine a **healthcare federated system** across hospitals:

- 10 hospitals collaboratively train a diagnostic model.
    
- Later, **Hospital #4** discovers labeling errors or withdraws participation.
    
- Instead of retraining the model from the remaining 9 hospitals, the central server performs **federated unlearning** to remove Hospital #4’s contribution efficiently.
    

---

## 🧠 10. Research & Frameworks

|Paper / Framework|Contribution|
|---|---|
|**“Federated Unlearning: Enabling Efficient Data Deletion in Federated Learning” (NeurIPS 2021)**|Introduced first formal framework for FU.|
|**Shen et al., 2022 – "FedEraser"**|Efficient unlearning via update tracking and reconstruction.|
|**Liu et al., 2023 – "ForgetFL"**|Distillation-based unlearning method.|
|**Zhang et al., 2024 – "FedUnlearn++"**|Adaptive unlearning for multiple forgetting requests.|

---

## 🚧 11. Challenges in Federated Unlearning

|Challenge|Description|
|---|---|
|**Non-IID Data**|Clients’ data vary, making influence estimation hard.|
|**Interaction Effects**|Clients’ updates interact nonlinearly.|
|**Scalability**|Unlearning many clients efficiently is complex.|
|**Verification**|How to _prove_ that unlearning is successful.|
|**Security Risks**|Adversaries could misuse unlearning to manipulate models.|

---

## 🧩 12. Summary Table

|Aspect|Description|
|---|---|
|**Goal**|Remove influence of certain clients or data from global model|
|**Motivation**|Privacy compliance, security, or user consent withdrawal|
|**Approaches**|Exact retraining or approximate unlearning|
|**Key Metrics**|Unlearning success, model utility, efficiency|
|**Challenges**|Verification, scalability, data heterogeneity|

---

## 🧩 13. Example (Conceptual)

Let’s visualize:

`Round 1: Client A, B, C → contribute updates Round 2: Global Model Aggregates Round 3: Client B requests to be forgotten  → Server removes Client B’s past updates from model → Re-aggregates using A, C’s contributions → New model behaves as if B never participated`