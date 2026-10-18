# Practice — Computer Vision

Try each task before reading `solution.md`. Use `examples.ipynb` to test code and observe how image data changes.

## A. Image fundamentals

1. What is computer vision? Name four practical applications.
2. Explain image height, width, channels, and pixel values.
3. What is the difference between grayscale and RGB images?
4. What are common shapes for a single image and a batch in PyTorch?
5. Why must you check whether image values are in `[0, 255]` or `[0, 1]`?
6. Compare PNG and JPEG at a high level.
7. What information can be lost when resizing an image?
8. Why can EXIF orientation metadata matter when loading images?
9. What is the difference between an image label and a bounding box?
10. Name two privacy or consent issues that can arise in image datasets.

## B. Preprocessing and augmentation

11. Explain why image dimensions are often standardized before batching.
12. What does normalization do, and why can it help training?
13. Implement a Torchvision transform pipeline that resizes to 64×64 and converts to a float tensor.
14. What is data augmentation? Give four examples.
15. Why should random augmentation normally be applied to training data but not validation/test data?
16. Can a horizontal flip change the label? Give an example where it could.
17. What is data leakage in an image dataset? Give two examples.
18. Why should near-duplicate images be considered when splitting a dataset?

## C. Convolutional neural networks

19. Explain the purpose of a convolution kernel/filter.
20. What do stride and padding control?
21. What is a feature map?
22. How does pooling change spatial dimensions?
23. Why do CNNs often use multiple feature channels?
24. What is a receptive field?
25. What are the main stages of a basic CNN classifier?
26. Explain the difference between logits and probabilities.
27. For a 32×32 input, a 3×3 convolution with stride 1 and padding 1, what is the spatial output size?
28. Why should the output layer's size match the number of classes in a single-label classifier?

## D. Training and evaluation

29. What is the difference between training, validation, and test sets?
30. Why can overall accuracy be misleading with imbalanced classes?
31. What can a confusion matrix tell you?
32. Name three techniques that may reduce overfitting.
33. What is transfer learning, and why is it useful when data is limited?
34. What is the difference between freezing a backbone and fine-tuning it?
35. Why should the preprocessing used with pretrained weights match their expected input format?
36. Name three reasons a model may perform well on a benchmark but poorly in production.

## E. Coding challenges

37. Create a synthetic RGB image as a NumPy array and display it with Matplotlib.
38. Load a sample image using Pillow, inspect its dimensions, and convert it to RGB.
39. Use Torchvision transforms to resize and normalize an image tensor.
40. Apply a 2D convolution to a grayscale image and compare the output dimensions for different padding values.
41. Train the notebook CNN for different epoch counts and plot loss.
42. Compare two learning rates and explain the impact on training stability.
43. Report accuracy, precision, recall, F1, and a confusion matrix for a classifier.
44. Display a grid of correctly and incorrectly classified examples.
45. Propose a transfer-learning experiment with a pretrained model. Specify the split, preprocessing, metrics, and safeguards against leakage.
46. Design a mini-project and write a short report describing dataset provenance, method, results, limitations, and next steps.

## Mini-project rubric (20 points)

| Criterion | Points |
|---|---:|
| Dataset, labels, and provenance documented | 3 |
| Correct split and preprocessing | 4 |
| Working baseline and CNN/transfer-learning model | 4 |
| Appropriate metrics and error analysis | 4 |
| Reproducible code and readable notebook | 2 |
| Limitations, privacy, and bias discussion | 3 |

**Reminder:** fit learned preprocessing only on training data. Keep validation/test transforms deterministic, and never use the test set for repeated model selection.
