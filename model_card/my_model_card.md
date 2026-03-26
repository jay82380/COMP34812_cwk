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

 This model is based on DeBERTa-v3-base and fine-tuned on a dataset of roughly 
    24000 premise-hypothesis pairs. The task is to determine whether the hypothesis logically follows from the premise
    the model takes two input sequences (premise and hypothesis), tokenizes them jointly, and predicts a binary label
    

- **Developed by:** Jay Parekh and Matthew Cook
- **Language(s):** English
- **Model type:** Supervised
- **Model architecture:** Transformers (DeBERTa-v3)
- **Finetuned from model [optional]:** microsoft/deberta-v3-base

### Model Resources

<!-- Provide links where applicable. -->

- **Repository:** https://huggingface.co/microsoft/deberta-v3-base
- **Paper or documentation:** https://huggingface.co/microsoft/deberta-v3-base

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


      - learning_rate: 1e-05
      - train_batch_size: 8
      - eval_batch_size: 8
      - seed: 3
      - max_sequence_length:256
      - num_epochs: 3

#### Speeds, Sizes, Times

<!-- This section provides information about how roughly how long it takes to train the model and the size of the resulting model. -->


      - overall training time: 5 hours
      - duration per training epoch: 30 minutes
      - model size: 400MB

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
      - Accuracy: 0.87-0.9
      - F1-score: 0.86-0.89
    

## Technical Specifications

### Hardware


      - RAM: >= 8GB
      - Storage: at least 2GB,
      - GPU: Optional (recommended for faster training)

### Software


      - Python 3.10+
      - PyTorch
      - HuggingFace Transformers
      - Datasets library
      - scikit-learn
      

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->


      - The model may struggle with ambiguous or linguitically complex sentence pairs 
      - Performance may degrade on out-of-domain data 
      - Inputs longer than 256 tokens are truncated, potentially losing information 
      - The model may inherit biases present in the training data
      

## Additional Information

<!-- Any other information that would be useful for other people to know. -->


      - The model was trained using a custom PyTorch training loop for stability 
      - A small-scale overfitting test was performned to validate correctness of the training pipeline 
      - The hyperparameters were determined by experimentation with different values.
