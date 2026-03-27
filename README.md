# COMP34812 Shared Task — Category B: Non-Transformer NLI

This repository contains the Category B (non-transformer) submission for Track A: Natural Language Inference in the COMP34812 shared task (AY 2025–26).

**Author:** Mateusz Wojcieszyk

## Repository Structure

| File | Description |
|------|-------------|
| `train_nli_B_model.ipynb` | Training notebook — trains the ESIM+ model and saves the bundle |
| `evaluate_nli_B_dev.ipynb` | Evaluation notebook — loads the saved bundle and evaluates on `dev.csv` |
| `demo_nli_B_predict.ipynb` | Demo/inference notebook — produces predictions on an unlabelled `test.csv` |
| `model_card.md` | Model card for the ESIM+ submission |

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
- **Model card format:** Mitchell, M. et al. (2019). *Model Cards for Model Reporting.*
- **Model card template and creation notebook:** Provided as part of the COMP34812 coursework materials.

## How to Run

All notebooks are designed for Google Colab with a GPU runtime.

1. **Train:** Upload `train.csv` and `dev.csv` to the Colab session, then run `train_nli_B_model.ipynb`. This produces `nli_esim_plus_bundle.pt`.
2. **Evaluate:** Upload `dev.csv` and the saved bundle, then run `evaluate_nli_B_dev.ipynb`.
3. **Predict:** Upload `test.csv` and the saved bundle, then run `demo_nli_B_predict.ipynb`. This outputs `Group_n_B.csv` with a single `prediction` column.
