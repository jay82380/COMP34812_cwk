# COMP34812 Shared Task — Category C: Transformer NLI

This repository contains the Category C (transformer) submission for Track A: Natural Language Inference in the COMP34812 shared task (AY 2025–26).

**Author:** Jay Parekh & Matthew Cook

## Repository Structure

| File | Description |
|------|-------------|
| `MoE_POS_Synthetic_Ensemble_REAL.ipynb` | Training/Evaluate notebook -- trains the model on `train.csv`, evaluates it on `dev.csv` and saves it|
| `demo_code.ipynb` | Demo/inference notebook -- produces predictions on an unlabelled `test.csv` |
| `model_card.md` | Model card for the submission |

## Trained Model

The trained model bundle (`nli_esim_plus_bundle.pt`) is stored on OneDrive:

[nli_esim_plus_bundle.pt](https://livemanchesterac-my.sharepoint.com/:u:/g/personal/mateusz_wojcieszyk_student_manchester_ac_uk/IQC51O_qiy0yTbi9D-YCIxYLAWy7S7AykQ726iLX15iScfg)

The bundle contains model weights, vocabulary, configuration, decision threshold, and training metadata. It is loaded by both the evaluation and demo notebooks.

## Data Sources

- **Training and development data:** Coursework-provided `train.csv` and `dev.csv` splits for the NLI track. No external labelled or unlabelled task datasets were used.
- **Pretrained word embeddings:** [GloVe](https://nlp.stanford.edu/projects/glove/) (`glove-wiki-gigaword-100`), loaded via [Gensim](https://radimrehurek.com/gensim/). Used for embedding initialisation only.

## Code and Architecture Attribution

- **ESIM architecture:** Chen, Q., Zhu, X., Ling, Z., Inkpen, D., and Wei, S. (2017). *Enhanced LSTM for Natural Language Inference.* Proceedings of ACL 2017. https://aclanthology.org/P17-1152/
- **R-Drop regularization:** Liang, X., Wu, L., Li, J., Wang, L., and Long, M. (2021). *R-Drop: Regularized Dropout for Neural Networks.* NeurIPS 2021.
- **SWA-style model averaging:** Talman, A., Yli-Jyrä, A., and Tiedemann, J. (2023). *Uncertainty-Aware Natural Language Inference with Stochastic Weight Averaging.*


- **Transformer Backbones:** https://arxiv.org/abs/2006.03654 
- **Mixture oF Experts (MoE):** https://arxiv.org/abs/1701.06538
- **Linguistically-Informed/ POS-Specialised Models**  https://arxiv.org/abs/1804.08199  https://arxiv.org/abs/1809.05724
- **Data Augmentation (T5):** https://arxiv.org/abs/1910.10683
- **Ensembling:**        -  https://web.engr.oregonstate.edu/~tgd/publications/mcs-ensembles.pdf https://arxiv.org/abs/1912.02757
- **Model card format:** Mitchell, M. et al. (2019). *Model Cards for Model Reporting.*
- **Model card template and creation notebook:** Provided as part of the COMP34812 coursework materials.

## How to Run

All notebooks are designed for Google Colab with a GPU runtime.

1. **Train:** Upload `train.csv` and `dev.csv` under training_data/NLI/ to the Colab session, then run `MoE_POS_Synthetic_Ensemble_REAL.ipynb`. This produces `nli_esim_plus_bundle.pt`.
3. **Predict:** Upload `test.csv` and the saved bundle, then run `demo_code.ipynb`. This outputs `Group_n_B.csv` with a single `prediction` column.