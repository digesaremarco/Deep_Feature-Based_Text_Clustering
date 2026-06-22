# Deep Feature-Based Text Clustering and its Explanation

This repository implements and analyzes the methodology proposed in:

> **Zhao et al. (2021)**
> *Deep Feature-Based Text Clustering and its Explanation*, Knowledge-Based Systems

The goal of this project is to investigate the effectiveness of deep contextual representations for unsupervised text clustering and to analyze how clustering decisions can be interpreted through post-hoc explainability techniques inspired by the TCRE framework.

---

## Project Overview

Recent advances in pretrained language models have significantly improved the quality of textual representations. However, while these representations are widely used for supervised tasks, their behavior in unsupervised clustering scenarios remains less explored.

This project studies the relationship between embedding geometry and clustering performance by systematically evaluating different representation extraction strategies, normalization techniques, and clustering algorithms. In addition, the semantic coherence of the resulting clusters is examined through an interpretable surrogate model capable of identifying the most representative terms associated with each cluster.

The overall pipeline follows a representation-learning and clustering workflow:

**Text → Pretrained Encoder → Pooling → Normalization → Clustering → Evaluation → Explanation**

---

## Text Representation

Documents are encoded using pretrained contextual language models capable of capturing semantic and syntactic information beyond traditional Bag-of-Words representations.

The following encoders are considered:

* **ELMo**
* **BERT** (`bert-base-uncased`)

Since clustering algorithms require fixed-size vectors, contextual token representations are aggregated into sentence-level embeddings through different pooling mechanisms. Mean pooling computes the average representation across all tokens and generally produces smooth and stable embeddings. Max pooling emphasizes the most activated dimensions and may generate more anisotropic representations. The last-token strategy uses the final contextual representation as a document embedding.

To further study the role of embedding geometry, several normalization procedures are applied after pooling. The identity transformation leaves embeddings unchanged, Layer Normalization standardizes feature distributions and reduces variance across samples, while L2 normalization projects embeddings onto the unit hypersphere, emphasizing angular similarity.

The underlying hypothesis is that clustering quality is strongly influenced by the geometric structure induced by these representation choices.

---

## Clustering Methods

Two widely adopted clustering algorithms are evaluated throughout the experiments.

**K-Means** partitions the embedding space into a predefined number of clusters by minimizing within-cluster variance. Its effectiveness largely depends on the presence of compact and approximately spherical groups in the feature space.

**Agglomerative Clustering** follows a hierarchical bottom-up strategy in which individual samples are progressively merged according to a similarity criterion. This approach can capture local structural relationships but is generally more sensitive to noise and representation quality.

The number of clusters is always fixed to the number of ground-truth classes in the corresponding dataset.

---

## Evaluation Protocol

Clustering quality is assessed using external evaluation metrics computed against the available class labels.

**Clustering Accuracy (ACC)** measures the proportion of correctly assigned samples after optimal alignment between predicted clusters and ground-truth labels.

**Normalized Mutual Information (NMI)** quantifies the amount of shared information between cluster assignments and true labels while remaining invariant to label permutations.

**Adjusted Rand Index (ARI)** evaluates pairwise agreement between partitions while correcting for chance.

Together, these metrics provide a comprehensive assessment of clustering performance.

---

## Cluster Interpretability

To improve the transparency of clustering results, a post-hoc explanation procedure inspired by the TCRE methodology is applied.

Cluster assignments produced by the unsupervised algorithm are first treated as pseudo-labels. A sparse Bag-of-Words representation of the original documents is then used to train a Logistic Regression classifier that approximates the clustering decisions. The learned coefficients are subsequently analyzed to identify the most influential words associated with each cluster.

This approach provides human-readable explanations that facilitate the semantic interpretation of discovered groups without modifying the clustering process itself.

---

## Experimental Setting

Experiments are conducted on several benchmark datasets commonly used in text classification and clustering research:

* AG News
* DBpedia
* Yahoo Answers
* Reuters R5
* Emotion

These datasets cover a variety of domains, document lengths, semantic structures, and difficulty levels, allowing a comprehensive analysis of the proposed methodology.

To ensure a fair comparison, pooling strategies and normalization methods are systematically varied while all remaining components are kept unchanged.

---

## Experimental Findings

The results consistently indicate that representation quality is the dominant factor affecting clustering performance. Differences induced by embedding extraction and normalization often have a larger impact than the choice of clustering algorithm itself.

Among all evaluated configurations, the combination of **ELMo embeddings**, **Mean Pooling**, **Layer Normalization**, and **K-Means** provides the most stable and robust performance across datasets. This setting achieves strong clustering accuracy while maintaining a high degree of semantic interpretability.

Normalization plays a crucial role in shaping the geometry of the embedding space. Layer Normalization produces the most reliable results across datasets by reducing variability and improving cluster compactness. L2 normalization is beneficial in some structured domains but can occasionally distort useful information. In contrast, the absence of normalization often leads to unstable cluster structures and lower performance.

Pooling strategy also significantly influences clustering behavior. Mean pooling consistently generates well-balanced representations and emerges as the most reliable choice. Max pooling tends to amplify a limited number of dimensions, resulting in anisotropic embeddings that negatively affect clustering quality. Representations derived from the last token generally exhibit less stable behavior.

The comparison between clustering algorithms reveals that K-Means remains a highly competitive baseline when embeddings form compact and approximately isotropic groups. Agglomerative Clustering can better capture local or hierarchical structures but is more sensitive to noise and representation artifacts.

Dataset characteristics further influence performance. AG News and Reuters R5 exhibit clear semantic separability and are therefore easier to cluster. DBpedia presents a richer hierarchical organization that increases complexity while remaining structured. Yahoo Answers is substantially more challenging because of lexical overlap, informal language, and noisy content. The Emotion dataset is also difficult due to the short length of texts and the limited semantic signal available in individual samples.

---

## Qualitative Analysis

The interpretability framework confirms that high-quality clusters correspond to meaningful semantic categories.

On AG News, clusters naturally align with topics such as world affairs, sports, business, and science or technology. The most influential terms extracted by the surrogate model clearly reflect the semantic content of each category.

For DBpedia, the discovered clusters closely resemble ontology-level concepts, including geographic entities, biological categories, companies, and artistic domains.

More challenging datasets provide useful insights into the limitations of the approach. In Yahoo Answers, explanations often reveal substantial lexical overlap between clusters, highlighting the difficulty of separating topics in noisy user-generated content.

