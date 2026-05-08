---
layout: post
title: Riemannian Geometry-Based Machine Learning on Multi-Class Multi-Subject Semantic Classification of THINGS-EEG
date: 2026-02-20
categories: research
math: "true"
---

After testing the EEGNet performance on THINGS-EEG for end-to-end semantic decoding, I approached the task and dataset with classical ML algorithms, such as KNN, MDM and SVM.<br>
Clearly, this approach is not done on the EEG signal matrix, but on its covariance matrix. This approach generates competitive results compared to Deep Learning models by leveraging more robustness of the covariance matrix in terms of noise, that is, second order statistics are more reliable than the signal alone.

## Riemannian Methods
The approach described has nothing different from regular algorithms if run on the covariance matrices without any special treatment. Let me explain briefly the structure underlying this type of data.

### SPD Matrices
The signal matrix of a stimuli is given by $$M\in \mathbb{R}^{C\times T},$$
where $C$ is the quantity of channels (brain electrodes) and $T$ are the quantity of time samples.

The covariance matrix of a signal is given by $$\mathcal{C}=MM^T,$$
and, since $T>>C$ and the float representation of the potential at each electrode, it is very likely (sometimes called *empirically proved*) that the covariance matrices are **Simetric Positive Definite** (SPD, for short), living in the SPD ($$\mathcal{S}_{++}^{\mathcal{C}}$$) curved space.

Since it is a curved space, it is true that given two SPD matrices $A, B \in \mathcal{S}_{++}^{n}$ and a scalar $a \in \mathbb{R}$ it is possible that $$A+B\notin \mathcal{S}_{++}^{n},$$ $$aA\notin \mathcal{S}_{++}^{n}.$$

This means that regular Euclidean operations on those matrices are not well suited for their treatment. Therefore, we treat $\mathcal{S}_{++}^{n}$ as a **Riemannian manifold**.

### Log-Exp Mapping

The key tool to work on this curved space is the pair of mappings that connect the manifold to its local tangent space (which is flat, i.e., Euclidean).

Given a reference point $P \in \mathcal{S}_{++}^{n}$, the **Exponential map** $\text{Exp}_P: T_P\mathcal{M} \rightarrow \mathcal{S}_{++}^{n}$ takes a tangent matrix $S$ back onto the manifold,

$$\text{Exp}_P(S) = P^{1/2}\exp(P^{-1/2}SP^{-1/2})P^{1/2},$$

and its inverse, the **Logarithmic map** $\text{Log}_P: \mathcal{S}_{++}^{n} \rightarrow T_P\mathcal{M}$, projects a manifold matrix $Q$ onto the tangent space at $P$,

$$\text{Log}_P(Q) = P^{1/2}\log(P^{-1/2}QP^{-1/2})P^{1/2}.$$

This allows us to temporarily "unfold" the manifold locally and perform Euclidean operations in tangent space, then map results back.

### Riemannian Mean

Since the space is curved, the notion of mean must also be adapted. The **Fréchet mean** (or geometric mean) of a set of SPD matrices $\{\mathcal{C}_i\}_{i=1}^{N}$ is defined as

$$\bar{\mathcal{C}} = \arg\min_{M \in \mathcal{S}_{++}^{n}} \sum_{i=1}^{N} d(M, \mathcal{C}_i)^2.$$

There is no closed-form solution in general, so this is computed iteratively. Starting from an initial point, at each step the current estimate $M_k$ is updated by mapping all matrices to the tangent space at $M_k$, computing the Euclidean mean of the tangent matrices, and mapping the result back to the manifold:

$$M_{k+1} = \text{Exp}_{M_k}\left(\frac{1}{N}\sum_{i=1}^{N}\text{Log}_{M_k}(\mathcal{C}_i)\right).$$

This is repeated until convergence.

### MDM, KNN and SVM on the Manifold

With a proper notion of distance and mean, classical algorithms can be adapted straightforwardly.

**MDM** (Minimum Distance to Mean) computes one Fréchet mean per class using the iterative procedure above, then assigns a test sample to the class whose mean is closest.

**KNN** the $k$ nearest neighbors are found using the geodesic distance $d(\cdot, \cdot)$ instead of Euclidean distance.

**SVM** using a linear kernel on the log-mapped points on the mean, using OVO or OVA multi-classification.

## xDAWN + SVM

The best performing pipeline was **xDAWN spatial filtering** followed by **SVM** on the Riemannian-projected covariance matrices. xDAWN [[Rivet et al., 2009](https://ieeexplore.ieee.org/document/5154296)] is a supervised spatial filter originally designed for P300 detection that finds a set of spatial filters $W$ maximizing the signal-to-signal-plus-noise ratio (SSNR) with respect to a target response. Concretely, it solves a generalized eigenvalue problem

$$\Sigma_{\hat{A}} W = \Sigma_{X} W \Lambda,$$

where $\Sigma_{\hat{A}}$ is the covariance of the projected target response and $\Sigma_{X}$ is the covariance of the full signal. The filtered signal $\tilde{M} = W^T M$ has reduced dimensionality and, crucially, enhanced class-relevant structure.

xDAWN + SVM outperformed the other methods, which is consistent with the idea that better spatial filtering reduces the variance in covariance estimation and makes the manifold geometry more exploitable.

While MDM and KNN got about 0.35 accuracy, even with xDAWN filtering - that is below EEGNet's performance I acessed - the SVM got 0.621 accuracy under an OVA regime, outperforming EEGNet.

Testing the winning pipeline on a increasing number of subjects, it shows some degradation, varying from 0.594 to 0.670 accuracy, not linearly. This variance may tinker on subjects biased neural processation and noise, so I did not give it much importance. Nonetheless, it is expected to have a smaller accuracy witha  agreat number of subjects, but which will encompass a more "general semantic".