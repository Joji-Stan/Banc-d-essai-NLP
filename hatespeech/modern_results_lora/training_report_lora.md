
# 📊 Rapport d'entraînement et d'évaluation

## 🔍 **Résultats**
- **Accuracy** : 0.8597
- **F1-score (macro)** : 0.2311
- **Precision globale** : 0.9649
- **Recall global** : 0.25
- **Loss finale** : 0.4783

## ⚙️ **Hyperparamètres**
- **Epochs** : 3
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
- **Tokenizer utilisé** : ModernBERT-base

## ⏱ **Temps d'entraînement**
- **Temps moyen par époque** : 1150 sec

## 🌱 **Empreinte carbone**
- **CO₂ estimé** : 0.0299 kg

