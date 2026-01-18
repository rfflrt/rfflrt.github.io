---
layout: post
title: Multi-Class Classification on Variable Number of Subjects Using EEGNet
date: 2026-01-16
categories: research
math: "true"
---
This post will summarize the results achieved using variable number of subjects and different parameters on EEGNet on THINGS-EEG dataset. Here, we will show how increasing the amount of data changes the network performance and possible inferences.

## RSVP Paradigm
The RSVP paradigm, in which THINGS-EEG was constructed upon, is short for *"Rapid Serial Visualization Paradigm"*. This mode of collecting data is, making it simple, various images presented sequentially, with duration of (but can be different) 100 ms each image.
The subject is prompted a class and, after the presentation, says if the class was present in the images visualized. This makes possible for the dataset to be larger due to the reduction of sampling and stimulus time.
Nonetheless, during a LEARN meeting (the student research group I participate in) Victor Martins, who has significant understanding of neuroscience, asked questions about contamination of the data by other images. Even though the subject is looking for a certain class, the other images are also processed and, therefore, contaminates the stimuli captured.
This is a problem, since CNNs learns filters that comprises all the signal and, therefore, may generate activations for other classes that may merge on the signal processing. But, on the dense part of the network, it may be treated.
Although, the preprocessing of the data, reducing 1000 time samples to 100, even though reduces the processing power and removes artifacts, loses a lot of information, including those more relevant to the processing and classification by the network. Therefore, I may experiment on the unprocessed THINGS-EEG, removing artifacts without that much downsampling.