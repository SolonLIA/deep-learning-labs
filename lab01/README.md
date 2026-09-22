# deep-learning-labs
For ENGR4170U Course

**Google Colab Link**
https://colab.research.google.com/drive/1gwXdS9YbpzZKUBp7iqiJJyU0EKHyWhiJ?authuser=1#scrollTo=ssUvgAduEJ0e

# Lab 1 — Building Your Deep Learning Workbench

## Objective

The objective of this laboratory was to set up and explore a basic deep-learning workflow using GitHub, Google Colab, PyTorch, and Hugging Face. The laboratory included checking the Python environment, using a GPU, performing basic tensor operations, loading a pretrained image-classification model, and testing the model on several images.

## Tools Used

- Python
- Google Colab
- GitHub
- PyTorch
- NumPy
- Pandas
- Matplotlib
- Hugging Face Transformers
- PIL

## Model Used

The pretrained model used in this laboratory was:

`google/vit-base-patch16-224`

This is a Vision Transformer (ViT) model used for image classification.

## Main Result

The pretrained model was successfully loaded and used to perform image-classification inference on several test images. The model produced reasonable predictions for familiar objects, but the confidence and correctness of the predictions varied depending on the image.

For example, an image of Elizabeth Tower (commonly referred to as Big Ben) was classified as "bell cote, bell cot" with a score of approximately 0.872. This prediction was related to the object but was not a fully accurate identification.

## Interesting Failure

A fictional science-fiction spacecraft was used as a failure-case test. The model predicted "airship, dirigible" as its highest-scoring class with a score of approximately 0.1436, followed by several unrelated classes such as sea slug, nematode, chiton, and jellyfish.

The low and widely distributed prediction scores suggest that the model was uncertain because the fictional spacecraft did not closely match the object classes available in its training label set.
