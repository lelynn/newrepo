---
layout: page
title: Project 3
description: Adversarial Images
img: /assets/img/project_adversarial/adversarial_lady.png

redirect: 
---

## Abstract
Adversarial examples are inputs that are slightly modified with the objective to mislead com-
puter algorithms. Adversarial images, perceived as a great threat for computer vision models,
has not yet been proven to be dangerous for the human visual system. Some slight effects of
adversarial images on humans have recently been shown, however these results apply only in
very limited experimental settings. It remains a question whether adversarial images designed
to attack computer vision models can influence visual processes such as overt attention. In
this study, we address this question by making use of state-of-the-art approaches to gener-
ate adversarial images with the intent to alter human attention. We train a CNN-ensemble
to predict saliency maps and take the learned parameters to generate adversarial images with
the pursue to influence human eye movements in the same way as the CNN-ensemble. We
find that our adversarial images has fooled the saliency prediction of well-performing convo-
lutional network models and also has succeeded in steering human eye-movements to target
locations

## Results

<p align="center">
<img src="/assets/img/project_adversarial/plots.png" alt="plots" width="700"/> 
</p>

## Report
See [my full report :book:]({{ site.url }}/assets/MasterThesis2_final.pdf){:target="\_blank"} on this project. 



