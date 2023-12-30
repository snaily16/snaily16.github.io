---
layout: page
title: Face Aging using Style Transfer
description: a M.Tech thesis project under KhojApnoKi sponsored by MeitY Gov. of India 
img: assets/img/face_ageing/face_architecture.png
importance: 1
category: work
---

Face Aging is a technique to render a face image with aging effects, while preserving the attribute-irrelevant regions. With Generative Adversarial Network (GANs) we proposed a style based face ageing model following encoder-decoder architecture and skip connections with selective transfer units (STU) to imporve image quality and modify encoder feature for enchance attribute editing.
Our model is trained on unparied dataset and used style information gathered from random image from target age groups.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/face_ageing/aging_example.jpeg" title="Aging Example" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Face Aging is a technique for applying ageing effects to a face image in order to generate apredictable face at a specific age while maintaining the original image’s individuality, as shown in this figure
</div>

## Model Architecture
It is a style based generative adversarial network for face aging. As shown in figure, it consist of a generator and a discriminator to learn to synthesize an image of desired target age, given a source image of particular age and a style image of desired target age. The generator is divided in two parts - encoder ($$ G_{enc} $$) and decoder ($$ G_{dec} $$).
The source face image $$ x_s; x_s \in \mathbb{R}^{h \times w \times 3} $$ , of age s along with a style image $$ x_t'; x_t' \in \mathbb{R}^{h \times w \times 3} $$  of desired target age t; is passed through the generator and discriminator (where h and w are the height and width of image), to obtain a target image $$ \tilde{x}_t $$ with aging effects
of target class.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/face_ageing/face_architecture.png" title="Model Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Inspired from Georgopoulos et al., we proposed the following work. We introduced the Selective Transfer Unit (STU) block in the skip connections (described below). At the first three layers of this skip connection, we added the STU blocks, which selectively transform encoder feature and pass to the decoder
</div>

## Loss functions
The aim is to optimize the model to achieve these 4 main goals: (i) Better synthesized image quality, (ii) Diversity in aging pattern, (iii) Enforce cycle consistency, and (iv) Preserve identity of the original image. Therefore, they used four respective types of loss functions for training.

1. Adversarial Loss 

    To achieve better quality of synthesized image, we need to train the network with the minmax loss, the generator tries to minimize the following function while the discriminator tries to maximize it.

2. Feature Matching Loss
    
    The aim of the generator is to generate best possible image that would fool the discriminator. During the min-max game between the generator and discriminator this “best” image keeps on changing. However, the optimization process might get greedy and the model won’t converge and cause mode collapse problem. To ensure diversity in aging pattern, we used feature matching loss which modifies the generator’s cost function to minimise the statistical difference between the real and synthesized image’ features. As a result, feature matching broadens the aim beyond defeating the opponent to matching features in real-world images. So, we used it to train the generator to match the expected value of the features on an intermediate layer of the discriminator 

3. Identity Loss
    
    The adversarial loss makes the generator generate faces from the target distribution. The resulting face can be like any person in the target age group. The identity of the synthesized image must be preserved and so along with the adversarial loss they added the identity loss, calculated between the original face image and the synthesized image.

4. Cycle Consitency Loss

    Inspired from Cycle GAN, here we used the forward cycle consistency loss to learn a mapping and $$ M : x_s \to \tilde{x}_t $$ and $$ N : \tilde{x}_t \to x_s $$. Here, they want to enforce the intuition that these mappings should be reverses of each other. This loss ensures that $$ N(M(x_s)) \approx x_s $$ and $$ M(N(\tilde{x}_t)) \approx  \tilde{x}_t $$

## Datasets Used

We used Cross-Age Celebrity Dataset (CACD) dataset for training and testing purpose. “CACD is the largest publicly available cross-age face dataset, consisting of more than 160,000 images of 2,000 celebrities with age ranging from 16 to 62”. We used 85% of dataset for training purpose and 15% for testing purpose. We also used the CACD-VS verification subset dataset for testing and evaluation purpose. It has 4000 image pairs.
The dataset has large variations in pose, illumination and even style. In pre-processing, we detected face, aligned and center cropped images to size 128 × 128. The faces are divided in four groups corresponding to ages ranging from 0-30, 31-40, 41-50 and 50+, respectively. Below figure shows graphical analysis of dataset plotted w.r.t age groups, separated by gender. As one can see the dataset is not perfectly balanced. The number of females in group 1 (i.e below 30 age) are more than as compared to other three groups. However, this dataset contains some incorrect data. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/face_ageing/cacd_age_gender.png" title="Aging Example" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/face_ageing/age_distribution.png" title="Aging Example" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/face_ageing/dataset_age_distribution.png" title="Aging Example" class="img-fluid rounded z-depth-1" %}
</div>

