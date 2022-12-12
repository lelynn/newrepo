---
layout: post
title:  Blogpost7| Running Jupyter lab/notebook remotely
date:   2022-12-10
description: A manual on forward porting using terminal
comments: true
---
## Introduction
Hello! In this post, I will show you how to run JupyterNotebook or JupyterLab on any server that you have ssh access to. I really like this method because it allows me to work and debug on the server without lag, especially when I have to work with large datasets and complex models. All you need is a terminal and a browser!

## Step-by-step guide
1.  First you need to ssh to your remote server by running the following from your laptop
  ```ssh usrname@ip.address```
2. One you're in the server, make sure that <b>jupyter is installed</b>. You can use [this documentation](https://github.com/jupyterlab/jupyterlab) to install JupyterLab or [this documention](https://docs.jupyter.org/en/latest/install/notebook-classic.html) for JupyterNotebook.
3. Once you have it installed, `cd` towards the directory that you want to work in. Then run the following command in the terminal of the remote server:
    ```jupyter lab --no-browser --port ####```
    You can replace the port numner (`####`) with any desired number that is not already in use, for instance here I use 8880. Then the terminal should output something like this:
    <p align="center">
    <img src="/assets/img/blog_img/blog_jupyter/jupyter_lab1.png" alt="JL1" width="700"/> 
    </p>
  4. Then you open a new tab in your terminal on your laptop and run the following command: 
    ```ssh -N -f -L localhost:####:localhost:#### usrname@ip.address```
    Where `####` is the portnumber (so it can be 8880), and `usrname@ip.address` are the credentials that you use to log into the server.

5. Once you run that, you can copy the link in your terminal from step 2 and paste it in your browser. Or paste `http://localhost:8880/lab`. Now you should be redirected to JupyterLab!

    <p align="center">
    <img src="/assets/img/blog_img/blog_jupyter/jupyter_lab2.png" alt="JL2" width="700"/> 
    </p>


Hope this was helpful :)