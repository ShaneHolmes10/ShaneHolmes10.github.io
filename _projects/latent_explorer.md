---
layout: page
title: Latent Explorer
description: Unsupervised Latent Space Visualizer — PyTorch training pipeline with interactive browser-based inference
img: assets/img/latent-explorer-thumbnail.jpg
importance: 4
category: work
---

## Overview

An end-to-end tool for learning and visualizing the latent structure of image datasets. Train a decoder to compress images into a low-dimensional representation, apply PCA to discover the most meaningful axes of variation, then explore what the model learned — live in the browser, no server required. This project was inspired by this [video](https://www.youtube.com/watch?v=4VAkrUNLKSo) by CodeParade, and his accompanying project [here](http://codeparade.net/faces/).

The full source is available on [GitHub](https://github.com/ShaneHolmes10/latent-explorer).

---

## The Challenge

The interesting question in generative modelling isn't just whether a model can reconstruct images — it's what structure it discovers in the process. A model trained on faces doesn't explicitly learn concepts like lighting direction, age, or expression. But if the latent space is well-organised, those concepts emerge as geometric structure: directions you can walk in to smoothly change one attribute while leaving others untouched. The challenge was building a system that makes that structure legible and interactive.

---

## What I Built

A full-stack ML project spanning training infrastructure, model architecture, and a browser-based interactive interface:

**Training Pipeline**
- Built a **convolutional decoder** trained on ~202k face images from the CelebA dataset at 128×128 resolution
- Implemented a **PCA post-processing stage** that reframes the 80-dimensional latent space in terms of principal components — ranked by variance, making the most meaningful axes of variation immediately accessible
- Designed a flexible model registry: new architectures plug in automatically to the browser-based interactive interface

**Interactive Desktop GUI**
- Built a **real-time exploration interface in Python** using sliders for each latent dimension
- Per-dimension lock/unlock, random sampling from the learned distribution, and reset to any reference image

**Browser Interface**
- Exported trained models to **ONNX format** and built a pure-JavaScript web app running inference entirely in the browser via **ONNX Runtime Web** — no backend, no API calls
- Handles ONNX external data files (split model format) and supports per-model default vectors loaded from JSON
- Deployed as a static site embeddable in any webpage

<iframe
  src="https://shaneholmes10.github.io/latent-explorer/web/element/"
  width="100%"
  height="600"
  style="border: none; border-radius: 4px;"
  title="Latent Explorer — Live Demo"
></iframe>

*Use the sliders to navigate the latent space. Lock dimensions to hold them fixed while randomising the rest. Other models can be selected from the drop down to get an idea of the history of performance.*

Several observations emerge from the results of this project. When the "Decoder 580 epochs" model is selected, no clear semantic correspondence is observed between individual components of the latent vector and visual attributes of the generated images. This outcome is expected, as the training objective imposes no constraint that would encourage the latent dimensions to be statistically independent or aligned with interpretable semantic factors. Without such a constraint, variation along any single axis tends to be entangled with variation along others, obscuring any one-to-one mapping between dimensions and visual attributes.

The "PCA Decoder" model addresses this directly. By applying a PCA transformation to the same 580-epoch decoder, the latent space is reparameterized into an orthogonal basis whose components are mutually uncorrelated and ordered by descending variance. Adjusting D00, the first principal component, produces a clear and consistent change in image brightness, indicating that global luminance accounts for the largest source of variation in the generated outputs. The remaining components, however, do not exhibit strong semantic correspondence comparable to that reported in CodeParade's [project](http://codeparade.net/faces/). One relevant thing to note is that ([Shen et al., 2020](https://arxiv.org/abs/1907.10786)) and ([Shen & Zhou, 2021](https://arxiv.org/abs/2007.06600)) show that facial features are linearly separable in a latent space. I believe that the backgrounds for these images are introducing either too much complexity and/or non-linearity and the model is struggling to deal with it. 

In future iterations of this project, I want to either normalize the images to exclude the background or to build a more complex model to handle the complicated data.

---

## Technical Stack

| Component | Technology |
|---|---|
| Training framework | PyTorch |
| Dataset | CelebA (~202k images) |
| Model architecture | Convolutional decoder + PCA |
| Latent space | 80 dimensions |
| Dimensionality reduction | PCA (scikit-learn) |
| Model export | ONNX (opset 17) |
| Browser inference | ONNX Runtime Web |
| Web interface | Vanilla JS, HTML/CSS |
| Desktop GUI | Python (tkinter) |
| Data pipeline | HDF5 |

---

## Takeaways

Training the model is the easy part. The more interesting problem is making the learned representation legible. Raw latent coordinates are numerically arbitrary — PCA reorders them by variance, which turns an opaque 80-dimensional space into something you can actually explore systematically. Exporting to ONNX and running inference in the browser removes the last barrier: anyone can interact with the model directly without needing Python or a GPU.

---

*Apr 2026 – Present · Personal Project*
