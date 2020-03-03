Title: The Illustrated SimCLR Framework
Date: 2020-03-01 10:00
Modified: 2020-03-01 10:00
Category: illustration
Slug: illustrated-simclr
Summary: A visual guide to the SimCLR framework for contrastive learning of visual representations
Status: draft
Authors: Amit Chaudhary

In recent years, [numerous self-supervised learning methods](https://amitness.com/2020/02/illustrated-self-supervised-learning/) have been proposed for learning image representations, each getting better than the previous. But, their performance was still below the supervised counterparts. This changed when **Chen et. al** proposed a new framework in their paper "[SimCLR: A Simple Framework for Contrastive Learning of Visual Representations](https://arxiv.org/abs/2002.05709)". The paper not only improves upon the previous state-of-the-art self-supervised learning methods but also beats the supervised learning method on ImageNet classification.

In this article, I will explain the key ideas of the framework proposed in the paper using diagrams.

## The Nostalgic Intuition
As a kid, I remember we had to solve such puzzles in our textbook.  
![](/images/contrastive-find-a-pair.png){.img-center}    
The way a child would solve it is look at the picture of animal on left side, know its a cat, then search for cat on the right side.  
![](/images/contrastive-puzzle.gif){.img-center}    
> "Such exercises were prepared for the child to be able to recognize an object and contrast that to other objects. Can we teach machines in a similar manner?"

It turns out that we can through a technique called **Contrastive Learning**. It attempts to teach machines to distinguish between similar and dissimilar things.

## Problem Formulation for Machines
In order to model the above exercise for a machine instead of a child, we see that we require 3 things:  

1. **Examples of similar and dissimilar images**   
We would require example pairs of images that are similar and images that are different for training a model.  
![](/images/contrastive-need-one.png){.img-center}  
The supervised school of thought would require us to manually create such pairs by a human. Self-supervised learning can be applied to automate this. But how?
![](/images/contrastive-supervised-approach.png){.img-center}  
![](/images/contrastive-self-supervised-approach.png){.img-center}  

2. **Ability to know what an image represents**  
We need some mechanism to get representations that allows machine to understand an image.
![](/images/image-representation.png){.img-center}

3. **Ability to quantify if two images are similar**  
We need some mechanism to compute similarity of two images. 
![](/images/image-similarity.png){.img-center}

## The SimCLR Framework Approach

The paper proposes a framework "**SimCLR**" for modeling the above problem in a self-supervised manner. It blends the concept of *Contrastive Learning* with a few novel ideas to learn visual representations without human supervision. 

## Framework
The framework, as the full-form suggests, is very simple. An image is taken and random transformations are applied to it to get a pair of two augmented images. Each image in that pair is passed through an encoder to get representations. Then a non-linear fully connected layer is applied to get representations z. The task is to maximize the similarity between these two representations z_i and z_j for same image.
![](/images/simclr-general-architecture.png){.img-center}


## Step by Step Example
Let's explore the various components of the framework with an example. Suppose we have a training corpus of millions of unlabeled images.
![](/images/simclr-raw-data.png){.img-center}

1. **Data Augmentation** (Self-supervised Formulation)  
First, we generate batches of size N from the raw images. Let's take a batch of size N = 2 for simplicity.
![](/images/simclr-single-batch.png){.img-center}  

The paper defines a random transformation function T that takes an image and applies a combination of `random (crop + flip + color jitter + grayscale)`.
![](/images/simclr-random-transformation-function.gif){.img-center}  

For each image in this batch, random transformation function is applied to get 2 pairs of images. Thus, for a batch size of 2, we get 2N = 4 total pairs of images.  
![](/images/simclr-batch-data-preparation.png){.img-center}  
2. **Base Encoder** (Getting Representations)  

Each augmented image in a pair is passed through an encoder to get image representations. The encoder used is generic and replaceable with other architectures. The two encoders shown above are weighted shared and we get vectors <tt class="math">h_i</tt> and <tt class="math">h_j</tt>.
![](/images/simclr-encoder-part.png){.img-center}

In the paper, the authors used ResNet-50 architecture as the ConvNet encoder. The output is a 2048-dimensional vector h.
![](/images/simclr-paper-encoder.png){.img-center}
3. **Projection Head**  
The representations <tt class="math">h_i</tt> and <tt class="math">h_j</tt> of the two augmented images are then passed through a series of non-linear **Dense -> Relu -> Dense** layers to apply non-linear transformation and project it into a representation <tt class="math">z_i</tt> and <tt class="math">z_j</tt>.

4. **Loss function**  
Similarity for the z-representations are computed using cosine similarity.  

> [figure for comparison of 2 z-representations]  

The similarity between two augmented versions of an image is calculated using cosine similarity. For two augmented images <tt class="math">x_i</tt> and <tt class="math">x_j</tt>, the cosine similarity is calculated on its projected representations <tt class="math">z_i</tt> and <tt class="math">z_j</tt>.
<pre class="math">
s_{i,j} = \frac{z_{i}^{T}z_{j}}{(\tau ||z_{i}|| ||z_{j}||)}
</pre>

where   
<tt class="math">\tau</tt> is the temperature parameter which can be tuned.
<tt class="math">||z_{i}||</tt> is the norm of the vector

SimCLR uses the NT-Xent loss (the normalized temperature-scaled cross entropy loss).
Based on the similarity, the loss function is computed as 
<pre class="math">
l(i, j) = -log\frac{exp(s_{i, j})}{ \sum_{k=1}^{2N} l_{[k!= i]} exp(s_{i, k})}
</pre>

<pre class="math">
L = \frac{1}{2N} \sum_{k=1}^{N} [l(2k-1, 2k) + l(2k, 2k-1)]
</pre>

## Downstream Tasks
Once the model is trained on contrastive learning task, it can be used for transfer learning. In this, the representations from the encoder are used instead of representations obtained from the projection head. This representations can be used for downstream tasks like classification on ImageNet.

## Objective Results
SimCLR outperformed previous supervised and self-supervised methods on ImageNet.  

- On ImageNet, it achieves 76.5% top-1 accuracy which is 7% improvement over previous SOTA self-supervised method and on-par with supervised ResNet50.  
- When trained on 1% of labels, it achieves 85.8% top-5 accuracy outperforming AlexNet with 100x fewer labels

## References
- [A Simple Framework for Contrastive Learning of Visual Representations](https://arxiv.org/abs/2002.05709)