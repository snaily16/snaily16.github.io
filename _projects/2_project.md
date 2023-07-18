---
layout: page
title: Face Aging using Style Transfer
description: a M.Tech thesis project under KhojApnoKi sponsored by MeitY Gov. of India 
img: assets/img/12.jpg
importance: 1
category: work
---

Face Aging is a technique to render a face image with aging effects, while preserving the attribute-irrelevant regions. With Generative Adversarial Network (GANs) we proposed a style based face ageing model following encoder-decoder architecture and skip connections with selective transfer units (STU) to imporve image quality and modify encoder feature for enchance attribute editing.
Our model is trained on unparied dataset and used style information gathered from random image from target age groups.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/aging_example.jpeg" title="Aging Example" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Face Aginig is a technique for applying ageing effects to a face image in order to generate apredictable face at a specific age while maintaining the original image’s individuality, as shown in this figure
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/face_architecture.png" title="Model Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Inspired from Georgopoulos et al. [10], we proposed the following work. We introduced the Selective Transfer Unit (STU) block in the skip connections (described below). At the first three layers of this skip connection, we added the STU blocks[38], which selectively transform encoder feature and pass to the decoder
</div>

We used Cross-Age Celebrity Dataset (CACD)[8] dataset for training and testing purpose. “CACD is the largest publicly available cross-age face dataset, consisting of more than 160,000 images of 2,000 celebrities with age ranging from 16 to 62” [8]. We used 85% of dataset for training purpose and 15% for testing purpose. We also used the CACD-VS verification subset dataset for testing and evaluation purpose. It has 4000 image pairs.
The dataset has large variations in pose, illumination and even style. In pre-processing, we detected face, aligned and center cropped images to size 128 × 128. The faces are divided in four groups corresponding to ages ranging from 0-30, 31-40, 41-50 and 50+, respectively. Table 5.1 shows the total number of samples in each age group, also number of male and female in each group. It also shows the average original and estimated age in each group. Fig 5.1 shows graphical analysis of dataset plotted w.r.t age groups, separated by gender. As one can see the dataset is not perfectly balanced. The number of females in group 1 (i.e below 30 age) are more than as compared to other three groups. However, this dataset contains some incorrect data. There exist some mismatched age labels to face images.ou can also put regular text between your rows of images.  Say you wanted to write a little bit about your project before you posted the rest of the images. 

 You describe how you toiled, sweated, *bled* for your project, and then... you reveal its glory in the next row of images.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/cacd_age_gender.png" title="Aging Example" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/age_distribution.png" title="Aging Example" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/dataset_age_distribution.png" title="Aging Example" class="img-fluid rounded z-depth-1" %}
</div>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>


The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}
```html
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
```
{% endraw %}
