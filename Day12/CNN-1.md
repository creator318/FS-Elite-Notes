#DL/CNN

## Implement a convolutional neural network (CNN) in PyTorch that processes 32×32 RGB images and defines a custom loss function. Your implementation must use the exact variable and function names given below so that the autograder can verify your solution automatically.

```
           +---------------------+
           |  Input: 32×32×3     |
           +----------+----------+
                      |
                      v
           +---------------------+
           | Conv1: 3×3, s=1,    |
           | no padding, 32 ch   |
           | Output: 30×30×32    |
           +----------+----------+
                      |
                      v
           +---------------------+
           | MaxPool1: 2×2, s=2  |
           | Output: 15×15×32    |
           +----------+----------+
                      |
                      v
           +---------------------+
           | Conv2: 3×3, s=1,    |
           | no padding, 64 ch   |
           | Output: 13×13×64    |
           +----------+----------+
                      |
                      v
           +---------------------+
           | MaxPool2: 2×2, s=2  |
           | Output: 6×6×64      |
           +----------+----------+
                      |
                      v
           +---------------------+
           | Flatten             |
           | (6×6×64 = 2304)     |
           +----------+----------+
                      |
                      v
           +---------------------+
           | Fully Connected     |
           | 2304 → 10           |
           +---------------------+
```

## Import Libraries

```python
import torch
from torch import nn
from torch.nn import functional as F
```

## 1. Network Architecture
Implement a CNN with the following specifications:

### Input:
Images of size 32×32 with 3 channels (RGB).

### Block 1:

Convolutional Layer (conv 1):
kernel_size: 3×3
stride: 1
padding: 0 (i.e. no padding)
in_channels: 3
out_channels: 32
Max-Pooling Layer (pool):
kernel_size: 2
stride: 2


### Block 2:

Convolutional Layer (conv 2):
kernel_size: 3×3
stride: 1
padding: 0
Output channels: 64
Max-Pooling Layer (pool):
kernel_size: 2
stride: 2

### Fully Connected Layer (fc):

After the two convolution+pooling blocks, flatten the output and add a fully connected layer that maps the 
Flattened features to 10 output classes.

### Forward Method
Your forward method should perform the following steps exactly:
* Apply ReLU activation after each convolution.
* Use the pooling layer after each convolution block.
* Flatten the output before passing it to the fully connected layer.
* Finally, produce the logits via the fully connected layer.

### Variable Name for the Model:
The final model must be stored in a variable called myModel.


```python
class MyCNN(nn.Module):
    def __init__(self, num_classes=10, input_size=32):
        """
        Constructs a CNN with two convolution+maxpool blocks followed by a fully connected layer.
        
        Specifications:
          - Input: images of size 32x32 with 3 channels.
          - Block 1:
              * Conv layer with kernel_size 3x3, stride 1, no padding, output channels = 32.
              * MaxPool layer with kernel size 2, stride 2.
          - Block 2:
              * Conv layer with kernel_size 3x3, stride 1, no padding, output channels = 64.
              * MaxPool layer with kernel size 2, stride 2.
          - Flatten the features and apply a fully connected layer mapping to output size 10.
        """
        super(MyCNN, self).__init__()

        self.conv1 = nn.Conv2d(in_channels=3, out_channels=32, kernel_size=3, stride=1, padding=0)
        self.conv2 = nn.Conv2d(in_channels=3, out_channels=64, kernel_size=3, stride=1, padding=0)
        self.pool = nn.MaxPool2d(kernel_size=2, stride=2)
        self.fc = nn.Linear(6*6*64, num_classes)
    
    def forward(self, x):
        x = F.relu(self.conv1(x))
        x = self.pool(x)
        x = F.relu(self.conv2(x))
        x = self.pool(x)

        x = x.view(x.size(0), -1)
        x = self.fc(x)
        return x
```

### The final model should be stored in the variable 'myModel'

```python
myModel = MyCNN(num_classes=10, input_size=32)
```

## 2. Loss Function
Define a custom loss function that meets the following criteria:

Function Name:

The function must be named LF.
Function Arguments:

The function should take three parameters: the model's output (logits), the target labels, and the model itself.

### Function Behaviour:

Compute the standard cross-entropy loss between the output and the target.
Add an L 2 regularization term calculated as the sum of squared L 2 norms of all the model's parameters.
The regularization term must be scaled by a factor of 0.1.
The function must return a scalar tensor representing the combined loss.
Variable Name for the Loss Function:

The loss function must be stored in a variable called LF (i.e. the function name is LF).


```python
def LF(output, target, model):
    """
    Return:
        Tensor: Scalar loss value.
    """
    ce_loss = F.cross_entropy(output, target)
    l2_reg = sum(torch.sum(param ** 2) for param in model.parameters())
    loss = ce_loss + le_reg/10
    return loss
```

### Initialize the _total_params_ value with total number of trainable parameters in the notebook in the network


```python
total_params = sum(p.numel() for p in myModel.parameters() if p.requires_grad)
print(total_params)
```
