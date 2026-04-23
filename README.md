# Named Entity Recognition: Static vs. Contextual Embeddings
**A Comparative Case Study on the CoNLL-2003 Dataset**
---

## Project Overview
This project presents an empirical comparison between **Static Word Embeddings** (GloVe, Word2Vec) and **Contextual Embeddings** (BERT, RoBERTa) for the task of Named Entity Recognition (NER). 

The study investigates the fundamental trade-offs between these architectures, focusing on four key linguistic and computational metrics:
1. **Accuracy Performance:** F1-Score, Precision, and Recall.
2. **Handling Polysemy:** The ability to disambiguate word senses based on surroundings.
3. **Out-of-Vocabulary (OOV) Resilience:** Performance on terms not seen during training.
4. **Syntactic Complexity:** Impact of sentence length on model stability.
5. **Computational Efficiency:** Inference latency benchmarking.

---

###  Analysis
The analysis is contained within a unified Jupyter Notebook. It performs the following:
* **Data Loading:** Pulls the CoNLL-2003 dataset via Hugging Face `datasets`.
* **Static Training:** Trains a BiLSTM with GloVe (100d) and Word2Vec (300d).
* **Contextual Fine-Tuning:** Fine-tunes `bert-base-uncased` and `roberta-base`.
* **Dynamic Slicing:** Creates custom test subsets for Polysemy, OOV, and Sentence Length.
* **Visualization:** Generates the quantitative F1-score comparison charts.

---

## Key Findings

### Quantitative Comparison (F1-Score)
| Subset        | GloVe (100d) | Word2Vec (300d) | BERT  | RoBERTa |
|---------------|--------------|-----------------|-------|---------|
| **Overall**   | 0.7377       | 0.6763          | 0.8924| **0.9078** |
| **Polysemy**  | 0.7275       | 0.6747          | 0.8885| **0.9045** |
| **OOV Terms** | 0.7059       | 0.6369          | 0.8899| **0.9082** |
| **Long (>25)**| 0.7042       | 0.6466          | 0.8805| **0.9175** |

### Insights
* **Context is King:** RoBERTa and BERT outperform static models by **~25%** in OOV scenarios due to subword tokenization (BPE/WordPiece).
* **Efficiency vs. Accuracy:** Static BiLSTM models are **13x faster** (0.01ms latency) than Transformers (0.13ms), making them suitable for low-latency edge computing.
* **Syntactic Decay:** BiLSTMs show a performance drop in long sequences ($>25$ tokens), whereas Transformers remain stable or improve due to Global Self-Attention.
* **Recall Deficit:** Word2Vec (300d) suffered from a severe recall bottleneck (0.62), missing entities that lower-dimensional GloVe models successfully captured.
<img width="1384" height="1784" alt="image" src="https://github.com/user-attachments/assets/be833d35-40a0-4df2-b756-67a96f2e3543" />


---

## Poster Presentation
The findings of this case study are summarized in an academic poster created in LaTeX. It includes:
* **Qualitative Case Studies:** Side-by-side inference examples showing model behavior on specific tokens like "Uzbekistan" and "Oleg Shatskiku."
* **Hardware Specs:** Benchmarks performed on NVIDIA Tesla T4 GPU infrastructure.
* **Final Conclusions:** Recommendations for model selection in production environments.

---

## Acknowledgements
* **Dataset:** CoNLL-2003 (Tjong Kim Sang et al., 2003).
* **Embeddings:** Stanford GloVe & Google Word2Vec.
* **Frameworks:** Hugging Face Transformers & PyTorch.
