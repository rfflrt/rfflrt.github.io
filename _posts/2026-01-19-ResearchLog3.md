---
layout: post
title: Riemannian Geometry-Based Machine Learning on Multi-Class Multi-Subject Semantic Classification of THINGS-EEG
date: 2026-02-20
categories: research
math: "true"
---
***UNFINISHED LOG***
After testing the EEGNet performance on THINGS-EEG for end-to-end semantic decoding, I approached the task and dataset with classical ML algorithms, such as KNN, MDM and SVM.<br>
Clearly, this approach is not done on the EEG signal matrix, but on its covariance matrix. This approach generates competitive results compared to Deep Learning models by leveraging more robustness of the covariance matrix in terms of noise, that is, second order statistics are more reliable than the signal alone.

## Riemannian Methods
The approach described has nothing different from regular algorithms if run on the covariance matrices without any special treatment. Let me explain briefly the structure underlying this type of data.
### SPD Matrices
The signal matrix of a stimuli is given by $$M\in \mathbb{R}^{C\times T},$$
where $C$ is the quantity of channels (brain electrodes) and $T$ are the quantity of time samples.<br>
The covariance matrix of a signal is given by $$\mathcal{C}=MM^T,$$
and, since $T>>C$ and the float representation of the potential at each electrode, it is very likely (sometimes called *empirically proved*) that the covariance matrices are **Simetric Positive Definite** (SPD, for short), living in the SPD ($\mathcal{S}_{++}^{\mathcal{C}}$) curved space.<br>
Since it is a curved space, it is true that given two SPD matrices $A, B \in \mathcal{S}_{++}^{n}$ and a scalar $a \in \mathbb{R}$ it is possible that $$A+B\notin \mathcal{S}_{++}^{n},$$ $$aA\notin \mathcal{S}_{++}^{n}.$$