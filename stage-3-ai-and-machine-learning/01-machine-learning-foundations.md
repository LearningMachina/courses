# Machine Learning Foundations

## Concept

Machine learning is the practice of writing programs that improve from experience rather than from explicit rules. Instead of coding every decision, you show the algorithm examples and it learns the pattern. On the robot this means teaching it to recognise obstacles, interpret sensor readings, or predict good actions — without hand-crafting every rule.

## Topics

- What is machine learning? Supervised vs unsupervised vs reinforcement
- Linear regression and logistic regression from scratch
- Train/validation/test splits, overfitting, regularisation
- Gradient descent: the engine of learning
- Introduction to PyTorch

## Example

```python
import torch
import torch.nn as nn

# Toy dataset: predict motor speed from distance reading
# (supervised regression)
X = torch.tensor([[1.0], [0.5], [0.3], [0.1], [0.8]], dtype=torch.float32)
y = torch.tensor([[0.2], [0.4], [0.6], [1.0], [0.3]], dtype=torch.float32)

model     = nn.Linear(1, 1)
criterion = nn.MSELoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)

for epoch in range(200):
    pred = model(X)
    loss = criterion(pred, y)
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    if epoch % 50 == 0:
        print(f"Epoch {epoch:3d}  loss={loss.item():.4f}")

# Predict on new input
with torch.no_grad():
    print(model(torch.tensor([[0.4]])))
```

```
Train / Validation / Test split
  Training   70 % — model sees these, updates weights
  Validation 15 % — monitor for overfitting, tune hyperparameters
  Test       15 % — final, one-time evaluation — never peek early!
```

## Exercise

1. Run the PyTorch regression example and plot the predicted line against the data points (use `matplotlib`).
2. Deliberately overfit the model (increase epochs to 5 000, reduce dataset to 3 points) and observe the validation loss rising while training loss falls.
3. Add L2 regularisation (`weight_decay=0.01` in the optimizer) and see how it changes the overfitting.
4. Replace `nn.Linear` with a two-layer network and compare performance.

## Check

Explain to the robot:

- What is the difference between supervised and reinforcement learning? Give a robot example of each.
- What does gradient descent actually do step by step?
- Why is it cheating to evaluate your model on the training set?
