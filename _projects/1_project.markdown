---
layout: page
title: Project 1
description: Brain2Pix
img: /assets/img/project_brain2pix/tardis.png
---

# Abstract
Reconstructing complex and dynamic visual perception from brain activity remains a major challenge in machine learning applications to neuroscience. Here, we present a new method for reconstructing naturalistic images and videos from very large single-participant functional magnetic resonance imaging data that leverages the recent success of image-to-image transformation networks. This is achieved by exploiting spatial information obtained from retinotopic mappings across the visual system. More specifically, we first determine what position each voxel in a particular region of interest would represent in the visual field based on its corresponding receptive field location. Then, the 2D image representation of the brain activity on the visual field is passed to a fully convolutional image-to-image network trained to recover the original stimuli using VGG feature loss with an adversarial regularizer. In our experiments, we show that our method offers a significant improvement over existing video reconstruction techniques.

## Results

<b>Main results -- FixedRF (test set):</b>

`---[input] --- [rec] --- [stimuli] `

![fixedRF_a](/assets/img/project_brain2pix/recons_fixed_of_all_frames_as_video_a.gif)


![fixedRF_b](/assets/img/project_brain2pix/recons_fixed_of_all_frames_as_video_b.gif)

![fixedRF_c](/assets/img/project_brain2pix/recons_fixed_of_all_frames_as_video_c.gif)


## Paper + Repository

[Click :book:](https://www.frontiersin.org/articles/10.3389/fnins.2022.940972/full) for the published paper.

[Click :computer:](https://github.com/neuralcodinglab/brain2pix) for the source code / github repository.


Thank you!

## Tutorial Videos
