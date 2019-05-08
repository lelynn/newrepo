---
layout: page
title: Project 3
description: Artificial Intelligence (current project)
img: /assets/img/project3/aibrain.jpg

redirect: 
---
After taking some AI classes, I decided it is something I really enjoy. My supervisor introduced me to the AI department, where I then started an intership project. This internship became way more technical than the first one, where I had to become familiar with many new concepts such as adversarial images, different loss functions and regularizers, k-clustering, Chainer and Pytorch, working via ssh cluster, etc. I am currently still working on this project, and is quite challenging for me since I have never done anything like this before. But I liked to be challenged. 


So far I have become familiar with making my own convolutional models that take in an input image and that outputs specific things. Convolutional neural networks are a form of artificial neural network, composed of artificial neurons. Artificial neurons are are an abstraction of neural behavior, which performs by first summing the input signals over all incoming synapses, formally written as:

![synapse](https://latex.codecogs.com/gif.latex?I_i%20%3D%20%5Csum_j%7Bw_%7Bij%7D%20v_j%7D)

where _j_ is the pre-synaptic neuron and _i_ is the post-synaptic neuron, _v<sub>j</sub>_ is the signal from _j_ to _i_ and _w_{ij}$` is e strength of the synapse. 

Then the integral signal is transformed according to a non-linear function, formally as: 

![non-linear](https://latex.codecogs.com/gif.latex?v_i%20%3D%20f%28I_i%29)

Where _f(I_<sub>i</sub>)_ is often a sigmoid or linear rectifier function. 

The goal of my model, is thus to learn by adjusting the weights in the nodes to produce the desired outputs to their input data. The desired outputs are defined by the targets that are provided to the model along with the input.

Adversarial images are images generated with the intention to get the model to predict something different than what the actual target is. 



<div class="img_row">
    <img class="col three" src="{{ site.baseurl }}/assets/img/project3/NN.png" alt="" title="example image"/>
</div>
<div class="col three caption">
Convolutional neural network architecture.
</div>

So above is an convolutional network, that takes in an image and transforms it into an output with specific dimensions. My goals of this project are basically to: 1) train a bunch of models that take in specific inputs and then outputs predictions with specific dimensions. 2) combine all these models to make a huge supermodel (so that it is a strong model). 3) make adversarial images based on this supermodel (by iterating through the input with self-made adversarial targets).

<div class="img_row">
    <img class="col one" src="{{ site.baseurl }}/assets/img/project3/githubcat.png" alt="" title="example image"/>
</div>

<div class="col one caption">
Github Logo 
</div>
