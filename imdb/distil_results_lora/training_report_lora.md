
# 📊 Rapport d'entraînement et d'évaluation

## 🔍 **Résultats**
- **Accuracy** : 0.9026
- **F1-score (macro)** : 0.9026
- **Precision globale** : 0.9026
- **Recall global** : 0.9027
- **Loss finale** : 0.2434

## ⚙️ **Hyperparamètres**
- **Epochs** : 5
- **Batch Size (train / eval)** : 32 / 32
- **Gradient Accumulation Steps** : 2
- **Learning Rate** : 3e-05
- **Weight Decay** : 0.05
- **Warmup Steps** : 1000
- **Scheduler** : SchedulerType.COSINE
- **Max Grad Norm** : 0.8
- **Seed** : 123
- **LoRA Config** : r=8, alpha=16, dropout=0.1

## 🧠 **Tokenizer**
- **Tokenizer utilisé** : distilbert-base-uncased

## ⏱ **Temps d'entraînement**
- **Temps moyen par époque** : 1136.7510 sec

## 🌱 **Empreinte carbone**
- **CO₂ estimé** : 0.0104 kg

