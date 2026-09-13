# ECG Image Classification

A computer vision project for preprocessing and preparing ECG (Electrocardiogram) images for automated classification. The project focuses on extracting the ECG waveform from images, reducing visual noise such as grid lines, enhancing image quality, and converting the images into a consistent format suitable for machine learning and deep learning models.

## Project Overview

Electrocardiogram images often contain background grids, varying image dimensions, noise, and unnecessary regions surrounding the waveform. These factors can make automated analysis more difficult.

This project develops an image-processing pipeline that:

* Loads ECG images from class-based folders
* Detects and crops the ECG graph region
* Converts images to grayscale
* Enhances contrast using histogram equalization
* Removes grid components using Fast Fourier Transform (FFT)
* Applies the processed waveform as a mask to the original image
* Resizes images to `128 × 128`
* Organizes the processed images according to their original classes

The preprocessing pipeline is designed as a foundation for an ECG image classification system.

## Dataset

The notebook processes ECG images belonging to four categories:

| Class                                              | Description                                                      |
| -------------------------------------------------- | ---------------------------------------------------------------- |
| Normal Person ECG Images                           | ECG images from normal individuals                               |
| ECG Images of Patient that have abnormal heartbeat | ECG images showing abnormal heartbeat patterns                   |
| ECG Images of Myocardial Infarction Patients       | ECG images associated with myocardial infarction                 |
| ECG Images of Patient that have History of MI      | ECG images from patients with a history of myocardial infarction |

The notebook reports **948 processed images** with dimensions of `128 × 128`.

## Methodology

### 1. ECG Image Loading

Images are read from class-specific directories using OpenCV. Supported formats include:

* `.jpg`
* `.jpeg`
* `.png`

### 2. ECG Graph Area Detection

The unnecessary regions around the ECG graph are removed using:

1. Grayscale conversion
2. Gaussian blurring
3. Binary thresholding
4. Contour detection
5. Selection of the largest contour
6. Cropping to the detected bounding rectangle

This helps focus the processing on the ECG graph itself.

### 3. Image Enhancement

The cropped ECG image is converted to grayscale and histogram equalization is applied to improve contrast.

The processed images are then resized to:

```text
128 × 128 pixels
```

### 4. Grid Line Removal

ECG images commonly contain horizontal and vertical grid lines that can interfere with computer vision models.

This project uses an FFT-based approach:

```text
Image
  ↓
Grayscale
  ↓
2D FFT
  ↓
Frequency-domain filtering
  ↓
Inverse FFT
  ↓
Normalized image
```

The frequency-domain mask suppresses components around the central horizontal and vertical frequency axes before reconstructing the image.

### 5. Waveform Extraction

The cleaned grayscale image is normalized and used as a mask over the cropped color ECG image. This produces an image emphasizing the ECG waveform while reducing the influence of the background grid.

### 6. Processed Dataset Generation

The cleaned images are saved into a new directory while preserving their original class-folder structure.

```text
Original Dataset
       │
       ▼
 ECG Image
       │
       ▼
Crop Graph Area
       │
       ▼
Grayscale Conversion
       │
       ▼
FFT-based Grid Removal
       │
       ▼
Waveform Masking
       │
       ▼
Processed ECG Image
       │
       ▼
Class-specific Folder
```

## Technologies Used

* Python
* OpenCV
* NumPy
* Matplotlib
* tqdm
* Google Colab / Google Drive

## Project Structure

A recommended GitHub structure is:

```text
ECG-Image-Classification/
│
├── ECG_Image_Classification.ipynb
├── README.md
├── requirements.txt
│
├── data/
│   ├── normal/
│   ├── abnormal/
│   ├── myocardial_infarction/
│   └── history_of_mi/
│
└── processed_data/
    ├── normal/
    ├── abnormal/
    ├── myocardial_infarction/
    └── history_of_mi/
```

> The dataset itself is not included in this repository unless you have permission to redistribute it.

## Installation

Clone the repository:

```bash
git clone https://github.com/your-username/ECG-Image-Classification.git
cd ECG-Image-Classification
```

Install the required Python packages:

```bash
pip install opencv-python numpy matplotlib tqdm
```

Or install from `requirements.txt`:

```bash
pip install -r requirements.txt
```

## Running the Project

The project is implemented as a Jupyter/Google Colab notebook.

Open:

```text
ECG_Image_Classification.ipynb
```

If using Google Colab, update the dataset path in the notebook:

```python
dataset_path = '/content/drive/MyDrive/your_dataset_folder'
```

Run the cells sequentially to:

1. Load the ECG dataset
2. Inspect ECG images
3. Crop the graph area
4. Remove grid components
5. Extract the waveform
6. Generate the processed dataset

## Results

The preprocessing pipeline successfully processes the ECG images and produces standardized image representations.

The notebook reports:

```text
Processed images shape: (948, 128, 128)
```

with four ECG-related classes detected in the dataset.

The notebook also includes visual comparisons of the original ECG image and the cropped graph area to inspect the effectiveness of the preprocessing steps.

## Current Scope

The current notebook primarily focuses on **ECG image preprocessing and waveform extraction**.

A machine learning/deep learning classifier is not implemented in the provided notebook. The processed images can subsequently be used to train models such as:

* CNN
* Transfer learning models
* ResNet
* EfficientNet
* Vision Transformers

Possible classification tasks include distinguishing normal ECGs from abnormal ECGs and identifying myocardial infarction-related ECG patterns.

## Future Improvements

* Train a CNN-based ECG image classifier
* Apply data augmentation
* Handle class imbalance
* Add train/validation/test splits
* Compare multiple deep learning architectures
* Evaluate using accuracy, precision, recall, F1-score, and confusion matrix
* Add Grad-CAM or other explainability techniques
* Develop an inference pipeline for new ECG images
* Deploy the trained model using Streamlit or Flask

---

⭐ If you find this project useful, consider giving the repository a star.
