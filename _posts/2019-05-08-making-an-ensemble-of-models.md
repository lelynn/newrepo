---
layout: post
title:  Making an ensemble of models
date:   2019-05-08
description: It is easier than I thought
---

# Trained models
The pretrained models that I used ([models provided by PyTorch](https://pytorch.org/docs/stable/torchvision/models.html)) all contained linear layers as last layers. I replaced these layers with convolutional layers since I do not want to loose spatial information when predicting my outputs. Additionally I had a self-made deep neural network which consisted of residual blocks and deconvolution layers. I then trained the models separately and ended up having 7 models that performed well. 

# Ensembling the models

In order to make a supermodel in PyTorch, we first have to make instances of every model classes. Since we have 7 quite large models, I made a separate file containing all the model classes, and named in `all7models.py`. First I need to import all necessary things:

![imports](/assets/img/blog_img/blog4/imports1.png){:width="100%"}


Then all the different classes of models that were trained. Here is ana exmaple of the AlexNet model (1 out of 7):


![AlexNet](/assets/img/blog_img/blog4/AlexNetInstance.png "AlexNet"){:width="100%"}

Pretty much, I used the pretrained model from `torchvision.models.alexnet(pretrained=True)` and then replace the last layers with a sequentual layer containing upsampling, convolutional, batchnormalization and rectified linear unit layers to ultimately obtain an output of my desired dimensions. The layers that were replaced with the sequential layer were selected based on their outsize, meaning I only ended up using pretrained layers that kept the images at large enough dimensions (again, to keep enough spatia information). I did this with every model and found that they predicted nice outputs.

Anyways, when all the instances of the models are made, the instance of the ensemble can be made. This is a class that initializes all the instances of the models. For PyTorch, it is done like this:

![Ensemble](/assets/img/blog_img/blog4/EnsembleInstance.png "Ensemble"){:width="100%"}


# Initializing the ensemble
So now that the instances of the models and the the final ensemble are made, the `all7models.py` is complete and ready to be used.

In a separate file (I used a jupyter notebook file) in the same directory, I can do `import all7models`. Then I can initialize the ensemble like this: 

![Initialize](/assets/img/blog_img/blog4/InitEnsem.png "initialize"){:width="100%"}

And then the model can be used to predict!!