# Solutions — Computer Vision

These are reference answers. Many image-processing and model-design choices are valid; explain the reason for your choice and evaluate it empirically.

## A. Image fundamentals

1. Computer vision extracts information from images/video. Applications include medical image analysis, quality inspection, document OCR, agriculture, robotics, and accessibility.
2. Height and width describe spatial dimensions; channels describe values per pixel, such as RGB red/green/blue; pixel values encode intensity or color.
3. Grayscale commonly has one intensity channel; RGB has three color channels.
4. Single image: `(C, H, W)`. Batch: `(N, C, H, W)`.
5. Models and transforms expect specific value ranges. Accidentally passing 0–255 floats to a model expecting 0–1 changes input scale and can harm performance.
6. PNG is lossless and often useful for graphics/transparency; JPEG is lossy and often useful for photographs with smaller file sizes.
7. Resizing can remove fine details, distort aspect ratio if done carelessly, or blur small objects.
8. EXIF orientation can indicate that an image should be rotated for correct display; ignoring it may make images inconsistent.
9. A label identifies a class; a bounding box gives the location and extent of an object, usually as coordinates.
10. Faces or license plates may identify people; collection and use may require consent, legal review, or data minimization.

## B. Preprocessing and augmentation

11. Standardized dimensions allow examples to be stacked into batches and make model input dimensions predictable.
12. Normalization rescales or standardizes input values, often making optimization more stable. Use the range and mean/std expected by the model.
13.
```python
from torchvision.transforms import v2
import torch

transform = v2.Compose([
    v2.Resize((64, 64), antialias=True),
    v2.ToDtype(torch.float32, scale=True),
])
```
For a PIL image, include `v2.ToImage()` before `ToDtype`.
14. Augmentation creates label-preserving variations, such as random crop, horizontal flip, mild rotation, and color jitter.
15. Random validation/test transforms make evaluation noisy and may change the meaning of the test. Use deterministic transforms for comparable measurements.
16. Yes. A horizontal flip can alter text, left/right-specific signs, or an object whose class depends on orientation.
17. Examples: near-duplicate photos in train and test; fitting normalization or selecting augmentation based on the test set; splitting frames from the same video across sets.
18. Near-duplicates make the test set too similar to training data and can inflate apparent generalization.

## C. Convolutional neural networks

19. A kernel slides over local regions and learns weights that detect useful spatial patterns.
20. Stride controls the step size of the filter; padding adds border values and influences output dimensions.
21. A feature map is the activation output produced by a filter across spatial locations.
22. Pooling downsamples spatial dimensions, reducing computation and retaining salient local information.
23. Different channels can learn different patterns, from edges and textures to more complex combinations.
24. A receptive field is the region of the original input that can influence a particular activation.
25. A common sequence is convolution → activation → optional pooling, repeated as needed, followed by a classifier head.
26. Logits are raw model scores. Probabilities are transformed scores, for example via softmax for mutually exclusive classes.
27. With stride 1 and padding 1, the output spatial size remains 32×32.
28. Each output logit corresponds to one class, so the output dimension must equal the class count.

## D. Training and evaluation

29. Training data updates parameters; validation data supports model selection; test data estimates final generalization after choices are made.
30. A model can predict the majority class and still achieve high accuracy while missing minority examples.
31. It shows counts of true and predicted classes, revealing which classes are confused.
32. Data augmentation, early stopping, weight decay, dropout, more representative data, or a smaller model.
33. Transfer learning reuses features learned from a larger dataset, reducing data and compute requirements for a new task.
34. Freezing prevents selected parameters from updating; fine-tuning allows some or all pretrained parameters to adapt to the new dataset.
35. Pretrained weights were learned with particular input sizes, value scaling, and normalization; mismatches can degrade predictions.
36. Possible causes include distribution shift, label noise, leakage in the benchmark, class imbalance, changed camera conditions, or different user populations.

## E. Coding challenges — reference patterns

37.
```python
import numpy as np
import matplotlib.pyplot as plt

image = np.zeros((64, 64, 3), dtype=np.uint8)
image[10:40, 12:30, 0] = 255
image[25:55, 30:55, 1] = 180
plt.imshow(image)
plt.axis("off")
plt.show()
```
38.
```python
from PIL import Image
image = Image.open("your_image.jpg").convert("RGB")
print(image.size)  # (width, height)
```
39.
```python
from torchvision.transforms import v2
import torch

transform = v2.Compose([
    v2.ToImage(),
    v2.Resize((64, 64), antialias=True),
    v2.ToDtype(torch.float32, scale=True),
    v2.Normalize(mean=[0.5, 0.5, 0.5], std=[0.5, 0.5, 0.5]),
])
# image_tensor = transform(image)  # expects a PIL image or supported image input
```
Use normalization values appropriate for your model/dataset; these example values map values near `[0,1]` to approximately `[-1,1]`.
40.
```python
import torch
from torch import nn

image = torch.randn(1, 1, 32, 32)
same_size = nn.Conv2d(1, 4, kernel_size=3, stride=1, padding=1)(image)
smaller = nn.Conv2d(1, 4, kernel_size=3, stride=1, padding=0)(image)
print(same_size.shape)  # (1, 4, 32, 32)
print(smaller.shape)    # (1, 4, 30, 30)
```
41. Store epoch loss values in a list and plot them with Matplotlib. Compare training and validation curves rather than relying only on training loss.
42. A very low rate may learn slowly; a very high rate may oscillate or diverge. Record results rather than assuming a fixed best value.
43. Use `sklearn.metrics.classification_report` and `confusion_matrix` on held-out predictions.
44. Save predicted labels and probabilities, then plot a grid with each image, true label, and predicted label.
45. Use a stratified or group-aware split as appropriate, apply the pretrained model's documented preprocessing, replace the classifier head, and compare frozen-feature training with fine-tuning. Keep test data untouched until final evaluation.
46. A strong report documents data provenance and license, split strategy, preprocessing, baseline, metrics, errors, privacy/bias risks, and a specific next experiment.

## Mini-project checklist

- Verify labels and inspect class balance.
- Keep near-duplicate sources together when splitting.
- Fit preprocessing only on training data.
- Compare against a baseline and report class-level errors.
- Document known limitations and potential real-world risks.
