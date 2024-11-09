Here’s a README outline for each of the provided notebooks to clarify the purpose, processes, and outputs:

---

### `face_detection_cropped_dataset.ipynb`

**Purpose:**  
This notebook detects faces in a large dataset of celebrity images, crops each detected face, and organizes them into a new folder structure. This is the first step in creating a cleaned dataset of consistent face images for later processing.

**Main Processes:**
1. **Face Detection and Visualization:** Uses the MTCNN model to detect faces in each image and draws bounding boxes around detected faces.
2. **Face Cropping and Saving:** Crops each detected face and resizes it to 224x224 pixels. Saves these cropped images in a new folder structure under a `cropped` directory, with subdirectories for each celebrity.

**Outputs:**
- **Cropped Face Dataset:** Organized in a folder structure where each celebrity's images are saved in separate folders.
- **Log of Images without Faces:** Lists paths to images where no faces were detected.

---

### `npALLDataa.ipynb`

**Purpose:**  
This notebook loads each cropped face image, preprocesses it, and saves it as a .npy file to streamline data loading in subsequent notebooks, generates face embeddings for each cropped image using a pre-trained FaceNet model. The embeddings represent each face as a feature vector, capturing unique characteristics for classification or clustering tasks(this part is extra to our project)

**Main Processes:**
1. **Image and Label Loading:** Loads all images from the `cropped` dataset, resizes them to 224x224 pixels, and applies preprocessing.
2. **Label Mapping:** Maps each image to its respective celebrity label for identification.
3. **Data Storage:** Saves the processed images and corresponding labels as .npy files for efficient model training and evaluation.
4. **Embedding Extraction:** Uses FaceNet to compute a 128-dimensional embedding for each face.
5. **Embedding Storage:** Saves embeddings to a .npy file for use in training models or as inputs to other classification and clustering algorithms.

**Outputs:**
- **all_images.npy:** Contains preprocessed image data for each cropped face.
- **all_labels.npy:** Holds integer labels corresponding to each image.
- **label_mapping.npy:** Maps integer labels to celebrity names for future use.
 - **face_embeddings.npy:** Contains 128-dimensional embeddings for each face image, generated by FaceNet.


---

### `FaceNetEmbbedder.ipynb`

**Purpose:**  
This notebook 

**Main Processes:**
1. **Image and Label Loading:** Loads all images from the `cropped` dataset, resizes them to 224x224 pixels, and applies preprocessing.
2. **Label Mapping:** Maps each image to its respective celebrity label for identification.
3. **Data Storage:** Saves the processed images and corresponding labels as .npy files for efficient model training and evaluation.
4. **Embedding Extraction:** Uses FaceNet to compute a 128-dimensional embedding for each face.
5. **Embedding Storage:** Saves embeddings to a .npy file for use in training models or as inputs to other classification and clustering algorithms.

**Outputs:**
- **all_images.npy:** Contains preprocessed image data for each cropped face.
- **all_labels.npy:** Holds integer labels corresponding to each image.
- **label_mapping.npy:** Maps integer labels to celebrity names for future use.
 - **face_embeddings.npy:** Contains 128-dimensional embeddings for each face image, generated by FaceNet.

---

### `liveCamera.ipynb`

**Purpose:**  
This notebook captures live video from the camera, detects faces in real-time, and classifies detected faces based on precomputed average embeddings, identifying if they belong to a known celebrity.

**Main Processes:**
1. **Real-time Face Detection:** Detects faces from the live camera feed.
2. **Face Embedding and Classification:** Computes embeddings for detected faces and compares them with saved average embeddings to classify them into known categories.
3. **Unknown Face Handling:** Labels unrecognized faces as unidentified.

**Outputs:**
- **Real-time Classification Output:** Displays each detected face with its classification result on the live video feed.

---

### `inception80%.ipynb`

**Purpose:**  
This notebook trains a deep learning model (using Inception architecture) for classifying celebrity faces. Fine-tunes the InceptionV3 model to classify faces across 105 classes (celebrities).
**Files and Outputs**
  - face_embeddings.npy: Embeddings generated for each image using FaceNet.
  - inception80%.h5: The trained model file, achieving an accuracy of 80%.
  - Training History Graphs: Visualizations of accuracy and loss over epochs for both training and validation.
  - Classification Report: Includes precision, recall, F1-score, and support for each class.


## Model Architecture

1. **Base Model**: InceptionV3 (pre-trained) with top layers removed.
2. **Custom Layers**:
   - Flatten Layer
   - Dense Layer with 1024 neurons, ReLU activation
   - Dropout Layer (20%)
   - Dense Layer with 128 neurons, ELU activation
   - Dropout Layer (10%)
   - Dense Layer with 105 neurons, Softmax activation (for multi-class classification)
3. **Learning Rate Scheduler**: Exponential decay for dynamic learning rate adjustment.

## Training

The model was trained for 100 epochs with early stopping (patience of 5 epochs without improvement in validation accuracy).

### Custom Early Stopping

Implemented a callback to halt training when validation accuracy does not improve after 5 epochs.

## Evaluation

### Loss and Accuracy

The model achieved an accuracy of **80%** on the validation data. The following graphs visualize the training and validation accuracy and loss over epochs:

![Training vs Validation Loss](images/loss.JPG)  
![Training vs Validation Accuracy](images/acc.JPG)

### Precision, Recall, and F1 Score

The classification report includes detailed precision, recall, and F1-score for each class:

```plaintext
Accuracy: 0.7981
Classification Report:
              precision    recall  f1-score   support
           0       0.86      0.90      0.88        40
           1       0.64      0.93      0.76        30
           2       0.88      0.93      0.90        40
           ...
    accuracy                           0.80      3487
   macro avg       0.81      0.79      0.79      3487
weighted avg       0.81      0.80      0.80      3487
```


### Additional Metrics

- **Precision (Weighted)**: 0.81
- **Recall (Weighted)**: 0.80
- **F1 Score (Weighted)**: 0.80

## Usage

### Training

To train the model:

```python
history = model.fit(
    x_train,
    y_train_encoded,
    epochs=100,
    validation_data=(x_test, y_test_encoded),
    batch_size=128,
    callbacks=[early_stop]
)
```

### Evaluation

Evaluate the model with:

```python
evaluation = model.evaluate(x_test, y_test_encoded)
```

### Model Inference

For predictions:

```python
y_pred = model.predict(x_test)
```
**Main Processes:**
1. **Model Training:** Trains an Inception model with a dataset of celebrity face images and their embeddings.
2. **Model Evaluation:** Monitors and evaluates the model's performance, optimizing for a target accuracy of 80%.

**Outputs:**
- **Trained Model (inception80%.h5):** Saved model weights for use in downstream tasks such as embedding calculation or real-time classification.

## Project Overview

This project aims to classify celebrity faces using embeddings generated by a pre-trained InceptionV3 model and fine-tuned for improved accuracy. The generated 128-dimensional embeddings serve as feature vectors, which aid in the classification and identification of each face.

## Key Objectives

1. **Embedding Generation:** 
   - Utilizes a FaceNet model to generate 128-dimensional embeddings for each face.
2. **Embedding Storage:** 
   - Saves embeddings in `.npy` format for reuse in classification and clustering tasks.
3. **Model Training:** 
   - 
## Files and Outputs

- **face_embeddings.npy**: Embeddings generated for each image using FaceNet.
- **inception80%.h5**: The trained model file, achieving an accuracy of 80%.
- **Training History Graphs**: Visualizations of accuracy and loss over epochs for both training and validation.
- **Confusion Matrix**: A heatmap illustrating true vs. predicted labels.
- **Classification Report**: Includes precision, recall, F1-score, and support for each class.

## Data Preprocessing

The dataset contains 17,431 images, each resized to \(224 \times 224\) with 3 color channels. The labels were one-hot encoded for classification across 105 classes.



## Repository Structure

```plaintext
CelebrityFaceClassification/
├── all_images.npy              # Array of celebrity face images
├── all_labels.npy              # Labels for each face
├── face_embeddings.npy         # Face embeddings generated by FaceNet
├── inception80%.h5             # Trained model with 80% accuracy
├── history_plot.png            # Accuracy and loss graph
├── confusion_matrix.png        # Confusion matrix heatmap
├── README.md                   # Project documentation
└── scripts/
    ├── data_preprocessing.py   # Data processing and augmentation
    ├── train_model.py          # Model training and evaluation
    ├── evaluate_metrics.py     # Code for classification report and metrics
    └── plot_visualizations.py  # Plots for training history and confusion matrix
```

## Conclusion

This model demonstrates a promising approach to celebrity face classification, with 80% accuracy and substantial precision, recall, and F1 scores across 105 classes. Fine-tuning the model and incorporating additional data could improve classification performance even further.


### `MMO1.ipynb`

**Purpose:**  
This notebook contains initial model development and experimentation, potentially testing multiple model options or architectures for classification of the celebrity face dataset. This includes pre-processing and potential baseline model comparisons.

**Main Processes:**
1. **Model Testing and Experimentation:** Explores various model architectures, hyperparameters, or pre-processing techniques.
2. **Performance Evaluation:** Provides insights on each model's effectiveness, establishing baselines for the face classification task.

**Outputs:**
- **Baseline Results:** Experimental results from initial models or architectures, potentially saved as plots or metrics within the notebook for comparison.

--- 

These descriptions summarize the purpose and main processes of each notebook, as well as the expected outputs, creating a cohesive workflow from data preprocessing through real-time classification. Let me know if you'd like further adjustments or additional details for each notebook!
