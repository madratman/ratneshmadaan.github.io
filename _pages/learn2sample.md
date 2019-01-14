---
title: "Learning Non-Stationary Sampling Distributions for Robot Motion Planning"
permalink: /projects/learn2sample
author_profile: false
---

Ratnesh Madaan\*, Sam Zeng\*, Brian Okorn, Rogerio Bonatti<br/>
\*_Equal Contribution_<br/>
<!-- This was the most fun and productive course project during my Masters, for  -->
[16-831 Statistical Techniques in Robotics](https://piazza-syllabus.s3.amazonaws.com/j6e1dluhfx87f8/RoboStats_16_831_Syllabus_Fall_20176.pdf?) & [16-782 Planning and Decision-making in Robotics](http://www.cs.cmu.edu/~maxim/classes/robotplanning_grad/), Fall 2017. <br/>
[Report](https://ratneshmadaan.github.io/files/learn2sample_course_project.pdf),
[Videos](https://drive.google.com/drive/u/1/folders/1llWLtbF7IiIhOAdnTTrOBJOgi9Lmt4LE),
[Slides - 16-782](https://docs.google.com/presentation/d/1sBc4q5Kv9zcjhsiyo42cBNqK6wUrj9dvKu6H8JmvYL8/view?usp=sharing), 
[Slides - 16-831](https://docs.google.com/presentation/d/1UfaseawKhyFu42TSZVjzaYJUQtq6m6JIxUsnyiM9Gws/view?usp=sharing),<br/>
Code - Modified OMPL (C++), 
Code - Training Pipeline (Python) <br/>
<!-- [Code - Modified OMPL (C++)](https://github.com/ratneshmadaan/ompl_learn2sample),  -->
<!-- [Code - Training Pipeline (Python)](https://github.com/ratneshmadaan/learn2sample_python), <br/> -->

Here, we learn an adapting sampling distribution for RRTs(Rapidly Exploring Random Trees), which is conditioned on both the workspace environment and the instantaneous planning graph by using CVAEs(Conditional Variational Auto-Encoders). 
This project is inspired by two recent papers : Ichter et. al, [Learning Sampling Distributions for Robot Motion Planning](https://arxiv.org/abs/1709.05448) (ICRA 2018) and Bhardwaj et al., [Search as Imitation Learning](https://mohakbhardwaj.github.io/SaIL/) (CoRL 2017).  
Ichter et al. learn a static distribution by conditioning on the workspace environment. 
Bhardwaj et al. learn an adaptive heuristic policy for search based planners, by imitating an oracle which has full access to the world at train time. 
We combined these two ideas, by conditioning the distribution both on the planning graph and the environment, and used [DAGGer](https://arxiv.org/abs/1011.0686) for training the policy. 

The first key challenge here is to define what is the optimal sample given a partially solved planning tree. 
Given the tree corresponding to a partially solved planning problem, we generate a set of optimal samples which have collision free connections to the tree, and have low cost to goal starting from the sample (obtained by computing backward Dijkstra distance from goal to start). 
The second challenge lies in extracting a feature representation from the ever increasing in size random tree. 
For the purposes of the course project, we used a 2-stream convolutional CVAE which takes both the image of the environment and the image of the instantaneous tree (!) and outputs the optimal sample. 
 
Although we demonstrated results on a 2D holonomic problem, we are working towards generalizing our framework for higher dimensional planning problems, and will be submitting to CoRL 2018. 

In each video, the start state is bottom left of image, the goal state is top right of the image, blue shoes the tree progress, the distribution is shown by fitting a Kernel Density Estimator on samples from the CVAE. The sample itself is seen in yellow <br/>

Implementation Notes: We exposed OMPL's RRT to a Python via an interface, such that it can send the graph to Python at each iteration, where a CVAE uses the graph and the environment, does its magic and returns the sample, which is then used by OMPL's RRT. <br/>

