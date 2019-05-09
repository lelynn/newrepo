---
layout: post
title:  Making an ensemble of models
date:   2019-05-08
description: It is easier than I thought
---

# Trained models
The pretrained models that I used ([models provided by PyTorch](https://pytorch.org/docs/stable/torchvision/models.html){:target="\_blank"}) all contained linear layers as last layers. I replaced these layers with convolutional layers since I do not want to loose spatial information when predicting my outputs. Additionally I had a self-made deep neural network which consisted of residual blocks and deconvolution layers. I then trained the models separately and ended up having 7 models that performed well. 

# Ensembling the models

In order to make a supermodel in PyTorch, we first have to make instances of every model classes. Since we have 7 quite large models, I made a separate file containing all the model classes, and named in `all7models.py`. First I need to import all necessary things:

``` python
import torch.nn as nn
import torchvision.models as models
import torch
import torch.nn.functional as F
import numpy as np
```



<!-- ![imports](/assets/img/blog_img/blog4/imports1.png){:width="100%"} -->


Then all the different classes of models that were trained. Here is ana exmaple of the AlexNet model (1 out of 7):

``` python 
#   ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
#   :   A l e x N e t                                                                        
#   ::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
class AlexNet(nn.Module):
    def __init__(self, outsize):
        super(AlexNet, self).__init__()
        self.outsize = outsize
        self.alexnet = models.alexnet(pretrained = True)
        self.convout = nn.Sequential(
            nn.Upsample(48),
            nn.Conv2d(in_channels=192, out_channels=192, kernel_size=3, stride=1, padding=1),
            nn.BatchNorm2d(192),
            nn.ReLU(),
            nn.Upsample(48),
            nn.Conv2d(in_channels=192, out_channels=96, kernel_size=3, stride= 1, padding=1),
            nn.BatchNorm2d(96),
            nn.ReLU(),
            nn.Upsample((self.outsize[0]+3, self.outsize[1]+3)),
            nn.Conv2d(in_channels=96, out_channels=1, kernel_size=6, stride= 1, padding=1)
            )

    def forward(self, x):
        x = self.alexnet.features[:5](x)
        x = self.convout(x)
        y = torch.sigmoid(x)
        return y 
```


<!-- ![AlexNet](/assets/img/blog_img/blog4/AlexNetInstance.png "AlexNet"){:width="100%"} -->

I used the pretrained model from `torchvision.models.alexnet(pretrained=True)` and then replace the last layers with a sequentual layer containing upsampling, convolutional, batchnormalization and rectified linear unit layers to ultimately obtain an output of my desired dimensions. The layers that were replaced with the sequential layer were selected based on their outsize, meaning I only ended up using pretrained layers that kept the images at large enough dimensions (again, to keep enough spatia information). I did this with every model and found that they predicted nice outputs.

Anyways, when all the instances of the models are made, the instance of the ensemble can be made. This is a class that initializes all the instances of the models. For PyTorch, it is done like this:

<!-- ![Ensemble](/assets/img/blog_img/blog4/EnsembleInstance.png "Ensemble"){:width="100%"} -->
``` python
class MyFinalEnsemble_cpu(nn.Module):
    def __init__(self, outsize):
        super(MyFinalEnsemble, self).__init__()
        """
        This is the ensemble of the SuperModel that is built up of the models (with some having shorter amount of iterations than 50). 
        The AlexNet 100x, the DenseNet 100x, InceptionV3 100x, ResDeconv 30x, ResNet 100x, SqueezeNet 25x, and VGG 19x."""
        
        self.outsize = outsize
        
        self.alexnet = AlexNet(self.outsize)
        self.alexnet.load_state_dict(torch.load('final_runs_48x64/AlexNet/AlexNet100ep_48x64', map_location='cpu'))
        
        self.densenet = DenseNet201(self.outsize)
        self.densenet.load_state_dict(torch.load('final_runs_48x64/DenseNet201/DenseNet100ep_48x64', map_location='cpu'))
        
        self.inceptionv3 = InceptionV3(self.outsize)
        self.inceptionv3.load_state_dict(torch.load('final_runs_48x64/InceptionV3/InceptionV3_100ep_48x64', map_location='cpu'))
        
        self.resdeconv = ResblocksDeconv(3, self.outsize, self.outsize)
        self.resdeconv.load_state_dict(torch.load('final_runs_48x64/ResDeconv/ResDeconv30ep_48x64', map_location='cpu'))
        
        self.resnet = ResNet152(self.outsize)
        self.resnet.load_state_dict(torch.load('final_runs_48x64/ResNet152/ResNet152_100ep_48x64', map_location='cpu'))
        
        self.squeezenet = SqueezeNet1_1(self.outsize)
        self.squeezenet.load_state_dict(torch.load('final_runs_48x64/SqueezeNet/SqueezeNet25ep_48x64', map_location='cpu'))
        
        self.vgg19 = VGG19BN(self.outsize)
        self.vgg19.load_state_dict(torch.load('final_runs_48x64/VGG19BN/VGG19BN18ep', map_location='cpu'))

        
    def forward(self, x):
        x1 = self.alexnet(x)
        x2 = self.densenet(x)
        x3 = self.inceptionv3(x)
        x4 = self.resdeconv(x)
        x5 = self.resnet(x)
        x6 = self.squeezenet(x)
        x7 = self.vgg19(x)
        
        x = sum([x1, x2, x3, x4, x6])/5

        return x
```

# Initializing the ensemble
So now that the instances of the models and the the final ensemble are made, the `all7models.py` is complete and ready to be used.

In a separate file (I used a jupyter notebook file) in the same directory, I can do:
 ``` python 
import all7models
```

Then I can initialize the ensemble like this: 

``` python
model = all7models.MyFinalEnsemble(outsize)
model.eval()
model.cuda(device)
```

<!-- 
![Initialize](/assets/img/blog_img/blog4/InitEnsem.png "initialize"){:width="100%"} -->

And then the model can be used to predict!!