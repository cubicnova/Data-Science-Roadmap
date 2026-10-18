# 05 — Computer Vision

Computer vision enables machines to extract useful information from images and video. In this module, learn image representation and preprocessing, convolutional neural networks (CNNs), augmentation, transfer learning, and the foundations of object detection and image segmentation.

## Learning objectives

By the end of this module, you should be able to:

- Explain image dimensions, channels, pixel ranges, and common image formats.
- Load, visualize, resize, normalize, and augment images.
- Describe convolution, kernels, stride, padding, pooling, and feature maps.
- Build and train a CNN using PyTorch.
- Evaluate image classifiers with accuracy, precision, recall, F1, and a confusion matrix.
- Explain transfer learning and when to freeze or fine-tune pretrained layers.
- Distinguish image classification, object detection, and semantic/instance segmentation.
- Identify common computer-vision risks such as data leakage, class imbalance, and distribution shift.

## Prerequisites

- Python, NumPy, Matplotlib, and basic image arrays.
- Basic neural-network and PyTorch knowledge.
- Familiarity with classification metrics and train/validation/test splits.

## Setup

Install the core libraries:

```bash
python -m pip install torch torchvision numpy matplotlib scikit-learn pillow jupyter
```

PyTorch and Torchvision versions must be compatible. If installation fails or you need GPU acceleration, use the official [PyTorch installation selector](https://pytorch.org/get-started/locally/).

Launch Jupyter:

```bash
jupyter notebook
```

## Learning roadmap

1. **Image representation:** height, width, channels, pixels, RGB and grayscale.
2. **Image I/O:** load and display images, inspect dimensions, convert formats.
3. **Preprocessing:** resize, crop, normalize, and convert images to tensors.
4. **Data augmentation:** random flips, crops, rotations, and color changes for training.
5. **Convolutions:** filters/kernels, stride, padding, feature maps, receptive fields.
6. **Pooling and CNN architecture:** downsampling, channels, flattening, and classification heads.
7. **Training and evaluation:** batches, losses, optimizers, validation, confusion matrix.
8. **Transfer learning:** reuse a pretrained backbone and replace the classifier head.
9. **Computer-vision tasks:** classification vs. detection vs. segmentation.
10. **Responsible practice:** data provenance, consent, privacy, bias, robustness, and deployment limits.

## Notebook guide

Open `examples.ipynb` and run cells from top to bottom. The core examples use generated images and scikit-learn's digits dataset, so they do not need a large image download.

| Section | What you will learn |
|---|---|
| Image arrays | Create and visualize synthetic RGB images |
| Image transforms | Resize, flip, and normalize an image tensor |
| Convolution | Apply a convolution filter and inspect output shape |
| CNN | Train a small CNN on the digits dataset |
| Evaluation | Review accuracy, classification report, and confusion matrix |
| Augmentation | Compare deterministic preprocessing with random augmentation |
| Transfer learning | Understand a safe fine-tuning workflow |
| Vision task comparison | Choose classification, detection, or segmentation |

## Core concepts

### Image tensor convention

Torchvision commonly represents a single image as `(C, H, W)` and a batch as `(N, C, H, W)`. Integer image tensors often use pixel values from 0 to 255; float image tensors commonly use values from 0 to 1 before normalization. Check the expected range of the selected transform and model. See the [Torchvision transforms documentation](https://docs.pytorch.org/vision/stable/transforms.html). <Cite refs={["turn304789search0","turn304789search5"]}/>

### Main computer-vision tasks

| Task | Output | Example |
|---|---|---|
| Image classification | One or more labels per image | Classify a flower species |
| Object detection | Labels and bounding boxes | Locate vehicles in a street image |
| Semantic segmentation | A class for each pixel | Separate road, sky, and buildings |
| Instance segmentation | A mask for each detected object | Separate individual people |
| Keypoint estimation | Coordinates of important points | Estimate body joints |

### CNN building blocks

- **Convolution:** learns filters that respond to patterns such as edges, textures, and shapes.
- **Stride:** controls how far the filter moves per step.
- **Padding:** controls border handling and output dimensions.
- **Pooling:** reduces spatial resolution and computation.
- **Channels:** feature-map depth, not the same thing as RGB input channels.
- **Classifier head:** maps learned features to class scores (logits).

## Best practices

- Split data before augmenting or fitting preprocessing.
- Use random augmentation only for training; validation and test preprocessing should be deterministic.
- Avoid near-duplicate images crossing train/test splits.
- Track class-level metrics when classes are imbalanced.
- Compare against a simple baseline before increasing model complexity.
- For transfer learning, use the preprocessing associated with the pretrained weights.
- Keep a record of data source, license, consent, and known limitations.
- Do not treat model predictions as ground truth in safety-critical settings.

## Practice workflow

1. Read the notebook section and run it.
2. Complete `practice.md` without opening `solution.md`.
3. Check answers and reproduce any code you do not understand.
4. Change one setting at a time and record results.

## Mini-project: image classification

Build a classifier for a permitted dataset such as handwritten digits, a small plant dataset, or a curated set of everyday objects.

Deliverables:
- Dataset source, class list, and license/consent notes.
- Reproducible train/validation/test split.
- Image preprocessing and training-only augmentation.
- Baseline and CNN comparison.
- Accuracy, per-class precision/recall/F1, and confusion matrix.
- Sample correct and incorrect predictions.
- Limitations, potential bias, and a deployment-risk note.

## Completion checklist

- [ ] I can explain image tensor shapes and pixel ranges.
- [ ] I can preprocess and augment images appropriately.
- [ ] I understand convolution, stride, padding, and pooling.
- [ ] I can train and evaluate a small CNN.
- [ ] I can explain transfer learning.
- [ ] I can distinguish classification, detection, and segmentation.
- [ ] I can identify data leakage and limitations in a vision model.

## Further learning

- [Torchvision documentation](https://docs.pytorch.org/vision/stable/)
- [Torchvision transforms and augmentation](https://docs.pytorch.org/vision/stable/transforms.html)
- [PyTorch transfer learning tutorial](https://pytorch.org/tutorials/beginner/transfer_learning_tutorial.html)
- [scikit-learn evaluation metrics](https://scikit-learn.org/stable/modules/model_evaluation.html)

---
Part of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).
