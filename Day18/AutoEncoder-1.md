#DL/AutoEncoder

### **Problem Statement:**
Your task is to build a multi-layer **autoencoder** using **PyTorch** to compress the images into a lower-dimensional representation and then reconstruct them. The model should use the **Adam optimizer** and **Mean Squared Error (MSE) loss**. The goal is to minimize the reconstruction loss and generate an accurate representation of the input data.

#### **Steps to Complete the Exercise:**
1. **Build the Multi-Layer Autoencoder**:  
   - **Encoder**: Multiple dense layers reducing dimensionality.
   - **Decoder**: Symmetric layers reconstructing the image.
2. **Train the Model**: Use **Mean Squared Error (MSE) loss** and **Adam optimizer** to train the autoencoder.

The ANSI diagram for the multi-layer autoencoder
```
		Input Layer (28x28 pixels)  
          |
      [Flatten Layer] (784)
          |
      [Dense Layer 512] - Linear, ReLU
          |
      [Dense Layer 256] - Linear, ReLU
          |
      [Dense Layer 128] - Linear, ReLU
          |
      [Dense Layer 64]  <-- Latent Representation (Bottleneck)
          |
      [Dense Layer 128] - Linear, ReLU
          |
      [Dense Layer 256] - Linear, ReLU
          |
      [Dense Layer 512] - Linear, ReLU
          |
      [Dense Layer 784] - Sigmoid (Reconstructs 28x28 image)
          |
      [Unflatten Layer] (Reshape to 28x28)
          |
      Output (Reconstructed Image)
```

## Import Libraries

```python
import torch
from torch import nn, optim
```

## AutoEncoder

```python
class Autoencoder(nn.Module):
    def __init__(self):
        super(Autoencoder, self).__init__()
        # Encoder
        self.encoder = nn.Sequential(
            nn.Flatten(),
            nn.Linear(784, 512),
            nn.ReLU(),
            nn.Linear(512, 256),
            nn.ReLU(),
            nn.Linear(256, 128),
            nn.ReLU(),
            nn.Linear(128, 64)
        )
        # Decoder
        self.decoder = nn.Sequential(
            nn.Linear(64, 128),
            nn.ReLU(),
            nn.Linear(128, 256),
            nn.ReLU(),
            nn.Linear(256, 512),
            nn.ReLU(),
            nn.Linear(512, 784),
            nn.Sigmoid(),
            nn.Unflatten(1, (28, 28))
        )
    
    def forward(self, x):
        encoded = self.encoder(x)
        decoded = self.decoder(encoded)
        return decoded
```

## Define loss function and optimizer

```python
student_model = Autoencoder()
criterion = nn.MSELoss();
optimizer = optim.Adam(student_model.parameters())
```
