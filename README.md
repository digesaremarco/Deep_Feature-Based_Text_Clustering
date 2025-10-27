# Deep Feature-Based Text Clustering and its Explanation

This repository reproduces the main experiments and ideas from the paper:

> **Deep Feature-Based Text Clustering and its Explanation**  
> *Zhao et al., Knowledge-Based Systems (2021)*

The goal of this project is to explore how **deep pretrained representations** (ELMo, InferSent, BERT) can be used for **unsupervised text clustering**, and how to interpret cluster assignments using the **TCRE** (Text Clustering Result Explanation) model.

---

## 📘 Overview

The pipeline consists of three main components:

1. **Feature Extraction**  
   - Extract deep embeddings using pretrained models:  
     - [ELMo](https://allennlp.org/elmo)  
     - [InferSent](https://github.com/facebookresearch/InferSent)  
     - [BERT](https://huggingface.co/bert-base-uncased)  
   - Pooling strategies: `mean`, `max`, or `last`  
   - Normalization: `Identity (I)`, `LayerNorm (LN)`, `L2 Norm (N)`

2. **Clustering**  
   - K-Means (`KM`) is used to group documents in the embedding space.  
   - Evaluation metrics:  
     - **ACC** – Clustering accuracy  
     - **NMI** – Normalized Mutual Information  
     - **ARI** – Adjusted Rand Index  

3. **Explainability (TCRE)**  
   - A logistic regression model identifies **indication words** for each cluster.  
   - Words with the highest absolute weights are used to interpret cluster semantics.

---

## 🧩 Experiments

The experiments are structured as follows:

| Model | Pooling | Normalization | Clustering | Dataset(s) | Description |
|:------|:--------|:--------------|:------------|:------------|:-------------|
| ELMo | Mean | LN | KMeans | AG News | LM + Mean + LN + KM |
| ELMo | Mean | I | KMeans | DBpedia | LM + Mean + I + KM |
| ELMo | Mean | N | KMeans | AG News, DBpedia, Yahoo | LM + Mean + N + KM |
| InferSent | LN | KMeans | DBpedia | InferSent + LN + KM |
| InferSent | N | KMeans | AG News | InferSent + N + KM |
| BERT | Max | LN | KMeans | AG News, DBpedia | BERT + Max + LN + KM |

---

## 📊 Visualization

The notebook includes a t-SNE–based visualization of the learned features and cluster assignments.

```python
visualize_clusters(X_norm, y_pred, title="BERT + Max + LN + KM on AG News")
