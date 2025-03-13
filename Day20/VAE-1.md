#DL/VAE 

### **Problem Statement:**
In this notebook, you will implement a Variational Autoencoder (VAE) using PyTorch.

#### Tasks:
1. Implement the Encoder network to produce mean and log-variance.
2. Implement the Reparameterization Trick.
3. Implement the Decoder network to reconstruct images.
4. Implement the VAE loss function.
5. Train the VAE using the Adam optimizer.
6. Generate and visualize new images from random latent vectors.

The ANSI diagram for the required VAE
```
                         +-------------------------+
                         |        Input x          |
                         |    (Flattened Image)    |
                         +-------------------------+
                                     |
                                     v
                         +-------------------------+
                         |        Encoder          |
                         |  Linear(784 → 400)      |
                         |         ReLU            |
                         +-------------------------+
                                     |
                                     v
                  +---------------------------------------+
                  |      Mean (μ)      |    Log-Var       |
                  |  Linear(400 → 20)  | Linear(400 → 20) |
                  +---------------------------------------+
                                     |  |
                                     |  |
                                     v  v
                      +---------------------------------+
                      | Compute std = exp(0.5 * logvar) |
                      | Sample eps ~ N(0,1)             |
                      | Compute z = μ + eps * std       |
                      +---------------------------------+
                                     |
                                     v
                         +-------------------------+
                         |        Decoder          |
                         |  Linear(20 → 400)       |
                         |         ReLU            |
                         |  Linear(400 → 784)      |
                         |       Sigmoid           |
                         +-------------------------+
                                     |
                                     v
                         +-------------------------+
                         |   Reconstructed Image   |
                         +-------------------------+
                                     |
                                     v
                         +-------------------------+
                         |     Loss Function       |
                         |  Reconstruction Loss    |
                         |  Binary Cross-Entropy   |
                         |                         |
                         |   KL Divergence Loss    |
                         | D_KL(q(z|x) || p(z))    |
                         +-------------------------+
```

## Import Libraries

```python
import torch
from torch import nn, optim
from torch.nn import functional as F
from torchvision import datasets, transforms
from torch.utils.data import DataLoader
import matplotlib.pyplot as plt
from PIL import Image
```

## VAE

```python
class VAE(nn.Module):
    def __init__(self, latent_dim=20):
        super(VAE, self).__init__()
        self.latent_dim = latent_dim

        # Encoder
        self.encoder = nn.Sequential(
            nn.Linear(784,400),
            nn.ReLU()
        )

        self.fc_mu = nn.Linear(400,20)
        self.fc_logvar = nn.Linear(400,20)
        
        # Decoder
        self.decoder = nn.Sequential(
            nn.Linear(20,400),
            nn.ReLU(),
            nn.Linear(400,784),
            nn.Sigmoid()
        )
    
    def encode(self, x):
        x = self.encoder(x)
        mu = self.fc_mu(x)
        logvar = self.fc_logvar(x)
        return mu, logvar
    
    def reparameterize(self, mu, logvar):
        """Reparameterization Trick"""
        std = torch.exp(0.5*logvar)
        eps = torch.rand_like(std)
        return mu + eps * std
    
    def decode(self, z):
        return self.decoder(z)
    
    def forward(self, x):
        mu, logvar = self.encode(x)
        z = self.reparameterize(mu, logvar)
        return self.decode(z), mu, logvar
```

## Loss function

```python
def vae_loss(reconstructed_x, x, mu, logvar):
    reconstruction_loss = F.binary_cross_entropy(reconstructed_x,x,reduction='sum')
    kl_divergence = -0.5*torch.sum(1+logvar-mu.pow(2)-logvar.exp())
    return reconstruction_loss + kl_divergence
```

## Load Dataset

```python
DATASET_PATH = "..."

transform = transforms.Compose([
    transforms.Grayscale(num_output_channels=1),  # Ensure grayscale images like MNIST
    transforms.ToTensor(),  # Convert image to tensor
    transforms.Lambda(lambda x: x.view(-1))  # Flatten to match MNIST format (1, 28, 28) -> (784,)
])

# Use DatasetFolder to load images without requiring subdirectories
train_dataset = datasets.DatasetFolder(root=DATASET_PATH, loader=lambda x: Image.open(x).convert("L"), extensions=('png', 'jpg', 'jpeg'), transform=transform)
train_loader = DataLoader(train_dataset, batch_size=64, shuffle=True)
```

## Train VAE

```python
# Training loop
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
vae = VAE(latent_dim=20).to(device)
optimizer = optim.Adam(vae.parameters(), lr=1e-3)

num_epochs = 3
for epoch in range(num_epochs):
    vae.train()
    train_loss = 0
    for batch_idx, (data, _) in enumerate(train_loader):
        data = data.to(device)
        optimizer.zero_grad()
        reconstructed_x, mu, logvar = vae(data)
        loss = vae_loss(reconstructed_x, data, mu, logvar)
        loss.backward()
        train_loss += loss.item()
        optimizer.step()
    print(f"Epoch {epoch+1}, Loss: {train_loss / len(train_loader.dataset)}")
```

    Epoch 1, Loss: 540.5794140625
    Epoch 2, Loss: 502.70193359375
    Epoch 3, Loss: 444.66857421875
    

    [NVSHARE][INFO]: Successfully initialized nvshare GPU
    [NVSHARE][INFO]: Client ID = 58c052946641e577
    
## Generate new images


```python
vae.eval()
with torch.no_grad():
    z = torch.randn(16, 20).to(device)  # Sample random latent vectors
    samples = vae.decode(z).cpu().numpy()
```

## Plot generated images

```python
fig, axes = plt.subplots(4, 4, figsize=(5, 5))
for i, ax in enumerate(axes.flat):
    ax.imshow(samples[i].reshape(28, 28), cmap='gray')
    ax.axis('off')
plt.show()
```