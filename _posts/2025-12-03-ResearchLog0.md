---
layout: post
title: "Current Research Introduction"
date: 2025-12-03
categories: research
---

On my first post on this personal page, I briefely mentioned I was interested on EEG decoding using CNNs.
This interest happened to become my first research project, oriented by Dr. Nina S. T. Hirata and funded by FAPESP, started on August 2025.<br>
This post is the first about this research, explaining what I want to explore and discover. I intend to make logs of the experiments and progresses, even though some already happened.
I will make one or two posts about what I have already done.

## Research Description
The research abstract, as written on FAPESP's project, is:

> Brain decoding developed as a promising manner to develop brain computer interfaces (BCIs), aimed mainly to the interpretation of neural signals relative to tasks such as motor imagery and assisted spelling, used primarily by people physically limited. The use of convolutional neural networks (CNNs) emerged as an alternative to methods based on Riemannian geometry, motivated by the success of such networks in the field of computer vision. Hence, more complex tasks were tested using this approach, being the classification of viewed or imagined objects through brain signals one of them. However, challenges such as accuracy and cross-subject generalization still persist. Riemannian methods present good results on both aspects and are still focus of active research.<br> This project aims to investigate and compare the performance of CNNs on those questions, analyzing a possible semantic pairing across subjects, contributing to the development of neural decoding approaches.

Therefore, what we wanted to explore in this research project is to see how well CNNs can decode semantic information form EEG signals, using other methods as benchmark and as inspiration for the models and data exploration. Also, looking forward to practical implementation and usage, how the models generalize across different subjects and how much fine-tuning and data processing is needed to do so.<br> 

At the moment, after exploring the EEGNet on some data - which I will explain in other logs - we are aiming to explore other data processing and models, adding riemannian geometry to the models itself, considering the EEG capture paradigm and other information that can be added to the inductive bias.<br>

For other posts, the talking will be less informative in the sense it will resemble more of my internal reasoning and thoughts in contrast to the expositive this one, and probabbly the next, will have.