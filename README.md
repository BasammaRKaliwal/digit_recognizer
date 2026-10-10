# Week 1: Deep Learning (Skill Nexis)

| File | What it is |
|------|------------|
| `digit_recognizer/assignment1_neural_network.ipynb` | Assignment 1: neurons, tensors, regression training, loss plots, learning rate comparison |
| `digit_recognizer/numerical_pattern_predictor.ipynb` | Mini Project 1: Numerical Pattern Predictor |
| `digit_recognizer/app.py` and related files | Extra project: MNIST digit recognizer with a live drawing page |

---

# Digit Recognizer (MNIST)

A CNN built with PyTorch that recognizes handwritten digits (0-9), with a Flask web page where you draw a digit and get a live prediction.

## Results
- Test accuracy: **99.30%** (9930 of 10000 correct)
- Per-digit precision and recall are printed by `evaluate.py` and saved in `results.json`



![Confusion matrix](digit_recognizer/images/confusion_matrix.png)




![Training curves](digit_recognizer/images/training_curves.png)



## How to run
