---
language: en
license: cc-by-4.0
tags:
- natural-language-inference
- text-classification
- bilstm
- esim
- non-transformer
repo: https://github.com/jay82380/COMP34812_cwk/tree/category_b
---

---

# Model Card for nli-esim-plus-category-b

<!-- Provide a quick summary of what the model is/does. -->

This model performs binary Natural Language Inference (NLI), predicting whether a given hypothesis is supported by a premise.


## Model Details

### Model Description

<!-- Provide a longer summary of what this model is. -->

This model is a non-transformer NLI classifier developed for the COMP34812 shared task (AY 2025–26), Track A: Natural Language Inference. It is based on an ESIM-style BiLSTM architecture with soft alignment, local inference matching, a second BiLSTM composition layer, gated pooling, and sentence-level interaction features. In the strongest final run, R-Drop regularization and SWA-style model averaging were enabled.

- **Developed by:** Mateusz Wojcieszyk
- **Language(s):** English
- **Model type:** Supervised binary text-pair classifier
- **Model architecture:** ESIM-style BiLSTM with soft alignment, local inference matching, second BiLSTM composition, gated pooling, and sentence-level interaction features
- **Finetuned from model [optional]:** Not applicable. The model was trained on the coursework dataset only, with pretrained static word embeddings used for initialization.

### Model Resources

<!-- Provide links where applicable. -->

- **Repository:** https://github.com/jay82380/COMP34812_cwk/tree/category_b
- **Paper or documentation:**
  - Chen, Q., Zhu, X., Ling, Z., Inkpen, D., and Wei, S. (2017). *Enhanced LSTM for Natural Language Inference.*
  - Liang, X., Wu, L., Li, J., Wang, L., and Long, M. (2021). *R-Drop: Regularized Dropout for Neural Networks.*
  - Talman, A., Yli-Jyrä, A., and Tiedemann, J. (2023). *Uncertainty-Aware Natural Language Inference with Stochastic Weight Averaging.*
  - Mitchell, M. et al. (2019). *Model Cards for Model Reporting.*

## Training Details

### Training Data

<!-- This is a short stub of information on the training data that was used, and documentation related to data pre-processing or additional filtering (if applicable). -->

Training used only the coursework-provided `train.csv` split for the NLI track. Development evaluation used only the coursework-provided `dev.csv` split. No additional labelled or unlabelled task datasets were used.

Pretrained static word embeddings were used.

- **Training examples:** 24,432
- **Development examples:** 6,736
- **Training label distribution:** `1: 12,648`, `0: 11,784`
- **Development label distribution:** `1: 3,478`, `0: 3,258`

Text was lowercased, tokenised with a lightweight regex tokenizer that keeps punctuation as separate tokens, truncated to a maximum length of 128 tokens per sequence, and converted to token IDs using a vocabulary built from the coursework training data only.

Pretrained static embeddings (`glove-wiki-gigaword-100` via Gensim) were used as initialization only. Vocabulary size was 22,533 and embedding coverage over the vocabulary was approximately 0.9627.

### Training Procedure

<!-- This relates heavily to the Technical Specifications. Content here should link to that section when it is relevant to the training procedure. -->

The model was trained with `BCEWithLogitsLoss` and AdamW. Development macro-F1 was used for early stopping and best-checkpoint selection.

The strongest run also used two lightweight training improvements:

- **R-Drop**, implemented as consistency regularization over two dropout-enabled forward passes
- **SWA-style averaging**, applied in later training epochs

#### Training Hyperparameters

<!-- This is a summary of the values of hyperparameters used in training the model. -->

- learning_rate: 3e-4
- batch_size: 64
- seed: 42
- max_sequence_length: 128
- min_freq: 2
- max_vocab_size: 50,000
- embedding_backend: gensim
- embedding_name: glove-wiki-gigaword-100
- embedding_dim: 100
- train_embeddings: true
- hidden_size: 192
- dropout: 0.3
- weight_decay: 1e-5
- epochs: 12
- early_stopping_patience: 4
- gradient_clip: 5.0
- use_rdrop: true
- rdrop_alpha: 0.5
- use_swa: true
- swa_start_epoch: 8
- swa_lr: 1e-4

#### Speeds, Sizes, Times

<!-- This section provides information about how roughly how long it takes to train the model and the size of the resulting model. -->

- runtime_device: CUDA GPU when available
- approximate_duration_per_epoch: ~47 seconds on Google Colab GPU
- approximate_total_training_time: ~9–10 minutes for 12 epochs
- best_epoch: 12
- tuned_decision_threshold: 0.54
- saved_artifact: single PyTorch bundle (`nli_esim_plus_bundle.pt`) containing weights, vocab, config, threshold, and training metadata

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->

### Testing Data & Metrics

#### Testing Data

<!-- This should describe any evaluation data used (e.g., the development/validation set provided). -->

For reported labelled evaluation, the coursework `dev.csv` split was used.

#### Metrics

<!-- These are the evaluation metrics being used. -->

- Accuracy
- Macro precision
- Macro recall
- Macro F1
- Matthews correlation coefficient (MCC)
- ROC-AUC
- Binary cross-entropy loss

### Results

Development-set results for the best checkpoint:

- Accuracy: 0.7365
- Macro precision: 0.7371
- Macro recall: 0.7372
- Macro F1: 0.7365
- MCC: 0.4743
- ROC-AUC: 0.8180
- Loss: 0.5193

Class-wise development performance:

- **Label 0**
  - Precision: 0.7144
  - Recall: 0.7584
  - F1: 0.7357

- **Label 1**
  - Precision: 0.7598
  - Recall: 0.7159
  - F1: 0.7372

Confusion matrix on the development set:

- True negatives: 2471
- False positives: 787
- False negatives: 988
- True positives: 2490

## Technical Specifications

### Hardware

- CUDA-enabled GPU in Google Colab recommended for training
- CPU execution is possible but slower

### Software

- Python 3
- PyTorch
- pandas
- numpy
- scikit-learn
- matplotlib
- tqdm
- gensim

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->

- This model was trained only on the coursework dataset, so performance may not generalise to other domains or genres.
- The shared task was run in closed mode, so no external labelled NLI or claim-verification datasets were used.
- Static pretrained embeddings may encode biases from their source corpora.
- Although the model uses attention-based alignment, it remains a non-transformer recurrent architecture and may struggle with long-range dependencies or examples requiring substantial background knowledge.
- Threshold tuning was performed on the development set and may not transfer perfectly to the hidden test set.
- This model is intended for coursework evaluation rather than real-world deployment.

## Additional Information

<!-- Any other information that would be useful for other people to know. -->

Separate notebooks are provided for training, development-set evaluation, and demo inference. The trained model bundle is loaded by the evaluation and demo notebooks.

The trained model is stored on the cloud here:

`https://livemanchesterac-my.sharepoint.com/:u:/g/personal/mateusz_wojcieszyk_student_manchester_ac_uk/IQC51O_qiy0yTbi9D-YCIxYLAWy7S7AykQ726iLX15iScfg`
