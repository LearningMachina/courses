# Deep Learning

## Concept

Deep learning uses neural networks with many layers to learn hierarchical representations of data. Each layer learns increasingly abstract features — pixels → edges → shapes → objects. The Jetson's GPU makes it practical to train and run these networks on the robot itself.

## Topics

- Neural networks: layers, activations, backpropagation
- Training a network on the robot's own sensor data
- Convolutional Neural Networks (CNNs)
- How the robot's YOLO detector was trained
- Transfer learning: fine-tuning a pretrained model

## Example

```python
import torch
import torch.nn as nn
from torchvision import models, transforms
from PIL import Image

# --- Simple fully-connected network for sensor classification ---
class SensorClassifier(nn.Module):
    def __init__(self, input_size, num_classes):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(input_size, 64),
            nn.ReLU(),
            nn.Linear(64, 32),
            nn.ReLU(),
            nn.Linear(32, num_classes),
        )

    def forward(self, x):
        return self.net(x)

# --- Transfer learning: fine-tune MobileNetV3 for 2 classes ---
model = models.mobilenet_v3_small(weights="IMAGENET1K_V1")
model.classifier[-1] = nn.Linear(model.classifier[-1].in_features, 2)

# Freeze all layers except the classifier
for param in model.features.parameters():
    param.requires_grad = False

optimizer = torch.optim.Adam(model.classifier.parameters(), lr=1e-3)
```

```
Backpropagation in one sentence:
  Compute the loss, then walk backwards through the network
  using the chain rule to find how much each weight contributed
  to the error, then nudge each weight to reduce it.
```

## Exercise

1. Build a `SensorClassifier` that takes 4 sensor readings and outputs "safe" or "obstacle" (2 classes).
2. Generate 500 synthetic samples (random inputs, label = "obstacle" if any reading < 0.3) and train the network.
3. Evaluate on a held-out test set and print accuracy, precision, and recall.
4. Load MobileNetV3 and fine-tune it on a 2-class image dataset collected from the robot's camera (e.g., "clear path" vs "obstacle").

## Check

Explain to the robot:

- What does a ReLU activation do, and why is it used instead of a sigmoid in hidden layers?
- What is the vanishing gradient problem, and which architectural choices help avoid it?
- Why does transfer learning work? What has the pretrained model already learned?
