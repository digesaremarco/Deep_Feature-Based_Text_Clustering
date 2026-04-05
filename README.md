# Deep Feature-Based Text Clustering and its Explanation

This repository implements and analyzes the methodology proposed in:

> **Zhao et al. (2021)**  
> *Deep Feature-Based Text Clustering and its Explanation*, Knowledge-Based Systems

The objective of this project is to investigate the effectiveness of **deep contextual embeddings** for unsupervised text clustering and to study how clustering results can be interpreted through **post-hoc explainability methods (TCRE-style)**.

---

## 📌 Project Objective

The main goals of this work are:

- Evaluate the effectiveness of pretrained language model embeddings for text clustering
- Study the impact of pooling and normalization strategies on clustering performance
- Compare different clustering algorithms (K-Means and Agglomerative Clustering)
- Provide interpretable explanations of clusters using a linear surrogate model (TCRE approach)

---

## ⚙️ Methodology

The proposed pipeline follows a standard representation-learning and clustering workflow:

> **Text → Pretrained Encoder → Pooling → Normalization → Clustering → Evaluation → Explanation**

---

## 1. Text Representation

Documents are encoded using pretrained contextual language models:

- **ELMo**
- **BERT** (`bert-base-uncased`)

### Pooling Strategies
To obtain fixed-size sentence embeddings:

- Mean pooling
- Max pooling
- Last token representation

### Normalization Techniques

- **Identity (I)**: no normalization
- **Layer Normalization (LN)**: stabilizes embedding distribution
- **L2 Normalization (N)**: projects embeddings onto the unit hypersphere

> Key insight: the geometric properties of embeddings strongly influence clustering quality.

---

## 2. Clustering Methods

The following clustering algorithms are evaluated:

### K-Means
- Assumes spherical and isotropic clusters
- Performs well on well-separated embedding spaces
- Computationally efficient

### Agglomerative Clustering
- Hierarchical bottom-up approach
- Captures local structure in the data
- More sensitive to noise and representation quality

---

## 3. Evaluation Metrics

Clustering performance is evaluated using standard external metrics:

- **ACC (Clustering Accuracy)**: computed after optimal label alignment
- **NMI (Normalized Mutual Information)**
- **ARI (Adjusted Rand Index)**

These metrics measure agreement between predicted clusters and ground-truth labels.

---

## 4. Interpretability (TCRE-style Explanation)

To improve interpretability of clustering results, a post-hoc explanation model is applied:

1. Cluster assignments are treated as pseudo-labels  
2. A **Logistic Regression classifier** is trained using Bag-of-Words features  
3. The most influential words (highest absolute weights) are extracted per cluster  

This procedure provides **human-readable semantic interpretations** of clusters.

---

## 📊 Experimental Setup

### Datasets

Experiments are conducted on widely used text classification benchmarks:

- AG News
- DBpedia
- Yahoo Answers
- Reuters R5
- Emotion dataset (short text classification)

> The number of clusters is set equal to the number of ground-truth classes.

---

## 🧪 Controlled Variables

The study systematically analyzes the impact of:

- Pooling strategy
- Normalization method

All other components are kept fixed to ensure fair comparison.

---

## 📈 Key Results

### 1. Representation Quality is the Dominant Factor
Clustering performance is primarily determined by embedding quality rather than the clustering algorithm itself.

---

### 2. Best Performing Configuration

The most consistent and robust configuration is:

> **ELMo + Mean Pooling + Layer Normalization + K-Means**

This setup achieves:
- High clustering accuracy
- Stable behavior across datasets
- Good interpretability

---

### 3. Effect of Normalization

- **LayerNorm**: most stable and robust across datasets
- **L2 normalization**: beneficial for some structured datasets, but may degrade performance on others
- **No normalization**: leads to unstable embedding geometry

---

### 4. Pooling Strategy

- **Mean pooling**: most reliable and stable
- **Max pooling**: often leads to anisotropic embeddings and degraded clustering performance
- **Last token**: generally less stable

---

### 5. Clustering Algorithm Comparison

- **K-Means**:
  - Best for compact and isotropic embeddings
- **Agglomerative Clustering**:
  - Better for local or hierarchical structures
  - More sensitive to noise

---

### 6. Dataset Difficulty

| Dataset   | Behavior |
|-----------|----------|
| AG News   | Well-separated semantic classes |
| DBpedia   | Structured but complex hierarchy |
| Reuters R5| Strong separability |
| Yahoo     | High lexical overlap, noisy clusters |
| Emotion   | Short texts, weak semantic signal |

---

## 🧠 Interpretability Results

### AG News (Best Case)
Clusters show clear semantic structure:

- **World**: political and international entities
- **Sports**: teams, matches, competitions
- **Business**: markets, finance, economy
- **Sci/Tech**: technology, research, systems

---

### DBpedia
Clusters reflect ontology-like categories:

- Geography: cities, regions
- Biology: species, taxonomy
- Companies: organizations and firms
- Arts: media, entertainment

---

### Failure Case (Yahoo Answers)
Clustering quality degrades due to:

- Informal language
- High lexical overlap across topics
- Presence of noisy tokens (e.g., URLs, slang)

---

## 📉 Limitations

Clustering performance degrades under:

- Short text length (low semantic context)
- High class overlap
- Anisotropic embedding spaces (especially with max pooling)
- Poor normalization strategies

---

## 📌 Conclusions

- Embedding geometry is the key determinant of clustering quality
- Pooling and normalization significantly affect performance
- K-Means remains a strong baseline for isotropic embeddings
- Interpretability via TCRE confirms semantic coherence of clusters


