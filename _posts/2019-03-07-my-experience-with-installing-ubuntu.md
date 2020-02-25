---
layout: post
title:  Blogpost2| My experience with Ubuntu for cuda and cupy
date:   2019-03-07
description: More like a rant... I ended up having to install it three times and here is why...

---
# Installing Ubuntu

Since I have an nvidia GPU on my macbook pro, I have decided that I wanted to install Ubuntu. This would allow me to run Cupy and use the GPU for neural networks while using the Chainer framework.

### First time installing

After following a tutorial online on how to partition my harddrive and install ubuntu on my macbook properly, I was super happy to see this when booting up:


![ubuntu image](/assets/img/blog_img/ubuntu_loader.png "Ubuntu loader")

So my ultimate goal was to use cupy, which is supported by cuda 10.0 (or older), and cuda 10.0 or older is supported by Ubunutu 18.04.1 (or older). I didn't know this so I first installed the newest Ubuntu 18.04.2 and then after a while found out I couldn't install cuda 10.0 properly. This is because on the [cuda 10.0 installation guide](https://docs.nvidia.com/cuda/archive/10.0/){:target="\_blank"}, it says I needed the kernel version 14.15.0 (and I had 124.18.0). I tried different things to fix this problem (such as changing the kernel version to an older version) before I finally resolved to reinstalling an older Ubuntu.  

>Pretty much, I installed the wrong version of Ubuntu!

 When I installed [Ubuntu](https://www.ubuntu.com/#download){:target="\_blank"} from it's home page it was 18.04. Little did I know that I needed 18.04.1 instead of 18.04.2! So yes, I decided to reinstall. 

### Second time installing

The second time I installed Ubuntu, I specifically chose the 18.04.2 version and finally was able to install cuda 10.0. However, I thought to myself, I wonder how it would look like if I changed my screen resolution... Since I had an extra screen connected, I decided to play around with the different resolution possibilities. 

> Turns out changing the screen resolution made my whole screen black... and then green!

![alt text](/assets/img/blog_img/green_screen.jpg "Green screen")

Everytime I rebooted it turned completely black and green on both screens :'( I googled everywhere and tried all possible solutions that were offered on Stackoverflow. I was able to access the root terminal but still, but still nothing worked. I decided to install Ubuntu once again.

### Third time is the charm

I WAS SO ANNOYED AT THIS POINT. I started questioning whether I even still wanted to use Chainer anymore, perhaps it is easier to rewrite all my codes to PyTorch.. (probably it was and still is a good idea). But I thought, this is a good way to learn things. Besides, I already have put some time in this, might as well finish the job... So finally.. the third time installing Ubuntu, everything works. I was able to install cuda properly, and then cupy and then everything else. It was a relieving moment, to see I was able to import cupy into python. 

### UNTIL... 

>OUT OF MEMORRY!

![alt text](/assets/img/blog_img/out_of_memory.png "Out of memory")

Until I realized my model architecture is way too heavy for my tiny GPU... OUT OF MEMORY!!  

![alt text](/assets/img/blog_img/cryface.jpg "sadface")

Well.... I guess I cannot do much about this. It is nice to know how to use my GPU I guess.. And perhaps one day I should buy an external GPU.. but they're not cheap. 

### Lessons learned

- Think twice or three times before installing the latest version of everything.
- Dont just change your screen resolution on ubuntu. 
- Everything you do you can learn from. 


Now that I have let out my frustrations on this blog, I would like to thank whomever is reading this for listening (probably nobody). This was very therapeutical to write. 