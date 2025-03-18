#DL/StableDiffusion

### 1 . What is the purpose of the forward diffusion process in Stable Diffusion?
- ✅ To progressively add noise to an image
- ❌ To fine-tune the neural network
- ❌ To upscale the image resolution
- ❌ To remove noise from an image

### 2.  Why do we use the `torch.no_grad()` decorator in the `generate_image` function?
- ❌ To improve model accuracy
- ❌ To allow the network to learn during inference
- ✅ To prevent unnecessary gradient computations and reduce memory usage
- ❌ To ensure that the output image is always different

### 3. Why is `alpha_t` used in the reverse diffusion process?
- ❌ To update the weights of the U-Net model
- ❌ To randomly generate new noise samples
- ❌ To control the step size of the optimizer
- ✅ To scale back the original image information that was corrupted

### 4. In the denoising step, why do we add a small amount of random noise back in each step?
- ✅ To introduce diversity and prevent mode collapse
- ❌ To speed up the diffusion process
- ❌ To make the denoising process deterministic
- ❌ To reduce the training time

### 5. Why do we use a U-Net architecture in the reverse diffusion process?
- ❌ It applies transformations to images to enhance sharpness
- ✅ It efficiently predicts and removes noise from images
- ❌ It reduces computational cost by compressing images
- ❌ It generates new image samples directly from noise

### 6. What is the function of the 'add_noise method in the code?
- ✅ To add Gaussian noise to an image at a specific time step
- ❌ To predict the missing pixels in an image
- ❌ To transform images into high-resolution samples
- ❌ To denoise the images using the trained model

### 7. What loss function is used to train the U-Net model?
- ❌ Huber Loss
- ❌ Cross-Entropy Loss
- ✅ Mean Squared Error (MSE)
- ❌ Mean Absolute Error (MAE)

### 8. What is a key difference between Variational Autoencoders (VAEs) and Stable Diffusion in terms of randomness?
- ✅ VAEs use a fixed latent space, while Stable Diffusion introduces randomness at every denoising st
- ❌ Stable Diffusion does not use a probabilistic approach, unlike VAE
- ❌ VAEs use diffusion models internally for training
- ❌ VAEs add noise at every step like Stable Diffusion

### 9. What is the main advantage of a Variational Autoencoder (VAE) over a standard Autoencoder?
- ✅ VAE introduces stochasticity, allowing it to generate diverse outputs
- ❌ VAE removes noise in a stepwise manner like Stable Diffusion
- ❌ VAE can only reconstruct input images, while a standard Autoencoder generates new images
- ❌ VAE does not use any encoder-decoder structure

### 10.In the training step, what does the U-Net model predict?
- ✅ The noise added at a given time step
- ❌ The segmentation map of the image
- ❌ The denoised image
- ❌ The class label of the image

### 11. Why does Stable Diffusion perform better at high-resolution image generation compared to VAE?
- ✅ It works in a latent space and gradually denoises the image, preserving fine details
- ❌ It has a simpler architecture than VAE
- ❌ It does not require training like VAE
- ❌ It does not use any form of encoder-decoder architecture

### 12. How does Stable Diffusion differ from VAE in terms of image generation?
- ❌ Stable Diffusion does not use deep learning, while VAE does
- ✅ Stable Diffusion starts with a latent noise vector and removes noise iteratively, while VAE directly decodes a latent space vector
- ❌ VAE uses a stepwise noise removal process, while Stable Diffusion directly generates an image
- ❌ VAE does not encode image information, while Stable Diffusion does

### 13. What does the `generate_image` function do in the implementation?
- ✅ It generates an image by reversing the diffusion process from pure noise
- ❌ It classifies an image into categories
- ❌ It trains the U-Net model on new data
- ❌ It removes artifacts from a given image

### 14. How does a traditional Autoencoder differ from Stable Diffusion in terms of learning representation?
- ❌ Autoencoders generate images from pure noise, while Stable Diffusion reconstructs missing parts
- ❌ Autoencoders work with random noise, while Stable Diffusion works only with clean images
- ❌ Autoencoders use a denoising process to gradually add noise
- ✅ Autoencoders learn a compressed latent representation, while Stable Diffusion learns a noise removal process

### 15. In the forward diffusion process, what is the role of the `betas` variable?
- ❌ It sets the activation function threshold
- ❌ It defines the learning rate of the optimizer
- ❌ It determines the noise reduction factor during denoising
- ✅ It controls the noise variance dded at each step