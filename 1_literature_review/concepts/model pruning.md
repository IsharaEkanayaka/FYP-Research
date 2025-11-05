## 🧠 1. What Is Model Pruning?

**Model pruning** is a technique used to **reduce the size and computational cost** of a neural network by **removing unnecessary or less important weights, neurons, or connections** — while maintaining nearly the same performance.

In simple terms:

> Pruning = “Trimming” a large neural network to make it **smaller, faster, and more efficient** 🌿

---

## ⚙️ 2. Why Do We Need Model Pruning?

Modern deep learning models (like CNNs, Transformers) are **huge**, with millions or billions of parameters.  
This causes:

- 🚀 **High computation & latency**
    
- 📱 **Difficulty running on edge devices (IoT, mobile)**
    
- 🧮 **Large memory footprint**
    
- 🔋 **High power consumption**
    

**Pruning** helps solve these by removing redundant weights or neurons that contribute little to predictions.

---

## 🧩 3. Intuitive Example

Imagine a neural network as a brain with many neurons and synapses (connections).  
Some connections are _very weak_ and don’t affect decisions much.  
If we **cut** those weak connections, the brain still works fine — but faster and smaller.

That’s exactly what pruning does.

---

## 🧮 4. Basic Workflow of Model Pruning

1. **Train the original (dense) model**
    
    - Get high accuracy on the full dataset.
        
2. **Evaluate weight importance**
    
    - Compute a score for each weight (e.g., magnitude).
        
3. **Remove less important weights**
    
    - Zero out or delete low-importance weights.
        
4. **Fine-tune (retrain) the pruned model**
    
    - Recover accuracy lost due to pruning.
        
5. **Deploy the compressed model**
    

---

## 🧠 5. Types of Model Pruning

|Type|Description|Example|
|---|---|---|
|**Weight pruning (Unstructured)**|Remove individual weights below a threshold.|Set small weights (e.g., < 0.01) to 0.|
|**Structured pruning**|Remove entire neurons, filters, or channels.|Remove full convolution filters or dense layer neurons.|
|**Filter pruning**|Remove whole filters in CNNs.|Reduce number of conv kernels.|
|**Channel pruning**|Remove channels in feature maps.|Reduce input/output dimensions of conv layers.|
|**Layer pruning**|Remove redundant layers.|Drop full residual blocks in ResNet.|
|**Dynamic pruning**|Prune during training dynamically.|WeightDrop, movement pruning.|

---

## 🔢 6. Criteria for Pruning (How to Decide What to Remove)

|Criterion|Description|
|---|---|
|**Magnitude-based pruning**|Remove weights with smallest absolute values (|
|**Gradient-based pruning**|Remove weights with smallest gradients (low impact on loss).|
|**Hessian-based pruning**|Use second-order derivative (sensitivity analysis).|
|**L1/L2 norm pruning**|Compute norm of filters/channels; prune smallest ones.|
|**Information-theoretic**|Remove components with least mutual information with output.|

---

## 🧩 7. Example: Magnitude-Based Weight Pruning (Simple Math)

Suppose weights in a layer = [0.2, 0.001, -0.8, 0.0005, 0.9]

We set a **threshold = 0.01** → remove (set to zero) weights with |w| < 0.01

Result → [0.2, 0, -0.8, 0, 0.9]

💡 The two smallest weights are pruned away.

---

## 💻 8. Simple Example (PyTorch Pseudocode)

`import torch import torch.nn.utils.prune as prune import torch.nn.functional as F  # Example model class Net(torch.nn.Module):     def __init__(self):         super(Net, self).__init__()         self.fc1 = torch.nn.Linear(100, 50)         self.fc2 = torch.nn.Linear(50, 10)      def forward(self, x):         x = F.relu(self.fc1(x))         return self.fc2(x)  model = Net()  # Prune 30% of weights in fc1 based on magnitude prune.l1_unstructured(model.fc1, name="weight", amount=0.3)  # Remove pruning reparametrization and make pruning permanent prune.remove(model.fc1, "weight")  print(model.fc1.weight)`

---

## ⚙️ 9. Fine-tuning After Pruning

After pruning, the model may lose some accuracy.  
Hence, it is **fine-tuned** (retrained) on the same dataset for a few epochs to regain accuracy.

This step is critical — otherwise, the model may underperform.

---

## 🧠 10. Pruning Schedule

Instead of pruning everything at once (aggressive pruning), it’s better to **prune gradually**:

- Start pruning after a few epochs of training.
    
- Slowly increase pruning percentage (e.g., 10%, 20%, 30%) over time.
    
- Fine-tune after each phase.
    

This helps maintain model stability and accuracy.

---

## 📊 11. Evaluation Metrics

|Metric|Description|
|---|---|
|**Sparsity (%)**|Percentage of weights removed.|
|**Model Size Reduction**|Decrease in number of parameters.|
|**Speedup**|Reduction in inference time.|
|**Accuracy Drop**|Difference between pre- and post-pruning accuracy.|

Example:

`Before pruning: 10 million parameters → 95% accuracy After pruning: 3 million parameters → 94.7% accuracy`

---

## 🔬 12. Advantages of Model Pruning

✅ **Reduced model size**  
✅ **Faster inference**  
✅ **Lower memory & power usage**  
✅ **Easier deployment on edge/IoT devices**  
✅ **Can improve generalization (by removing overfitting connections)**

---

## ⚠️ 13. Limitations

❌ Pruning can harm accuracy if overdone.  
❌ Not all hardware benefits equally (depends on sparse-matrix support).  
❌ Harder to tune pruning thresholds optimally.  
❌ Some frameworks (like GPUs) may not efficiently exploit sparsity.

---

## 🧩 14. Related Techniques

|Technique|Description|
|---|---|
|**Quantization**|Reduce precision of weights (e.g., 32-bit → 8-bit).|
|**Knowledge Distillation**|Train a smaller student model using the large model’s outputs.|
|**Low-Rank Factorization**|Approximate weight matrices with lower-rank decompositions.|
|**Neural Architecture Search (NAS)**|Automatically find efficient architectures.|

Often, these are **combined** with pruning for optimal compression.

---

## 🧠 15. Modern Pruning Approaches (Research)

|Paper|Key Idea|
|---|---|
|**"The Lottery Ticket Hypothesis" (Frankle & Carbin, 2019)**|A smaller subnetwork ("winning ticket") within a large model can perform as well as the original.|
|**Movement Pruning (2020)**|Dynamically prune weights based on direction of updates during training.|
|**SNIP (2019)**|Prune before training using gradient sensitivity.|
|**GRASP (2020)**|Prune connections that minimally affect gradient flow.|

---

## 🔍 16. Real-World Use Cases

- **Mobile AI**: Compress CNNs for mobile vision tasks (e.g., MobileNet, EfficientNet).
    
- **Edge Computing**: Deploy pruned models on IoT sensors.
    
- **Large Language Models (LLMs)**: Pruning transformer layers for faster inference.
    
- **Federated Learning**: Reduce communication cost between clients and server.
    

---

## 🧩 17. Summary Table

|Aspect|Description|
|---|---|
|**Goal**|Reduce model size & speed up inference|
|**Main Idea**|Remove less important weights or neurons|
|**Steps**|Train → Prune → Fine-tune|
|**Types**|Unstructured, Structured, Filter, Layer pruning|
|**Metrics**|Sparsity, Speedup, Accuracy drop|
|**Common Libraries**|PyTorch (`torch.nn.utils.prune`), TensorFlow Model Optimization Toolkit|
|**Used In**|Edge AI, FL, LLM compression, CNN optimization|