# COMP34812 Shared Task — Category C: Transformer NLI

This repository contains the Category C (transformer) submission for Track A: Natural Language Inference in the COMP34812 shared task (AY 2025–26).

**Author:** Jay Parekh & Matthew Cook

## Overview 
We propose an ensemble Mixture-of-Experts (MoE) architecture built on top of transformer backbones (DeBERTa-v3 and ModernBERT) 

The model combines:
- Transformer representations (semantic understanding)
- POS-specialised experts (entities and actions)
- Handcrafted logical features (negation + lexical overlap)
- A learned gating mechanism (MoE)
- Model ensembling for improved generalisation
- T5-based data augmentation for robustness

## Repository Structure

| File | Description |
|------|-------------|
| `train_nli_C_model.ipynb` | Training notebook -- trains the model on `train.csv`, evaluates it on `dev.csv` and saves it|
| `evaluate_nli_C_dev.ipynb` | Evaluation notebook -- loads the saved bundle and evaluates on `dev.csv` |
| `demo_nli_C_predict.ipynb` | Demo/inference notebook -- produces predictions on an unlabelled `test.csv` |
| `model_card.md` | Model card for the submission |

## Trained Model

The trained model bundle (`nli_esim_plus_bundle.pt`) is stored on OneDrive:

[nli_esim_plus_bundle.pt](https://livemanchesterac-my.sharepoint.com/:u:/g/personal/mateusz_wojcieszyk_student_manchester_ac_uk/IQC51O_qiy0yTbi9D-YCIxYLAWy7S7AykQ726iLX15iScfg)

The bundle contains model weights, vocabulary, configuration, decision threshold, and training metadata. It is loaded by both the evaluation and demo notebooks.

## Data Sources

- **Training and development data:** Coursework-provided `train.csv` and `dev.csv` splits for the NLI track. No external labelled or unlabelled task datasets were used.

## Code and Architecture Attribution

The system consists of:

### Transformer Backbones 
- DeBERTa-v3-small 
- ModernBERT-base 

### Mixture of Experts (MoE)
- Semantic expert (full sentence)
- Entity expert (nouns/ proper nouns) 
- Action expert (verbs) 
- Logic expert (negation + lexical overlap)

A gating network dynamically combines expert outputs 

### Ensemble 
- Predictions from both backbones are averaged:
```
final_logits = mean(deberta_logits, modernbert_logits)
```

### Data Augmentation 
- T5 used to generate paraphrased hypotheses


## How to Run

All notebooks are designed for Google Colab with a GPU runtime.

1. **Train:** 
- Upload `train.csv` and `dev.csv` under training_data/NLI/ to the Colab session 
- Then run `train_nli_C_model.ipynb`. 
- This trains both models and saves weights into `nli_ensemble_model/`

2. **Evaluation**
- Upload `dev.csv` under `dev_data/NLI/dev.csv` to the Colab session 
- Then run `evaluate_nli_B_dev.ipynb`
- This computes:
    - Accuracy 
    - Precision 
    - Recall 
    - F1-Score 
    - MCC 
    - Confusion matrix

3. **Inference (Demo Code):** 
- Upload `test.csv` under `test_data/NLI/test.csv` to the Colab sess
- Then run `demo_code.ipynb`
- This outputs `Group_13_C.csv` with a single `prediction` column.


## References

- **Transformer Backbones:** https://arxiv.org/abs/2006.03654, https://arxiv.org/abs/2412.13663  
- **Mixture oF Experts (MoE):** https://arxiv.org/abs/1701.06538
- **Linguistically-Informed/ POS-Specialised Models**  https://arxiv.org/abs/1804.08199  https://arxiv.org/abs/1809.05724
- **Data Augmentation (T5):** https://arxiv.org/abs/1910.10683
- **Ensembling:**  https://web.engr.oregonstate.edu/~tgd/publications/mcs-ensembles.pdf 
- **Model card format:** Mitchell, M. et al. (2019). *Model Cards for Model Reporting.*
- **Model card template and creation notebook:** Provided as part of the COMP34812 coursework materials.