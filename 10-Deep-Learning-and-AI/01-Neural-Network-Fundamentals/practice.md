# Practice — Neural Network Fundamentals

Work through these exercises in order. Use `examples.ipynb` for reference, but try to solve each task before viewing `solution.md`.

## A. Concepts and calculations

1. Define a neuron in your own words.
2. Identify the roles of weights and bias in \(z = w^Tx+b\).
3. Calculate \(z\) for \(x=[2, -1]\), \(w=[0.5, 3]\), and \(b=1\).
4. What is the difference between a parameter and a hyperparameter? Give two examples of each.
5. Explain why a network with only linear transformations and no nonlinear activations is limited.
6. Describe the difference between forward propagation and backpropagation.
7. What does a loss function measure?
8. Explain the difference between an epoch, a batch, and an iteration/update.
9. Why should final evaluation use data not used to train the model?
10. Give two reasons training loss might fail to decrease.

## B. Activation functions

11. Implement sigmoid with NumPy and calculate it for `[-4, -1, 0, 1, 4]`.
12. Implement ReLU and evaluate it for `[-3, -0.5, 0, 2, 5]`.
13. State one advantage and one limitation of sigmoid.
14. When is softmax commonly used? What property do its outputs have?
15. Explain the “dying ReLU” problem at a high level.

## C. Perceptrons and forward propagation

16. Write a Python function for a step-activation perceptron with inputs, weights, and bias.
17. Use a perceptron to represent the AND function. Choose suitable weights and bias and test all four binary inputs.
18. Can a single perceptron represent XOR? Explain why or why not.
19. For a layer with input dimension 4 and output dimension 3, how many weights and biases are there?
20. Write the matrix equation for a dense layer with a batch of inputs.

## D. Training and evaluation

21. Explain gradient descent in plain language.
22. What can happen if the learning rate is too high? What if it is too low?
23. Why is feature scaling often helpful for neural networks?
24. For binary classification, name a common loss function and explain what the labels should look like.
25. Why can accuracy be misleading on a highly imbalanced dataset?
26. Name two ways to reduce overfitting.
27. Explain why setting a random seed can help reproducibility, and why it may not guarantee identical results on every device.
28. Compare a manually implemented NumPy network with a PyTorch model. What does the framework handle for you?

## E. Coding challenges

29. Modify the XOR NumPy example to train for 2,000, 5,000, and 10,000 epochs. Record the final loss and predictions. Do not assume more epochs always means better generalization.
30. Change the hidden-layer width from 4 to 2, 8, and 16. Compare convergence and final predictions.
31. Change the learning rate in the PyTorch example to `0.001`, `0.01`, and `0.1`. Plot or record training loss and explain the differences.
32. Add a validation split to the PyTorch example. Track validation loss after each epoch.
33. Build a small binary classifier using `make_moons` from scikit-learn. Compare a logistic-regression baseline with an MLP on held-out data.
34. Create a confusion matrix and report precision, recall, and F1 for your classifier.
35. Write a short experiment report: hypothesis, setup, results, limitations, and one next experiment.

## Mini-project rubric (20 points)

| Criterion | Points |
|---|---:|
| Clear target, features, and data description | 3 |
| Correct train/test split and preprocessing | 4 |
| Working neural-network implementation | 4 |
| Suitable metrics and error analysis | 4 |
| Reproducibility and readable code | 2 |
| Limitations and next steps | 3 |

**Reminder:** never fit preprocessing steps on the test set. Fit scalers on training data only, then transform validation/test data with the fitted scaler.
