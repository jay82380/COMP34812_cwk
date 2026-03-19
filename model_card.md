# Model Card for nli-esim-plus-category-b

This model is a non-transformer natural language inference classifier developed for the COMP34812 shared task (AY 2025–26), Track A: Natural Language Inference. It is based on an ESIM-style BiLSTM architecture with soft alignment, local inference matching, a second BiLSTM composition layer, gated pooling, and sentence-level interaction features. In the strongest final run, R-Drop regularization and SWA-style model averaging were enabled.

## Model Details

### Model Description

The model predicts whether a hypothesis is supported by a premise in the binary NLI setting used in the coursework.

- **Developed by:** [Add full student names here]
- **Language(s):** English
- **Model type:** Supervised binary classifier for pairwise sequence classification
- **Model architecture:** ESIM-style BiLSTM with soft alignment, local inference matching, a second BiLSTM composition layer, learned gated pooling, and sentence-level interaction features
- **Finetuned from model [optional]:** Not applicable. The model was trained on the coursework dataset only, with pretrained static word embeddings used for initialization.

### Model Resources

- **Repository / storage:** [Add OneDrive link or submission location here]
- **Training notebook:** `train_nli_B_model.ipynb`
- **Development evaluation notebook:** `evaluate_nli_B_dev.ipynb`
- **Demo / inference notebook:** `demo_nli_B_predict.ipynb`

### References

- Chen, Q., Zhu, X., Ling, Z., Inkpen, D., and Wei, S. (2017). *Enhanced LSTM for Natural Language Inference.*
- Liang, X., Wu, L., Li, J., Wang, L., and Long, M. (2021). *R-Drop: Regularized Dropout for Neural Networks.*
- Talman, A., Yli-Jyrä, A., and Tiedemann, J. (2023). *Uncertainty-Aware Natural Language Inference with Stochastic Weight Averaging.*  
  In this coursework submission, this paper is used as motivation for a simplified SWA-style averaging procedure rather than a full reproduction of the method.
- Mitchell, M. et al. (2019). *Model Cards for Model Reporting.*

## Training Details

### Training Data

Training used only the coursework-provided `train.csv` split for the NLI track. Development evaluation used only the coursework-provided `dev.csv` split. No additional labelled or unlabelled task datasets were used.

Pretrained static word embeddings were used in line with the coursework clarification that pretrained representations are allowed, provided no external corpora are used for task-specific training.

- **Training examples:** 24,432
- **Development examples:** 6,736
- **Training label distribution:** `1: 12,648`, `0: 11,784`
- **Development label distribution:** `1: 3,478`, `0: 3,258`

Text was lowercased, tokenised with a lightweight regex tokenizer that keeps punctuation as separate tokens, truncated to a maximum length of 128 tokens per sequence, and converted to token IDs using a vocabulary built from the coursework training data only.

### Training Procedure

The model was trained with `BCEWithLogitsLoss` using AdamW optimization. Development performance was monitored during training, and checkpoint selection was based on development macro-F1 after threshold tuning on the development set.

The final run also enabled two lightweight training-side improvements:

- **R-Drop regularization:** implemented as two dropout-enabled forward passes with an added consistency regularization term.
- **SWA-style model averaging:** implemented as stochastic weight averaging over later training epochs.

#### Training Hyperparameters

- **Random seed:** 42
- **Maximum sequence length:** 128
- **Minimum vocabulary frequency:** 2
- **Maximum vocabulary size:** 50,000
- **Embedding backend:** Gensim
- **Pretrained embedding source:** `glove-wiki-gigaword-100`
- **Embedding dimension:** 100
- **Train embeddings:** True
- **Hidden size:** 192
- **Dropout:** 0.3
- **Batch size:** 64
- **Learning rate:** 3e-4
- **Weight decay:** 1e-5
- **Requested epochs:** 12
- **Early stopping patience:** 4
- **Gradient clipping:** 5.0
- **R-Drop enabled:** True
- **R-Drop alpha:** 0.5
- **SWA enabled:** True
- **SWA start epoch:** 8
- **SWA learning rate:** 1e-4

#### Speeds, Sizes, Times

- **Device used:** CUDA-enabled GPU (from notebook config)
- **Best epoch:** 12
- **Best threshold:** 0.50

## Evaluation

### Testing Data & Metrics

#### Testing Data

Evaluation on labelled data was carried out using the coursework-provided development set only.

#### Metrics

The following metrics were tracked:

- Accuracy
- Macro-precision
- Macro-recall
- Macro-F1
- Matthews correlation coefficient (MCC)
- ROC-AUC
- Binary cross-entropy loss

### Results

Development results for the best checkpoint:

- **Accuracy:** 0.7393
- **Macro-precision:** 0.7393
- **Macro-recall:** 0.7396
- **Macro-F1:** 0.7392
- **MCC:** 0.4789
- **ROC-AUC:** 0.8162
- **Loss:** 0.5338

Class-wise development performance:

- **Label 0**
  - Precision: 0.7228
  - Recall: 0.7477
  - F1: 0.7351

- **Label 1**
  - Precision: 0.7558
  - Recall: 0.7315
  - F1: 0.7434

Confusion matrix on the development set:

- **True negatives:** 2436
- **False positives:** 822
- **False negatives:** 934
- **True positives:** 2544

## Technical Specifications

### Hardware

Training and evaluation were run on a CUDA-enabled GPU in Google Colab.

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

- This model was trained only on the coursework dataset, so performance may not generalise to other domains or genres.
- The shared task was run in closed mode, so no external labelled NLI or claim-verification datasets were used.
- Static pretrained embeddings may encode biases from their source corpora.
- Although the model uses attention-based alignment, it remains a non-transformer recurrent architecture and may struggle with long-range dependencies or examples requiring substantial background knowledge.
- Threshold tuning was performed on the development set and may not transfer perfectly to the hidden test set.
- This model is intended for coursework evaluation rather than real-world deployment.

## Additional Information

Separate notebooks are provided for training, development-set evaluation, and demo inference. The trained model bundle is loaded by the evaluation and demo notebooks.

If the trained model is stored on the cloud and can be found here `https://livemanchesterac-my.sharepoint.com/:u:/g/personal/mateusz_wojcieszyk_student_manchester_ac_uk/IQC51O_qiy0yTbi9D-YCIxYLAWy7S7AykQ726iLX15iScfg`

Any use of generative AI tools should be declared in the README in line with the coursework specification.
