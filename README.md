# (RU) Кластеризация русскоязычных запросов к чатботу по пользовательским интентам

### Использованный датасет: [huggingface.co/datasets/AmazonScience/massive](huggingface.co/datasets/AmazonScience/massive), подвыборка RU-ru, 1000 текстов из сплита *validation* (39 исходных интентов).

Произведено сравнение нескольких комбинаций эмбеддеров (CountVectorizer, [FRIDA](https://huggingface.co/ai-forever/FRIDA)) и алгоритмов кластеризации (*k*-means, Bisecting *k*-means, Spectral, Agglomerative Clustering, HDBSCAN). Результаты всех из них оказались довольно низки по сравнению с решением задачи с помощью LLM. Лучшие из них (в порядке убывания ключевых метрик Adjusted Rand Index и Normalized Mutual Information):
- Agglomerative, CountVectorizer
- *k*-means, CountVectorizer
- *k*-means, FRIDA
- Bisecting *k*-means, FRIDA
- Agglomerative, FRIDA

Полученные с помощью наилучшей по метрикам кластеризации (Agglomerative, CountVectorizer) наборы текстов, находящиеся в одном кластере, были переданы YandexGPT Pro(temperature=0.7) в составе промпта, целью которого было создание краткого описания семантики кластера. Из-за плохого качества кластеризации результат семантизации кластеров оказался неинформативным.

Клаастеризация с помощью YandexGPT Pro показала наилучшие результаты. Модели подавался список уже выделенных интентов (при наличии), небольшой набор примеров для Few-Shot классификации (кроме Zero-Shot решения), полученных из сплита *test* автоматически и переведённых на русский язык, а также текст, которому необходимо было сопоставить один из существующих интентов или сформулировать новый. Во втором случае полученная метка добавлялась в список существующих интентов. Далее этот пайплайн перезапускался с новым текстом.

## Метрики

| Метод | ARI | NMI |
|---------|--------|---------|
| Agg., CountVec | 8.17 | 33.98 |
| *k*-means, CountVec | 6.87 | 28.13 | 
| *k*-means, FRIDA | 1.36 | 10.37 | 
| Bisecting *k*-means, FRIDA | 1.31 | 10.20 | 
| Agglomerative, FRIDA | 1.15 | 10.86 |
| GPT 20-Shot | 60.47 | 77.91 |
| GPT 10-Shot | 63.39 | 77.75 |
| GPT 5-Shot | 45.61 | 71.74 |
| GPT Zero-Shot | 41.34 | 74.48 |

## Содержимое
##### Файл .ipynb, содержащий код для:
- Математической кластеризации
- Семантизации кластеров с помощью YandexGPT
- Кластеризации и определения интентов с помощью YandexGPT

##### Объекты кластеризации:
- Agglomerative, CountVectorizer
- *k*-means, CountVectorizer
- *k*-means, FRIDA
- Bisecting *k*-means, FRIDA
- Agglomerative, FRIDA
- HDBSCAN(min_cluster_size=5), FRIDA

# (EN) Russian-language chatbot request intent clustering

### Dataset used: [huggingface.co/datasets/AmazonScience/massive](huggingface.co/datasets/AmazonScience/massive), RU-ru subset, 1000 texts from the *validation* split containing 39 ground truth intent labels.

Includes a comparison of several embedder (CountVectorizer, [FRIDA](https://huggingface.co/ai-forever/FRIDA)) and clustering algorithm (*k*-means, Bisecting *k*-means, Spectral, Agglomerative Clustering, HDBSCAN) combinations, none of which showed satisfactory results using the chosen metrics (Adjusted Rand Index and Normalized Mutual Information). The best-performing of them were:
- Agglomerative, CountVectorizer
- *k*-means, CountVectorizer
- *k*-means, FRIDA
- Bisecting *k*-means, FRIDA
- Agglomerative, FRIDA

Texts of each cluster obtained from the top-1 clustering-embedding combination (Agglomerative, CountVectorizer) were fed to YandexGPT Pro(temperature=0.7) with a prompt aimed at creating a short semantic definition of each cluster. However, due to poor clustering quality, the result was not highly informative.

Clustering via YandexGPT Pro with a customized prompt achieved the highest metrics. The model was fed a list of existing intent labels (if any), a small sample of pre-labelled Few-Shot examples (unless Zero-Shot) automatically obtained and translated from the test split of the dataset, and a text which was to be labelled. The output label was then added to the list of existing intents (unless it already was), and the pipeline was run again with a new text from the training dataset.

## Metrics

| Method | ARI | NMI |
|---------|--------|---------|
| Agg., CountVec | 8.17 | 33.98 |
| *k*-means, CountVec | 6.87 | 28.13 | 
| *k*-means, FRIDA | 1.36 | 10.37 | 
| Bisecting *k*-means, FRIDA | 1.31 | 10.20 | 
| Agglomerative, FRIDA | 1.15 | 10.86 |
| GPT 20-Shot | 60.47 | 77.91 |
| GPT 10-Shot | 63.39 | 77.75 |
| GPT 5-Shot | 45.61 | 71.74 |
| GPT Zero-Shot | 41.34 | 74.48 |

## Contents
##### The Python Notebook, containing code to create:
- Non-LLM clustering
- LLM-powered cluster naming
- LLM-powered Zero-Shot and Few-Shot intent discovery

##### Clustering objects:
- Agglomerative, CountVectorizer
- *k*-means, CountVectorizer
- *k*-means, FRIDA
- Bisecting *k*-means, FRIDA
- Agglomerative, FRIDA
- HDBSCAN(min_cluster_size=5), FRIDA
