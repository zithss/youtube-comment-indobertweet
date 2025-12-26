# 🇮🇩 Semantic Analysis of Indonesian YouTube Comments

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![Status](https://img.shields.io/badge/Status-Research_Prototype-green)

## 📌 Project Overview
This repository contains the implementation of a semantic analysis pipeline designed to extract coherent topics and generate human-like summaries from **noisy Indonesian YouTube comments**. 

Traditional methods (TF-IDF + K-Means) often fail on Indonesian social media text due to the high prevalence of slang (*bahasa alay*), abbreviations, and code-mixing. This project proposes a **Semantic Pipeline** that leverages context-aware embeddings and density-based clustering to overcome these challenges, finalized with an abstractive summarization engine.

### 🚀 Key Technologies
* **Embedding:** [IndoBERTweet](https://huggingface.co/indolem/indobertweet-base-uncased) (Context-aware representation)
* **Dimensionality Reduction:** UMAP (Uniform Manifold Approximation and Projection)
* **Clustering:** HDBSCAN (Hierarchical Density-Based Spatial Clustering)
* **Summarization:** [SeaLLM-v3-7B](https://huggingface.co/SeaLLMs/SeaLLMs-v3-7B-Chat) with **Maximal Marginal Relevance (MMR)** for diversity.

---

## 📄 Abstract
Mining actionable insights from noisy Indonesian YouTube comments remains a challenge for traditional lexical models due to the high prevalence of slang and code-mixing. To address this, we propose a semantic pipeline leveraging **IndoBERTweet** and **HDBSCAN** for clustering, coupled with a diversity-aware summarization engine using **Maximal Marginal Relevance (MMR)** and **SeaLLM-v3-7B**. 

Benchmarked across technology, culinary, and automotive domains, the method achieves an average **Silhouette Score of 0.613**, significantly outperforming the TF-IDF baseline of **0.069**. Qualitative analysis confirms the pipeline’s ability to capture complex, multifaceted sentiments often missed by keyword-based methods. Sensitivity analysis identifies a performance threshold on micro-datasets ($N < 500$) where sparsity hinders cluster formation. However, on adequate data volumes, human evaluation validates the pipeline's effectiveness, yielding an average factual consistency score of **4.06** and an acceptability rate of over **93%**, confirming high fluency and reliability despite subjective variations in rater scoring.

---

## 📊 Performance Highlights

### 1. Clustering Quality (Silhouette Score)
The semantic approach demonstrates superior cluster density compared to the lexical baseline.

| Dataset Domain | Baseline (TF-IDF + K-Means) | **Proposed (IndoBERTweet + HDBSCAN)** |
| :--- | :---: | :---: |
| Technology (GadgetIn) | 0.054 | **0.598** |
| Culinary (Tanboy Kun) | 0.071 | **0.626** |
| Automotive (Fitra Eri) | 0.081 | **0.615** |
| **Average** | **0.069** | **0.613** |

### 2. Human Evaluation
Based on 12 independent annotators evaluating the generated summaries:
* **Factual Consistency:** 4.06 / 5.00
* **Fluency:** 4.03 / 5.00
* **Acceptability Rate:** >93% of summaries were rated as "Standard" or better.

---

## 🛠️ Pipeline Architecture
1.  **Preprocessing:** Emoji handling, lowercase, slang normalization (minimal), duplicate removal.
2.  **Embedding:** Text is converted into 768-dim vectors using `indolem/indobertweet-base-uncased`.
3.  **Projection:** Dimensions reduced to 10-components using UMAP to preserve local neighborhoods.
4.  **Clustering:** HDBSCAN identifies dense regions of comments (Topics) and filters noise (-1).
5.  **Representative Selection:** MMR selects 10 diverse comments per cluster to prevent redundancy.
6.  **Summarization:** SeaLLM-v3-7B generates a concise paragraph based on the selected representatives.
