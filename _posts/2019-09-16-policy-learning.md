---
layout: post
title:  Blogpost5| Reinforcement learning&#58; Policy-based
date:   2019-09-16
description: Explaining the concept of policy-based learning

---
Bological entities can extract a large amount of information from binary feedback signals. Reinforcement learning (RL) is a general term for an agent that learns to act using reinforce signals (for example reward or punishment). Two important types of RL in machine learning are Deep Q-learning and policy-based RL. This post is focused on the latter.  

An agent that does policy-based RL, learns by updating the parameters of its <b>policy</b> &#960;, which is based on the state that the agent is in and its received reward upon performing an action. 

In machine learning, the &#960; can be seen as a differentiable function, which can be implemented with a neural network. The &#960; receives a state as an input and in return outputs different probability scores of each action. In this post, I will focus on explaining how we can use this policy network to perform reinforcement learning. 


## The environment
<img width="40%" style="float: right;" src= "/assets/img/blog_img/blog5/agent_plateau.JPG">

Imagine an agent running down a continous plateau with the aim to stay on the plateau as long as possible. At every timestep _t_, he encounters a different surrounding structure and needs to decide whether to shift himself to the right or left in order to stay "alive". His lifespan is represented as _(t<sub>1</sub>, ..., T)_. The longer he manages to survive during his run, the better his performance. 


### Some definitions (in our environment)
- Agent: The agent interacts with its environment by performing actions in discrete time steps. 
- Actions _a<sub>t</sub>_ : The action that the agent can take at timestep _t_. At each timestep, the agent can either shift to the left, or to the right.
- State _s<sub>t</sub>_: The state that the agent is in at timestep _t_.
- Reward _r<sub>t</sub>_: The reward returned at timestep _t_.
- Episode: Everytime the agent runs on the plateau to learn, it creates an episode that contains all the actions taken at every timestep.
- Policy &#960; : Is the basic neural network that predicts the probability scores of every action at each time step given a state.


## The learning concept
Basically, the reinforcement algorithm is represented by the following equation:

![equation](https://latex.codecogs.com/gif.latex?%5Cnabla%20J%28%5Ctheta%29%20%3D%20%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bi%3D1%7D%5EN%5CBig%28%7B%5Csum_%7Bt%3D1%7D%5ET%7D%7B%5Cnabla_%5Ctheta%20%5Clog%7B%5Cpi_%5Ctheta%7D%28a_%7Bi%2Ct%7D%7Cs_%7Bi%2Ct%7D%29%7D%5CBig%29%20%5CBig%28%5Csum_%7Bt%3Di%7D%5ET%20r%28s_%7Bi%2Ct%7D%2C%20a_%7Bi%2Ct%7D%29%5CBig%29)

Where _r(s<sub>i,t</sub>,a<sub>i,t</sub>)_ is the reward at _(s<sub>i,t</sub>,a<sub>i,t</sub>)_.

During each episode, the agent runs on the plateau and performs some actions. After the episode, we make use of the reinforce algorithm to update the parameters of the policy network and then run it again.
<!-- ![equation](https://latex.codecogs.com/gif.latex?%5CDelta%20%5Ctheta%20%3D%20%5Ceta%20%5Cnabla_%5Ctheta%20J%28%5Ctheta%29%29) -->

In other words, we want to optimize the parameters in such a way that maximizes the gradient of &theta;. This is done in three steps:

1. Run the policy network &#960;<sub>&theta;</sub>_(a<sub>t</sub>&#124;s<sub>t</sub>)_  which outputs various action probability scores (&tau;) given a state _s<sub>t</sub>_ and sample a probability score &tau;<sup>i</sup> from the output.

2.  Then, we want to calculate the gradient ascent: 

<!-- $$\nabla J (\theta) \approx \sum_i \Big( \sum_t \nabla_\theta \log\pi_\theta(a_{i,t} | s_{i,t}) \Big) \Big(\sum_t r(s_{i,t}, a_{i,t})\Big)$$ -->

![equation](https://latex.codecogs.com/gif.latex?%5Cnabla%20J%20%28%5Ctheta%29%20%5Capprox%20%5Csum_i%20%5CBig%28%20%5Csum_t%20%5Cnabla_%5Ctheta%20%5Clog%5Cpi_%5Ctheta%28a_%7Bi%2Ct%7D%20%7C%20s_%7Bi%2Ct%7D%29%20%5CBig%29%20%5CBig%28%5Csum_t%20r%28s_%7Bi%2Ct%7D%2C%20a_%7Bi%2Ct%7D%29%5CBig%29)

<!-- 3) Then gradient asent 
$\theta \leftarrow  \theta + \eta \nabla_ \theta J(\theta)$ -->

3. To ultimately update the parameters of the policy network: 

![equation](https://latex.codecogs.com/gif.latex?%5Ctheta%20%5Cleftarrow%20%5Ctheta%20&plus;%20%5Ceta%20%5Cnabla_%20%5Ctheta%20J%28%5Ctheta%29)


## Using cost to go to reduce variance

So far I have explained the basic reinforcement algorithm. This explained algorithm learns by using ALL the rewards that it gets during that entire episode. Since the &#960;<sub>t'</sub> cannot affect the rewards that occurred before _t_, it results in a high variance method of learning. 

 To reduce the variance, we can make use of the future rewards after an action is performed, such that the agent performs based on future rewards only. 

This means that the reinforce algorithm becomes:

![equation](https://latex.codecogs.com/gif.latex?%5Cnabla%20J%20%28%5Ctheta%29%20%3D%20%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bi%3D1%7D%5EN%20%5Csum_%7Bt%3D1%7D%5ET%20%5Cnabla_%5Ctheta%20%5Clog%5Cpi_%5Ctheta%28a_%7Bi%2Ct%7D%20%7C%20s_%7Bi%2Ct%7D%29%5Chat%7BQ%7D_%7Bi%2Ct%7D)

Where the Q-hat is the 'cost-to-go':

![equation](https://latex.codecogs.com/gif.latex?%5Chat%7BQ%7D_%7Bi%2Ct%7D%20%3D%20%5Csum_%7Bt%27%20%3D%20t%7D%5ET%20r%28s_%7Bi%2Ct%27%7D%2C%20a_%7Bi%2Ct%27%7D%29)


which is the collection of all the future rewards, after a performed action at _t_.

## To summarize

For each episode, the agent gets to run on the plateau. For each timestep in the episode, the policy takes in a state, and outputs different probabilities of actions, and one action is then randomly sampled (the actions with higher probability scores have a higher probability to be sampled). This happens until the agent falls off the plateau.

We then look at each timestep of that episode, starting at the first timestep. At the first timestep, we sum all the rewards following that timestep, and then update the policy parameters of that timestep, such that the policy output has a positive effect on the total future reward (this we call gradient ascend).

 Then for the next timestep of that same episode, we again sum its future rewards, and again the parameters of the policy network is updated based on that sum. When repeating this with many episodes, the policy output becomes more optimal at each timestep and the episodes become longer.

### Reference

I have learned all of this material in the <a href="https://www.ru.nl/courseguides/socsci/courses-osiris/ai/sow-mki49-neural-information-processing-systems/">Neural Information Processing Systems</a> course, this year! 