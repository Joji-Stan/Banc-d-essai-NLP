## 🧠 Stratégie d'entraînement des modèles

Nous avons entraîné plusieurs modèles de classification :  **DistillBert**, **ModernBert**, et **Deberta-v3-base**,sur trois jeux de données : **HateSpeech**, **IMDB**, et **OHSUMED**, en comparant les performances avec et sans l'utilisation de **LoRA (Low-Rank Adaptation)**.

### 📊 Séparation des données
Chaque dataset a été divisé comme suit :
- **80% pour l'entraînement**
- **20% pour l'évaluation**

Cette répartition assure un bon équilibre entre apprentissage et validation.

### ⚙️ Paramètres d'entraînement communs
Les entraînements ont été réalisés avec les configurations suivantes :
- **Optimiseur** : AdamW
- **Scheduler** : Linear ou Cosine selon les cas
- **Learning Rate** : adapté pour chaque modèle
- **Weight Decay** : utilisé pour la régularisation
- **Nombre d’époques** : déterminé pour chaque jeu de données en fonction de la convergence
- **Gradient Accumulation Steps** : utilisé pour simuler de plus grands batchs
- **Tokenizer** : `distilbert-base-uncased` (pour les modèles DistilBERT), `deberta-v3-base` (pour le modèle deberta-v3-base)

### 🔒 Fine-tuning sans LoRA
Pour les versions **sans LoRA**, une approche de fine-tuning partiel a été utilisée :
- **Toutes les couches du modèle ont été gelées**, à l'exception **des deux dernières couches**.
- Cette méthode permet de réduire le temps d'entraînement et la consommation de ressources tout en maintenant une capacité d’adaptation.

### 🧩 Entraînement avec LoRA
Dans les versions **avec LoRA**, nous avons utilisé un entraînement avec adaptation à faible-rang pour améliorer l'efficacité et la performance tout en utilisant moins de ressources et de temps de calcul.

### 🗂️ HateSpeech

| **Méthode**     | **Modèle**              | **Accuracy** | **F1 (macro)** | **Precision** | **Recall** | **Loss** | **Epochs** | **Batch (train/eval)** | **LR**   | **Weight Decay** | **LoRA Config**            | **Tokenizer**              | **Temps/époque (s)** | **CO₂ (kg)** | **Grad Acc Steps** | **Warmup Steps** | **Scheduler** | **Max Grad Norm** | **Seed** | **GPU** |
|------------------|--------------------------|--------------|----------------|---------------|-------------|-----------|-------------|-------------------------|----------|------------------|----------------------------|-----------------------------|----------------------|--------------|--------------------|------------------|----------------|-------------------|----------|----------|
| Avec LoRA        | distilbert-base-uncased  | 0.9054       | 0.5807         | 0.8878        | 0.5441      | 0.3171    | 3           | 8 / 8                   | 5e-05    | 0.01             | r=8, α=16, dropout=0.1     | distilbert-base-uncased    | 225.9512             | 0.0010       | -                  | -                | -              | -                 | -        | T4       |
| Sans LoRA        | distilbert-base-uncased  | 0.9054       | 0.5807         | 0.8878        | 0.5441      | 0.3171    | 3           | 8 / 8                   | 5e-05    | 0.01             | non utilisé                | distilbert-base-uncased    | 225.9512             | 0.0010       | -                  | -                | -              | -                 | -        | T4       |
| Avec LoRA        | modern-bert              | 0.8597       | 0.2311         | 0.9649        | 0.25        | 0.4783    | 3           | 8 / 8                   | 5e-05    | 0.05             | r=8, α=16, dropout=0.1     | modern-bert-base          | 1150.0000            | 0.0299       | 2                  | 2000             | LINEAR         | 1                 | 123      | T4       |
| Sans LoRA        | modern-bert              | 0.85746      | 0.2308         | 0.9643        | 0.25        | 0.4977    | 3           | 8 / 8                   | 5e-05    | 0.05             | non utilisé                | modern-bert-base          | 1400.0000            | 0.0236       | 2                  | 2000             | LINEAR         | 1                 | 123      | T4       |
| Avec LoRA        | deberta-v3-base           | 0.8913       | 0.3718         | 0.8808        | 0.3654      | 0.3820    | 3           | 8 / 8                   | 5e-05    | 0.01             | r=8, α=16, dropout=0.1     | deberta-v3-base           | 468.1251             | 0.0071       | -                  | -                | -              | -                 | -        | T4       |
| Sans LoRA        | deberta-v3-base           | 0.9018       | 0.5673         | 0.8519        | 0.5406      | 0.3417    | 3           | 8 / 8                   | 5e-05    | 0.01             | non utilisé                | deberta-v3-base           | 252.1042             | 0.0037       | -                  | -                | -              | -                 | -        | T4       |

### 🗂️ IMDB

| **Méthode**     | **Modèle**              | **Accuracy** | **F1 (macro)** | **Precision** | **Recall** | **Loss** | **Epochs** | **Batch (train/eval)** | **LR**   | **Weight Decay** | **LoRA Config**            | **Tokenizer**              | **Temps/époque (s)** | **CO₂ (kg)** | **Grad Acc Steps** | **Warmup Steps** | **Scheduler** | **Max Grad Norm** | **Seed** | **GPU** |
|------------------|--------------------------|--------------|----------------|---------------|-------------|-----------|-------------|-------------------------|----------|------------------|----------------------------|-----------------------------|----------------------|--------------|--------------------|------------------|----------------|-------------------|----------|----------|
| Avec LoRA        | distilbert-base-uncased  | 0.9026       | 0.9026         | 0.9026        | 0.9027      | 0.2434    | 5           | 32 / 32                 | 3e-05    | 0.05             | r=8, α=16, dropout=0.1     | distilbert-base-uncased    | 1136.7510            | 0.0104       | 2                  | 1000             | COSINE         | 0.8               | 123      | T4       |
| Sans LoRA        | distilbert-base-uncased  | 0.9230       | 0.9230         | 0.9230        | 0.9230      | 0.1996    | 5           | 32 / 32                 | 3e-05    | 0.05             | non utilisé                | distilbert-base-uncased    | 733.0113             | 0.0067       | 2                  | 1000             | COSINE         | 0.8               | 123      | T4       |
| Avec LoRA        | modern-bert              | 0.8344       | 0.8394         | 0.8361        | 0.0346      | 0.3895    | 5           | 8 / 8                   | 5e-05    | 0.05             | r=8, α=16, dropout=0.1     | modern-bert-base          | 608.4                | -            | 2                  | 2000             | LINEAR         | 1                 | 123      | T4       |
| Sans LoRA        | modern-bert              | 0.8484       | 0.8482         | 0.8485        | 0.8481      | 1.001     | 5           | 8 / 8                   | 5e-05    | 0.05             | non utilisé                | modern-bert-base          | 1005.8               | 0.1183       | 2                  | 2000             | LINEAR         | 1                 | 123      | T4       |
| Avec LoRA        | deberta-v3-base           | 0.9354       | 0.9354         | 0.9354        | 0.9354      | 0.2160    | 1           | 8 / 8                   | 5e-05    | 0.01             | r=8, α=16, dropout=0.1     | deberta-v3-base           | 1220.1435            | 0.0138       | -                  | -                | -              | -                 | -        | T4       |
| Sans LoRA        | deberta-v3-base           | 0.9412       | 0.9412         | 0.9413        | 0.9411      | 0.1846    | 1           | 8 / 8                   | 5e-05    | 0.01             | non utilisé                | deberta-v3-base           | 709.3585             | 0.0080       | -                  | -                | -              | -                 | -        | T4       |

### 🗂️ OHSUMED

| **Méthode**     | **Modèle**              | **Accuracy** | **F1 (macro)** | **Precision** | **Recall** | **Loss** | **Epochs** | **Batch (train/eval)** | **LR**   | **Weight Decay** | **LoRA Config**            | **Tokenizer**              | **Temps/époque (s)** | **CO₂ (kg)** | **Grad Acc Steps** | **Warmup Steps** | **Scheduler** | **Max Grad Norm** | **Seed** | **GPU** |
|------------------|--------------------------|--------------|----------------|---------------|-------------|-----------|-------------|-------------------------|----------|------------------|----------------------------|-----------------------------|----------------------|--------------|--------------------|------------------|----------------|-------------------|----------|----------|
| Avec LoRA        | distilbert-base-uncased  | 0.4727       | 0.4385         | 0.4402        | 0.4558      | 1.5091    | 3           | 8 / 8                   | 5e-05    | 0.01             | r=8, α=16, dropout=0.1     | distilbert-base-uncased    | 1312.6677            | 0.0147       | -                  | -                | -              | -                 | -        | T4       |
| Sans LoRA        | distilbert-base-uncased  | 0.4702       | 0.4367         | 0.4348        | 0.4507      | 1.5054    | 3           | 8 / 8                   | 5e-05    | 0.01             | non utilisé                | distilbert-base-uncased    | 1229.2075            | 0.0184       | -                  | -                | -              | -                 | -        | T4       |
| Avec LoRA        | modern-bert              | 0.2882       | 0.2251         | 0.2185        | 0.2322      | 2.441     | 3           | 8 / 8                   | 5e-05    | 0.05             | r=8, α=16, dropout=0.1     | modern-bert-base          | 2247.67              | 0.021        | 2                  | 2000             | LINEAR         | 1                 | 123      | T4       |
| Sans LoRA        | modern-bert              | 0.3205       | 0.1615         | 0.6308        | 0.1685      | 2.3340    | 3           | 8 / 8                   | 5e-05    | 0.05             | non utilisé                | modern-bert-base          | 2205.67              | 0.0210       | 2                  | 2000             | LINEAR         | 1                 | 123      | T4       |
| Avec LoRA        | deberta-v3-base          | 0.4292       | 0.2979         | 0.4432        | 0.3316      | 1.8616    | 5           | 8 / 8                   | 5e-05    | 0.01             | r=8, α=16, dropout=0.15    | deberta-v3-base            | 7204.8530            | 0.0858       | -                  | -                | -              | -                 | -        | T4       |
| Sans LoRA        | deberta-v3-base          | 0.4299       | 0.2889         | 0.4452        | 0.3476      | 1.8514    | 5           | 8 / 8                   | 5e-05    | 0.01             | non utilisé                | deberta-v3-base            | 7154.5678            | 0.878        | -                  | -                | -              | -                 | -        | T4       |

Les résultats obtenus par nos entraînements sont cohérents avec les ressources que nous avons pu trouver, par exemple cet [article d’Hugging Face](https://huggingface.co/blog/modernbert) qui présente des métriques pour les modèles DeBERTa et ModernBERT.

Nous pouvons observer les différences entre nos évaluations et d'autres réalisées sur le dataset IMDb via cette page : [Papers with Code – Sentiment Analysis on IMDb](https://paperswithcode.com/sota/sentiment-analysis-on-imdb).

De même, pour le dataset Ohsumed, des comparaisons sont possibles ici : [Papers with Code – Text Classification on Ohsumed](https://paperswithcode.com/sota/text-classification-on-ohsumed).

🗂️ Tâche de Retrieval

## 📌 Objectif
Évaluer et comparer différents modèles de **sentence embedding** pour la tâche de retrieval de documents (recherche d'information) sur différents corpus :

## modèles sentences embeddings 


## 🗂️ Corpus
- **Quora**
- **NFCorpus**

**Fonctions principales** :
- Création/sauvegarde/chargment embeddings
- Similarité cosinus sur les embeddings
- Utilisation de `pytrec_eval` pour l'évaluation

## Métriques d'évaluation
- nDCG@100 : Pertinence des résultats en tenant compte de leur position
- MAP@100 : Précision moyenne sur les 100 premiers résultats

## Résultats
| # | Modèle           | Quora nDCG@100 | Quora MAP@100 | NFCorpus nDCG@100 | NFCorpus MAP@100 |
|---|------------------|----------------|---------------|-------------------|------------------|
| 1 | gte-modernbert   | 0.8832         | 0.8354        | 0.2728            | 0.1361           |
| 2 | distilbert-cos   | 0.8920         | 0.8477        | 0.2297            | 0.1084           |
| 3 | deberta-st       | 0.3217         | 0.2696        | 0.0202            | 0.0024           |



