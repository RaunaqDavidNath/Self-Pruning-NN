# Tredence_Assignment
Pruning_Assignment

Self Pruning Neural Network Analysis
1. Correctness of the PrunableLinear Layer
The PrunableLinear layer correctly implements a gated weight mechanism

Each weight has a corresponding learnable gate parameter called gate_scores

The gates are passed through a sigmoid function so their values remain between 0 and 1

This enables soft pruning instead of hard removal of weights

The effective weights are computed as weight multiplied by sigmoid of gate_scores

If a gate value approaches 0 the corresponding weight is effectively removed

If a gate value approaches 1 the weight remains active

Gradient Flow
Gradients flow through both the weights and the gate_scores

The sigmoid function is differentiable so gradients can pass through it without issues

This allows the model to learn which connections should be pruned

There are no non differentiable operations or detached tensors in the forward pass

Conclusion
The layer is correctly implemented and supports full gradient based learning :contentReference[oaicite:0]{index=0}

2. Training Loop and Sparsity Loss Implementation
Loss Function
The total loss combines classification loss and sparsity loss

Cross entropy loss is used for classification

Sparsity loss is computed from the gate values

The final loss is cross entropy plus lambda multiplied by sparsity loss

Sparsity Loss
The sparsity loss is calculated as the mean of all gate values across layers

Minimizing this value pushes gates toward zero which leads to pruning

Training Strategy
A warmup phase is used for the first few epochs

During warmup sparsity loss is not applied so the model can learn meaningful features

After warmup the sparsity term is introduced gradually

Optimizer Design
Different learning rates are used for different parts of the model

CNN parameters use a standard learning rate

MLP weights use a standard learning rate

Gate parameters use a higher learning rate so pruning happens faster

A cosine annealing scheduler is used to stabilize training

Conclusion
The training loop is properly designed and successfully integrates sparsity into learning :contentReference[oaicite:1]{index=1}

Why L1 Regularization Encourages Sparsity
The sparsity loss is computed using the mean of all gate values, which behaves similarly to an L1 penalty.

L1 regularization encourages sparsity because it applies a constant gradient pressure toward zero for all parameters.

Unlike L2 regularization, which shrinks values smoothly, L1 tends to push small values exactly to zero.

In this model, since gates control whether a weight is active, minimizing their values causes many gates to approach zero.

As a result, the corresponding weights are effectively removed, leading to a sparse network.

3. Quality of Results and Lambda Trade Off Analysis
Observations
As lambda increases the sparsity of the model increases

Higher sparsity means more weights are effectively pruned

However very high sparsity can reduce model accuracy

Trade Off
Low lambda results in weak pruning and higher accuracy

Medium lambda gives a balance between accuracy and sparsity

High lambda results in strong pruning but can lead to underfitting

Evidence of Self Pruning
Gate statistics show a reduction in average gate values over time

The percentage of near zero gates increases during training

Sparsity level is measured by counting gates below a small threshold

Conclusion
The model clearly demonstrates self pruning behavior and the lambda trade off is logical and consistent :contentReference[oaicite:2]{index=2}

4. Code Quality
Strengths
The code is modular with clearly defined components

Feature extraction and pruning logic are separated cleanly

Utility functions are provided for sparsity tracking and analysis

Training and evaluation functions are reusable and easy to follow

Possible Improvements
Activation functions could be made more consistent

More inline comments could improve readability for beginners

Logging could include more detailed metrics such as validation accuracy per epoch

Conclusion
The code is clean structured and easy to understand and extend :contentReference[oaicite:3]{index=3}

Model Evolution from MLP to CNN Based Architecture
Phase 1 Initial Approach Using MLP
The initial model was a simple multilayer perceptron

Input images were flattened into vectors before being passed to the network

This approach lacked spatial understanding of image data

Performance was limited because important image patterns were not captured effectively

Phase 2 Adding Self Pruning to MLP
PrunableLinear layers were introduced into the MLP

This allowed the model to learn which connections were important

Unnecessary weights were gradually reduced through gating

The model became more efficient but still lacked strong feature extraction

Phase 3 Final Architecture Using CNN and Self Pruning MLP
A convolutional neural network was added as a feature extractor

The CNN processes images and outputs a feature vector

This feature vector is passed to the self pruning MLP for classification

The CNN learns spatial features while the MLP performs classification and pruning

Final Pipeline
Input image goes through CNN feature extractor

The extracted features are passed into the prunable MLP

The final output is the predicted class

Benefits
Improved accuracy due to better feature extraction

Efficient model due to automatic pruning

Better balance between performance and complexity

Conclusion
The final architecture effectively combines CNN based feature learning with self pruning for efficient classification

Final Summary
The PrunableLinear layer is correctly implemented and supports gradient flow

The training loop correctly applies sparsity loss along with classification loss

The model successfully learns to prune itself during training

The lambda parameter provides a clear trade off between accuracy and sparsity

The transition from MLP to CNN based architecture significantly improves performance
