# LM-attacks

# Exploring LM-attacks

This repository has been created as part of the course "Response & Safe AI Systems" at IIIT Hyderabad in Spring 2024 semester. The aim of this project is to explore the vulnerabilities of language models to adversarial attacks. We will be using small language models for this purpose.

### Team Members:
1. Divij
2. Ananth Muppidi
3. Ashutosh Srivastava
4. Praddyumn Shukla
5. Arnav Mago

### Adversarial Attacks:
1. TextFooler
2. DeepWordBug
3. HotFlip

### Language Models:
1. Roberta-Base-SST2
2. LSTM-MR

### Libraries for models and attacks:
1. HuggingFace
2. TextAttack

## Classifiers for detecting attacks
Our first task was to build classifier to detect attacks like TextFooler. We used dataset marked with adversarial examples and trained a classifier on it. We used Roberta-Base-SST2 model for this purpose. The classifier used for this purpose was Bert-base-uncased. The classifier was able to detect adversarial examples with a 66.67% accuracy.

The main purpose of building this classifier for detecting adversarial examples was for comparison (explained in next section)

## Interpretability
We use probing classifiers to analyze the internal represntation of the models while under adversarial attack. We used simple Logistic Regression model to probe. The training data contained 449 examples of adversarial examples and 449 examples of clean examples. The results for this can be seen through the diagram below:

![alt text](image.png)

We also train probing classifiers on the same dataset but with the Canine model instead. Canine is a character level model and hence, it should be largely resistant to how textfooler changes words (based on word semantics). This gives us a base accuracy to compare the scores.

## Comparison of attacks
It's clear from looking at the scores that the model performs better on the classification task of adversarial examples when done using the hidden layer representations. The model, especially, in the later layers makes divergences between the normal and adversarial examples. We conclude that even when training something like an adversarial shields, it becomes more effective when done using the hidden state representations.

## Steering
We use the embedding representation obtained previously along with the knowledge obtained via probing that information regarding adversarial examples is best encoded by the 12th layer for creating a steering vector. We take mean over all normal and adversarial examples seperately, and then get the steering vector as:

Steering vector = mean(x_adv) - mean(x_normal)

After obtaining the steering vector, we check it's validity by applying it to various kinds of inputs. Since, we do not have a way to use the 12th layer's representation for inference, we instead train a classifier on the dataset to classify whether an input is adversarial or not. Since this classifier has a high accuracy (around 80-85%), we use it as a substitute to check if our steering vector is working.
