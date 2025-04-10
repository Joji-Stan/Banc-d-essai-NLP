
# 📊 Rapport d'entraînement et d'évaluation

## 🔍 **Résultats**
- **Accuracy** : 0.8885
- **F1-score (macro)** : 0.4820
- **Precision globale** : 0.8857
- **Recall global** : 0.4258
- **Loss finale** : 0.3146

## ⚙️ **Hyperparamètres**
- **Epochs** : 5
- **Batch Size (train / eval)** : 8 / 8
- **Gradient Accumulation Steps** : 2
- **Learning Rate** : 5e-05
- **Weight Decay** : 0.05
- **Warmup Steps** : 2000
- **Scheduler** : SchedulerType.LINEAR
- **Max Grad Norm** : 1
- **Seed** : 123
- **LoRA Config** : r=8, alpha=16, dropout=0.1

## 🧠 **Tokenizer**
- **Tokenizer utilisé** : distilbert-base-uncased

## ⏱ **Temps d'entraînement**
- **Temps moyen par époque** : 212.582 sec

## 🌱 **Empreinte carbone**
- **CO₂ estimé** : 0.0059 kg

