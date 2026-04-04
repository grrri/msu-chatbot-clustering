# Russian-language chatbot request intent clustering using mathematical clustering and YandexGPT Pro

### Dataset used: [huggingface.co/datasets/AmazonScience/massive](huggingface.co/datasets/AmazonScience/massive), RU-ru subset.

Includes a comparison of several different embedder (CountVectorizer, BERT, FRIDA) and clustering algorithm (K-means, Bisecting K-means, Spectral, Agglomerative, HDBSCAN) combinations, none of which showed satisfactory results.
The use of YandexGPT with a customized prompt achieved the highest metrics.

## Metrics

| Method | ARI | NMI |
|---------|--------|---------|
| K-means | 1.50 | 10.77  | 
| GPT Zero-shot | 63.51 | 84.34 |
| GPT Few-shot | 58.22 | 80.80 |

## Contents
##### The Python Notebook, containing code to create:
- Non-LLM clustering
- LLM-powered cluster naming
- LLM-powered Zero-Shot and Few-Shot intent discovery

##### Clustering objects:
- Agglomerative, CountVec
- *k*-means, CountVec
- *k*-means, FRIDA
- Bisecting *k*-means, FRIDA
- Agglomerative, FRIDA
- HDBSCAN(min_cluster_size=5), FRIDA
