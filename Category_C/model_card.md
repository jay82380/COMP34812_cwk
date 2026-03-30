---
language: en
license: cc-by-4.0
tags:
- natural-language-inference
- transformers
- text-classification
- ensemble
- mixture-of-experts
repo: https://github.com/jay82380/COMP34812_cwk/tree/category_C
---

# Model Card for group-13-devberta-NLI

This model performs Natural Language Inference (NLI), predicting whether a given hypothesis is entailed by a premise (binary classification: 0 or 1).

## Model Details

### Model Description

This model is an ensemble Mixture-of-Experts (MoE) architecture for Natural Language Inference (NLI), built on top of transformer backbones (DeBERTa-v3-small and ModernBERT).

The system processes premise–hypothesis pairs and combines multiple specialised representations:

- A semantic expert operating on the full sentence pair  
- An entity expert focusing on nouns and proper nouns  
- An action expert focusing on verbs  
- A logic expert incorporating handcrafted features such as negation and lexical overlap  

Each expert produces a representation which is combined via a learned gating mechanism (MoE), enabling conditional computation and specialisation.

To improve generalisation, predictions from DeBERTa-v3 and ModernBERT are ensembled.

Additionally, a T5-based text generation model is used to augment training data through paraphrasing, increasing robustness to linguistic variation.

The final prediction is obtained via a weighted combination of expert outputs and ensemble predictions.

- **Developed by:** Jay Parekh and Matthew Cook  
- **Language(s):** English  
- **Model type:** Supervised  
- **Model architecture:** Ensemble Transformer-based Mixture-of-Experts (DeBERTa-v3 + ModernBERT)  
- **Finetuned from:** microsoft/deberta-v3-small, answerdotai/modernbert-base  

### Model Resources

- **Repository:** https://github.com/microsoft/DeBERTa, https://github.com/answerdotai/modernbert  
- **Paper or documentation:** https://arxiv.org/abs/2006.03654, https://arxiv.org/abs/2412.13663  

## Training Details

### Training Data

Approximately 24,000 labelled premise–hypothesis pairs from the provided NLI dataset.

Labels are binary:
- 0 = not entailed  
- 1 = entailed  

The dataset is approximately balanced across both classes.

To improve robustness, additional synthetic training examples were generated using a T5-based text generation model, introducing variation in phrasing while preserving the original labels.

### Training Procedure

#### Training Hyperparameters
- Data Augmentation Model: T5
- Data Augmentation Implementation: t5-base
- Data Augmentation Sample Size: 1000
- Data Augmentation Generation Size: 64

- DeBERTa Implementation: deberta-v3-small
- DeBERTa Learning Rate: 3e-05  
- DeBERTa Batch Size: 16  
- DeBERTa Epochs: 12  
- DeBERTa Max Sequence Length: 128

- ModernBERT Implementation: ModernBERT-base
- ModernBERT Learning Rate: 3e-05  
- ModernBERT Batch Size: 8  
- ModernBERT Epochs: 8  
- ModernBERT Max Sequence Length: 128

- MoE Expert Count: 4
- MoE Gating Layer Size: 128
- MoE Gating Activation Function: ReLU
- MoE Activation Function: tanh
- MoE Normalisation: LayerNorm

#### Speeds, Sizes, Times

- Total training time: ~91 minutes  
- Duration per epoch (DeBERTa): ~3.6 minutes  
- Duration per epoch (ModernBERT): ~7.7 minutes  
- Model size: ~550MB (DeBERTa), ~575MB (ModernBERT)  

## Evaluation

### Testing Data & Metrics

#### Testing Data

Evaluation was performed on the provided development dataset (approximately 6,700 examples).

#### Metrics

- Precision  
- Recall  
- F1-score  
- Accuracy  

### Results

Performance on the development set:

**DeBERTa-v3**
- Accuracy: 0.88  
- F1-score: 0.87  

**ModernBERT**
- Accuracy: 0.88  
- F1-score: 0.88  

**Ensemble (DeBERTa + ModernBERT)**
- Accuracy: 0.90
- F1-score: 0.90 

The ensemble improves overall performance, demonstrating complementary strengths between the two backbone models.

## Technical Specifications

### Hardware

The model was trained on the following hardware:
- RAM: 32GB  
- GPU: RTX 4080
- CPU: i9-13900

Inference Recommended Specs (based on Google Colab run):
- RAM: 12GB
- GPU: NVIDIA T4
- CPU: Intel Xeon

### Software

- Python 3.10+  
- PyTorch  
- Transformers  
- Datasets  
- spaCy  
- pandas  
- numpy  
- scikit-learn  

## Bias, Risks, and Limitations

- The model may struggle with highly ambiguous or complex linguistic structures  
- Performance may degrade on out-of-domain data  
- Inputs longer than 128 tokens are truncated, potentially losing contextual information  
- The model may inherit biases present in the training data  
- Handcrafted logic features (e.g., negation detection) may not generalise to all linguistic constructions  

## Additional Information

- The model combines neural and linguistic features through a Mixture-of-Experts framework  
- POS tagging is performed using spaCy to extract structured linguistic signals  
- A small-scale overfitting test was conducted to validate the correctness of the training pipeline  
- Hyperparameters were selected empirically based on validation performance  
