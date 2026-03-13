# Domain-Specific Fine-Tuning with OpenShift AI

This repository contains a Jupyter Notebook demonstrating Supervised Fine-Tuning (SFT) on Red Hat OpenShift AI (RHOAI) 3.3. 

It takes a generalized foundation model (Qwen2.5-1.5B-Instruct) and specializes its behavior, formatting, and tone for specific industry use cases.

### 💡 Inspiration
This project was inspired by and builds upon the foundational work from the Red Hat AI Innovation Team, specifically the [`runnable_l40_sft.ipynb` notebook found in the training_hub repository](https://github.com/Red-Hat-AI-Innovation-Team/training_hub/blob/main/examples/notebooks/runnable_l40_sft.ipynb).

## ✨ Features
* **Universal Data Loader:** A dynamic configuration block that automatically ingests, parses, and formats wildly different Hugging Face datasets (tabular vs. ShareGPT conversational lists) into standard JSONL ChatML format.
* **Pre-Configured Scenarios:** Instantly swap between Cybersecurity Incident Analysis, Medical Summarization, or First Aid Response by changing a single variable (`ACTIVE_SCENARIO`).
* **Environment Auto-Cleanup:** Automatically deletes old checkpoints for the active scenario before initiating a new run to prevent PVC disk bloat.

## 🛠️ Prerequisites
* **Platform:** Red Hat OpenShift AI 3.3
* **Hardware:** Jupyter Workbench provisioned with at least 1x NVIDIA L40S GPU (or equivalent VRAM).
* **Storage:** Ensure your Cluster Storage (PVC) is increased to **40GB+**. Model checkpoints are large (~5-6GB each), and default 20GB volumes will encounter "No space left on device" errors if saving multiple epochs. 
* **Dependencies:** `training-hub[cuda]` (installed dynamically via pip in Cell 1). *Note: The initial compile of `flash-attn` and `causal-conv1d` during the pip install may take 15-20 minutes depending on CPU allocation.*

## 🚀 Usage
1. Clone this repository into your RHOAI Jupyter workspace.
2. Open the `.ipynb` notebook.
3. In Cell 4, change the `ACTIVE_SCENARIO` variable to your desired demo industry.
4. Click **Run -> Run All Cells**.

---

## ⏭️ Next Steps: From Notebook to Enterprise Pipeline

While this notebook is perfect for rapid experimentation and the "fast path" of fine-tuning, enterprise production requires reproducibility, governance, and automation. 

**1. Migrating to Kubeflow Trainer v2**
The immediate next step for scaling this architecture is swapping the underlying training execution from the Kubeflow v1 Training Operator to **Kubeflow Trainer v2**.

**2. Adopting Composable AI Pipelines**
To move beyond manual, monolithic scripts, this workflow should be transitioned into modular, automated AI Pipelines (Data Science Pipelines in RHOAI). As detailed in the recent Red Hat AI blog (*"Fine-tune AI pipelines in Red Hat OpenShift AI 3.3"*), we can break this notebook down into a flexible, four-step composable pipeline:

* **Data Preparation:** Download, validate, and split datasets from Hugging Face or S3.
* **Training (SFT/OSFT):** Distribute workloads using Kubeflow Trainer to generate the fine-tuned artifacts and loss metrics.
* **Evaluation:** Utilize `LM-eval` components to run rigorous benchmark tasks on the hold-out data.
* **Registration:** Automatically push the finalized model and metadata into the OpenShift AI Model Registry for lineage tracking and serving.

For a complete guide on building out these production-grade pipelines, check out the [kubeflow/pipelines-components repository](https://github.com/kubeflow/pipelines-components) and the RHOAI guided examples.
