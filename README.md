# Communication Systems II — Seeing Signals

**Author:** Bempelis Theofilos  
**Department:** Department of Electrical and Computer Engineering, Telecommunications Division, Aristotle University of Thessaloniki (AUTh)

---
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/bebelistheo/Seeing-Signals/blob/main/Seeing_Signals.ipynb)

## Abstract

The present study investigates the problem of Automatic Modulation Classification (AMC) and the characterization of physical-layer impairments in modern telecommunication systems, utilizing Deep Learning methods. For the purposes of this research, an extensive and realistic dataset (76,800 samples) was prepared via simulation, which includes 16 different modulation schemes (ASK, PSK, QAM, HQAM, APSK) under the influence of additive noise (AWGN), phase noise (Phase Noise), I/Q imbalance (I/Q Imbalance), and external interference (Interference). The signals were converted into two-dimensional histograms (Cartesian and Polar images) and analyzed through two radically different approaches: a specialized Multi-Task Convolutional Neural Network (Multi-Task CNN) and a state-of-the-art, generative Multimodal Model (`SmolVLM-256M-Instruct`) adapted via the LoRA technique.

The central axis of the evaluation was the comparative analysis of the two architectures in terms of classification accuracy, robustness in low-SNR environments, sensitivity in detecting distortions, and generalization capability in intermediate, unseen channel conditions. The results demonstrate the quantitative superiority and dramatically lower computational complexity of the CNN, making it ideal for real-time applications. Simultaneously, however, the impressive capability of the VLM to achieve comparable accuracy rates using a minimal fraction of the training data (sample efficiency) is proven, highlighting the potential of Large Foundation Models.

---

## Experimental Results

### Table 1: Multi-Task CNN Accuracy (Unseen Test Set)
Overall accuracy of the multi-criteria Convolutional Neural Network (Multi-Task CNN) across the five independent tasks, evaluated on the unseen test set (Test Set):

| Task | Accuracy |
| :--- | :---: |
| **Modulation Type** | 68.11% (approaching ~96% for SNR > 20 dB) |
| **SNR Range / Severity Level** | 96.88% |
| **Phase Noise Detect** | 77.35% |
| **I/Q Imbalance Detect** | 93.23% |
| **Interference Detect** | 93.59% |

### Table 2: Multimodal SmolVLM Evaluation (A/B Testing)
Comparative accuracy evaluation between the baseline (Standard, $r=16$) and the enhanced (Ultra, $r=64$) multimodal SmolVLM model (trained on 5,000 samples):

| Task | VLM Standard ($r=16$) | VLM Ultra ($r=64$) | Change |
| :--- | :---: | :---: | :---: |
| **Modulation Type** | 58.00% | 64.00% | +6.00% |
| **Phase Noise Detect** | 70.00% | 77.00% | +7.00% |
| **I/Q Imbalance Det.** | 78.00% | 89.00% | +11.00% |
| **Interference Detect** | 90.00% | 87.00% | -3.00% |

---

## Conclusions: Comparative Analysis of Architectures

1. **Quantitative Superiority (CNN):** The CNN prevailed consistently across all metrics (e.g., 93.2% vs. 89.0% in I/Q Imbalance, 68.1% vs. 64.0% in modulation). This superiority is due to the specialized classification heads, which are optimized purely for statistical prediction, free from the "burden" of text generation (autoregressive decoding) of VLMs.
2. **Generalization Capability (Sample Efficiency):** Despite lagging behind, the VLM demonstrated impressive efficiency. While the CNN "mapped" the problem by training on ~53,700 samples, the VLM achieved accuracies of ~89% having been trained on only 5,000 images. Achieving these percentages with 10 times fewer data underscores the strong generalization capability of Multimodal Models.
3. **Computational Complexity:** The VLM is an extremely "heavy" model. As the Ablation study showed, it requires high parametric capacity (the Ultra version prevailed over the simple LoRA $r=16$) and the use of accelerators (NVIDIA A100), restricting it to offline diagnostic roles. Conversely, the CNN has a minimal memory footprint and nearly zero inference time, making it completely viable for integration into edge devices and real-time analysis.
4. **Flexibility vs. Accuracy:** The CNN is strictly rigid, requiring radical retraining to incorporate new tasks. Conversely, the VLM is inherently flexible and can be rapidly adapted to new diagnostic requirements via simple language commands (prompting).

---

## Repository Structure
* `Seeing_Signals_english.ipynb`: Complete Google Colab Notebook (PyTorch & HuggingFace Transformers/PEFT implementation).
* `Seeing_Signals_Report_EN.pdf`: Technical Report in English.

