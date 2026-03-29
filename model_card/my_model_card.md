---
{}
---
language: en
license: cc-by-4.0
tags:
- natural-language-inference
- transformers
- text-classification
repo: https://github.com/username/project_name

---

# Model Card for group-13-devberta-NLI

<!-- Provide a quick summary of what the model is/does. -->

 This model performs Natural Language Inference (NLI), predicting whether
    a given hypothesis is entailed by a premise (binary classification: 0 or 1)


## Model Details

### Model Description

<!-- Provide a longer summary of what this model is. -->

 This model is a Mixture of Experts (MoE) architecture built on top of a DeBERTa-v3 backbone.
    It processes premise-hypothesis pairs and combines multiple representations:

    - A semantic expert using the full sentence pair
    - An entity expert focusing on nouns and proper nouns
    - An action expert focusing on verbs
    - A logic expert using handcrafted logical features

    Each expert produces a representation which is combined via a learned gating mechanism.
    The final prediction is produced from a weighted combination of these expert outputs.
    

- **Developed by:** Jay Parekh and Matthew Cook
- **Language(s):** English
- **Model type:** Supervised
- **Model architecture:** Ensemble MoE Transformers (DeBERTa-v3)
- **Finetuned from model [optional]:** microsoft/deberta-v3-small , answerdotai/ModernBERT-base

### Model Resources

<!-- Provide links where applicable. -->

- **Repository:** https://github.com/microsoft/DeBERTa , https://github.com/answerdotai/modernbert
- **Paper or documentation:** https://arxiv.org/abs/2111.09543 , https://arxiv.org/abs/2412.13663

## Training Details

### Training Data

<!-- This is a short stub of information on the training data that was used, and documentation related to data pre-processing or additional filtering (if applicable). -->

roughly 24,000 labelled premise-hypothesis pairs provided part of the shared task dataset
    Labels are binary (0 = not entailed, 1 = entailed).
    The dataset is approximately balanced across both classes
    

### Training Procedure

<!-- This relates heavily to the Technical Specifications. Content here should link to that section when it is relevant to the training procedure. -->

#### Training Hyperparameters

<!-- This is a summary of the values of hyperparameters used in training the model. -->


      - deberta learning rate: 3e-05,
      - deberta batch size: 16,
      - deberta epochs: 12,
      - modernbert learning rate: 3e-05,
      - modernbert batch size: 8,
      - modernbert epochs: 8
      - max_sequence_length: 128
    

#### Speeds, Sizes, Times

<!-- This section provides information about how roughly how long it takes to train the model and the size of the resulting model. -->


      - overall training time: 40
      - duration per training epoch (deberta): ~1.5 minutes
      - duration per training epoch (modernbert): ~3 minutes
      - model size: ~550MB (deberta), ~575MB (modernbert)

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->

### Testing Data & Metrics

#### Testing Data

<!-- This should describe any evaluation data used (e.g., the development/validation set provided). -->

Evaluation was performned on the provided developmet dataset (roughly 6,700 examples)

#### Metrics

<!-- These are the evaluation metrics being used. -->


      - Precision
      - Recall
      - F1-score
      - Accuracy

### Results


    The model acheived approximately:
      - Accuracy (deberta): 0.87
      - Accuracy (modernbert): 0.68
      - F1-score (deberta): 0.87
      - F1-score (modernbert): 0.88
      The ensemble of both models achieved an accuracy of 0.88 and an F1-score of 0.88 on the development set.

## Technical Specifications

### Hardware


      The model was trained on the following hardware (however is not the minimum requirement for inference):
      - RAM: >= 8GB
      - Storage: at least 2GB,
      - GPU: Optional (recommended for faster training)

### Software


      - Python 3.10+
      - PyTorch
      - Transformers
      - Datasets
      - Spacy
      - pandas 
      - numpy
      - scikit-learn
      

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->


      - The model may struggle with ambiguous or linguitically complex sentence pairs
      - Performance may degrade on out-of-domain data
      - Inputs longer than 128 tokens are truncated, potentially losing information
      - The model may inherit biases present in the training data
      

## Additional Information

<!-- Any other information that would be useful for other people to know. -->


      - The model was trained using a custom PyTorch training loop for stability
      - A small-scale overfitting test was performned to validate correctness of the training pipeline
      - The hyperparameters were determined by experimentation with different values.
