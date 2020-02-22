Title: Illustrated Survey of Self-Supervised Learning
Date: 2020-02-22 03:00
Modified: 2020-02-22 03:00
Category: research
Slug: illustrated-self-supervised-learning
Summary: ...
Status: draft
Authors: Amit Chaudhary

# Image  
## 1. **Image Colorization**
Formulation:   
> What if we prepared pairs of (grayscale, colorized) images by applying grayscale to millions of images we have freely available?  

![](/images/ss-colorization-data-gen.png){.img-center}  

We could use a encoder-decoder architecture based on fully convolutional neural network and compute the L2 loss between the predicted and actual color images.

![](/images/ss-image-colorization.png){.img-center}    

To solve this task, the model has to learn about different objects present in image and related parts so that it can paint those parts in the same color. Thus, representations learned are useful for downstream tasks.
![](/images/ss-colorization-learning.png){.img-center}  

## 2. **Image Superresolution**
Formulation:   
> What if we prepared training pairs of (small, upscaled) images by downsampling millions of images we have freely available?  

![](/images/ss-superresolution-training-gen.png){.img-center}  


GAN based models such as [SRGAN](https://arxiv.org/abs/1609.04802) are popular for this task. A generator takes a low-resolution image and outputs a high-resolution image using fully convolutional network. The actual and generated images are compared using both mean-squared-error and content loss to imitate human like quality comparison. A binary-classification discriminator takes an image and classifies whether it's an actual high resolution image(1) or a fake generated superresolution image(0). This interplay between the two models leads to generator learning to produce images wth fine details. 
![](/images/ss-srgan-architecture.png)

Semantic features are learnt by this task and the features from the discriminator can be used for downstream tasks.

**Papers**:  
[Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network](https://arxiv.org/abs/1609.04802)


## 3. **Image Inpainting**
Formulation:   
> What if we prepared training pairs of (corruped, fixed) images by randomly removing part of images?  

![](/images/ss-image-inpainting-data-gen.png){.img-center}  


GAN based models such as [SRGAN](https://arxiv.org/abs/1609.04802) are popular for this task. A generator takes a low-resolution image and outputs a high-resolution image using fully convolutional network. The actual and generated images are compared using both mean-squared-error and content loss to imitate human like quality comparison. A binary-classification discriminator takes an image and classifies whether it's an actual high resolution image(1) or a fake generated superresolution image(0). This interplay between the two models leads to generator learning to produce images wth fine details. 
![](/images/ss-srgan-architecture.png)

Semantic features are learnt by this task and the features from the discriminator can be used for downstream tasks.

**Papers**:  
[Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network](https://arxiv.org/abs/1609.04802)

## References
- Jing, et al. “[Self-Supervised Visual Feature Learning with Deep Neural Networks: A Survey.](https://arxiv.org/abs/1902.06162)”