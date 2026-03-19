# README — Solution B (Non-Transformer NLI: ESIM-style model)

## Overview

This folder contains our **Category B** solution for the COMP34812 Natural Language Understanding shared task, Track A: **Natural Language Inference (NLI)**.

Our solution is a **non-transformer deep learning model** based on an **ESIM-style BiLSTM architecture**. The implementation uses:

- pretrained static word embeddings (GloVe via Gensim),
- BiLSTM sequence encoding,
- soft cross-sentence alignment,
- local inference matching,
- a second BiLSTM composition layer,
- pooled classification,
- and extensions such as gated pooling / sentence-level interaction features

The notebooks are structured so that **training**, **development-set evaluation**, and **demo/inference** are separated.

---

## File structure

- `train_nli_B_model.ipynb`
- `evaluate_nli_B_dev.ipynb`
- `demo_nli_B_predict.ipynb`
- `model_card_nli_esim.md`

## TO DO

- `README.md`
- `Group_n_B.csv` (predictions on the hidden test set)  ## TO DO

Link to trained model:

`https://livemanchesterac-my.sharepoint.com/:u:/g/personal/mateusz_wojcieszyk_student_manchester_ac_uk/IQC51O_qiy0yTbi9D-YCIxYLAWy7S7AykQ726iLX15iScfg`

---

## What each notebook does

### 1. `train_nli_B_model.ipyn`

This notebook:

- loads the coursework `train.csv` (and, if applicable in the notebook, uses `dev.csv` internally for checkpoint selection / threshold tuning),
- preprocesses the data,
- builds the vocabulary,
- loads pretrained embeddings,
- defines and trains the ESIM-style model,
- saves the trained model bundle for later reuse.

Expected output:

- `nli_esim_plus_bundle.pt` (or the equivalent saved model bundle used by the notebook)

### 2. `evaluate_dev_nli_esim.ipynb`

This notebook:

- loads the saved model bundle,
- loads `dev.csv`,
- rebuilds the model from the saved bundle/configuration,
- evaluates the model on the development set,
- reports metrics such as accuracy, macro-F1, MCC, ROC-AUC,
- may also generate plots, a confusion matrix, and baseline comparison outputs if enabled.

### 3. `predict_nli_esim.ipynb`

This notebook is the **demo/inference notebook** required by the coursework. It:

- loads the saved model bundle,
- takes an input CSV file containing the NLI instances,
- runs the model in inference mode,
- writes predictions to a CSV file with a single column called `prediction`.

---

## How to run the notebooks

These notebooks were designed to be runnable in **Google Colab** or a local Jupyter environment with Python 3 and the required packages installed.

### Step 1: Train the model

Run:

- `train_nli_B_model.ipynb`

This produces the saved model bundle (for example `nli_esim_plus_bundle.pt`).

### Step 2: Evaluate on the development set

Run:

- `evaluate_nli_B_dev.ipynb`

Required inputs:

- the saved model bundle from step 1
- `dev.csv`

### Step 3: Generate predictions for the test set

Run:

- `demo_nli_B_predict.ipynb`

Required inputs:

- the saved model bundle from step 1
- the provided hidden test CSV

Required output format:

- a CSV with exactly one column named `prediction`

Final prediction filename for this solution:

- `Group_n_B.csv` ## TO DO

Replace `n` with the actual group number from Canvas. ## TO DO

---

## Data sources used

This system was developed for the COMP34812 shared task in **closed mode**.

### Coursework data

Only the coursework-provided data were used for model development and evaluation:

- `train.csv`
- `dev.csv`
- hidden test CSV (for final prediction generation only)

### Pretrained representations

In line with the coursework clarification, we used **pretrained word embeddings**:

- GloVe embeddings loaded via Gensim (for example `glove-wiki-gigaword-100` or `glove-wiki-gigaword-300`, depending on the final run)

No additional labelled or unlabelled task datasets were used.

---

## Attribution and reused resources

### Research inspiration

The main architectural inspiration for this solution is:

- Chen et al. (2017), *Enhanced LSTM for Natural Language Inference (ESIM)*

Additional reporting inspiration:

- Mitchell et al. (2019), *Model Cards for Model Reporting*
- Liang et al. (2021), *R-Drop: Regularized Dropout for Neural Networks* — this is the paper for the “two dropout forward passes + consistency regularization” idea.
- Talman et al. (2023) — motivation for applying stochastic weight averaging in NLI; our system uses a simplified SWA-style checkpoint averaging approach.

### Code attribution

Inspired from: https://github.com/dunesand/Text-Matching-based-on-ESIM-model/blob/master/esim_model.py
the implementation of Chen et al. (2017), *Enhanced LSTM for Natural Language Inference (ESIM)

**Important:** any reused code bases or snippets should be declared here to comply with the coursework requirements.
n/a
---

## Trained model storage (OneDrive)

**OneDrive link to trained model bundle:**  
`https://livemanchesterac-my.sharepoint.com/:u:/g/personal/mateusz_wojcieszyk_student_manchester_ac_uk/IQC51O_qiy0yTbi9D-YCIxYLAWy7S7AykQ726iLX15iScfg`

Recommended file stored on OneDrive:

- `nli_esim_plus_bundle.pt`

---

## Use of Generative AI Tools

- Generative AI tools were used for limited assistance with drafting documentation, debugging, and code explanation.
- All final implementation decisions, testing, and submission preparation were reviewed and edited by us the team #TO DO
