# ENGR 4170 — Lab 4: From Deep Neural Networks to CNNs

## 1. Engineering Problem

This laboratory investigates automated visual classification of steel surface defects using deep learning. Two neural-network architectures, a deep fully connected neural network (DNN) and a convolutional neural network (CNN), are trained and evaluated on the same six-class classification problem.

The objective is to compare their performance and understand the advantages of CNNs in processing image data.

All images in this dataset represent defects. The models classify the type of defect, rather than determining whether a manufactured component is acceptable or defective.

## 2. Dataset and Preprocessing

The instructor-provided NEU Surface Defect Dataset contains 1,800 grayscale images, with 300 images for each of the following six classes:

- Crazing
- Inclusion
- Patches
- Pitted Surface
- Rolled-in Scale
- Scratches

Images are converted into single-channel tensors and resized to 64 × 64 pixels using PyTorch.

The dataset is divided into:

| Dataset | Images | Percentage |
|---|---:|---:|
| Training | 1,260 | 70% |
| Validation | 270 | 15% |
| Testing | 270 | 15% |

The random split uses seed 42, and the training batch size is 32.

## 3. Deep Neural Network (DNN)

The fully connected network uses the following architecture:

**4096 → 256 → 64 → 6**

The input images are flattened into 4,096 pixel values before entering the network. Two hidden layers use ReLU activation functions, followed by a six-neuron output layer.

Training settings:

- Loss function: Cross-Entropy Loss
- Optimizer: Adam
- Learning rate: 0.001
- Epochs: 20
- Trainable parameters: 1,065,670

Flattening removes the explicit spatial arrangement of pixels, which limits the network's ability to efficiently recognize local image patterns.

## 4. Convolutional Neural Network (CNN)

The CNN uses three convolutional layers, followed by a fully connected classifier.

Architecture:

- Convolution 1: 16 filters, 3 × 3 kernel
- Convolution 2: 32 filters, 3 × 3 kernel
- Convolution 3: 64 filters, 3 × 3 kernel
- ReLU activation and 2 × 2 max pooling after each convolution
- Fully connected classifier: 4096 → 128 → 6

Training settings:

- Loss function: Cross-Entropy Loss
- Optimizer: Adam
- Learning rate: 0.001
- Epochs: 20
- Trainable parameters: 548,486

Unlike the DNN, the CNN uses local feature extraction and weight sharing to identify spatial features such as edges, cracks, and textures.

## 5. Model Evaluation

### 5.1 Model Comparison

| Model | Final Validation Accuracy | Test Accuracy | Parameters |
|---|---:|---:|---:|
| Deep NN | 46.67% | 48.52% | 1,065,670 |
| CNN | 92.59% | 92.59% | 548,486 |

The CNN achieved 44.07 percentage points higher test accuracy while using approximately 48.5% fewer trainable parameters.

### 5.2 CNN Classification Performance

| Defect Class | Precision | Recall | F1-Score |
|---|---:|---:|---:|
| Crazing | 0.98 | 0.96 | 0.97 |
| Inclusion | 0.87 | 0.80 | 0.84 |
| Patches | 1.00 | 1.00 | 1.00 |
| Pitted Surface | 0.84 | 0.92 | 0.88 |
| Rolled-in Scale | 0.95 | 0.98 | 0.96 |
| Scratches | 0.93 | 0.88 | 0.90 |

The CNN correctly classified 250 out of 270 test images, achieving an overall test accuracy of 92.59%.

## 6. Error Analysis

The confusion matrix indicates that inclusion and pitted surface are the most frequently confused classes.

Eight inclusion images were incorrectly classified as pitted surface, while two pitted surface images were classified as inclusion. These defects can have similar dark and irregular patterns, making them difficult to distinguish at reduced image resolution.

One notable example was `In_251.bmp`, which was incorrectly classified as pitted surface with 98.58% confidence.

This demonstrates that high prediction confidence does not necessarily guarantee a correct classification.

## 7. Additional CNN Experiment

An additional experiment was performed by increasing the number of convolutional filters from 16/32/64 to 24/48/96.

The hypothesis was that additional filters might improve feature extraction and validation performance, but could also increase computational complexity and overfitting.

### Experimental Results

| Model | Validation Accuracy | Test Accuracy | Parameters |
|---|---:|---:|---:|
| Original CNN | 92.59% | 92.59% | 548,486 |
| Modified CNN | 92.96% | 89.63% | 839,558 |

The modified CNN achieved slightly higher validation accuracy but 2.96 percentage points lower test accuracy.

This suggests that increasing model complexity does not necessarily improve generalization. The comparison is based on one training run for each architecture.

## 8. Interactive Application

A small interactive application was developed using Gradio in Part VI of `lab04.ipynb`.

The application accepts an uploaded steel surface image, applies the same preprocessing pipeline used during training, and returns classification scores for all six defect categories.

The application is implemented directly inside the notebook rather than in a separate `app.py` file.

### Running the Application

1. Open `lab04.ipynb` using Google Colab.
2. Upload the instructor-provided `NEU-CLS.rar` dataset archive to `/content/`.
3. Execute the notebook cells from the beginning to extract the images, preprocess the dataset, and train the models.
4. Navigate to Part VI and execute the Gradio application cell.
5. Upload a steel surface image to view its predicted defect classification and confidence scores.

The application requires the trained CNN model to be available in the active Colab runtime.

Main Python libraries include PyTorch, Torchvision, NumPy, Matplotlib, Scikit-learn, Pandas, Pillow, and Gradio.

## 9. Repository Structure

```text
deep-learning-labs/
|
+-- lab04/
    +-- lab04.ipynb
    +-- figures/
    +-- README.md
```

The notebook includes the implementation, training and validation curves, classification results, confusion matrix, error analysis, Gradio application, additional experiment, and laboratory question answers.

The `figures/` directory contains exported visualizations of the model performance.

The main evaluation results are retained directly in the executed notebook.

The original NEU dataset and trained model checkpoints are not included in the repository. The notebook can be rerun to reproduce the workflow, although training results may vary between executions.

## 10. Engineering Conclusion

The CNN significantly outperformed the fully connected DNN for steel surface defect classification, demonstrating the advantages of preserving spatial information and using shared convolutional filters.

However, the achieved 92.59% test accuracy does not provide sufficient evidence for fully automated industrial quality-control decisions.

The classifier was trained exclusively on defective surface images and cannot distinguish acceptable components from defective ones. High-confidence classification errors were also observed.

Before industrial deployment, additional testing would be required using representative manufacturing data, including acceptable components, different lighting conditions, surface finishes, and defect variations. Further evaluation of confidence calibration and the consequences of incorrect predictions would also be necessary.

Human review would remain appropriate until the system demonstrates sufficient reliability under realistic production conditions.
