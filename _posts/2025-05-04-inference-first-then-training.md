---
layout: post
title: From Song & Zhou - Ideas in Inference-time Scaling can Benefit Generative Pre-training Algorithms
date: 2025-05-04 21:25:00
description: how Inference-time Scaling can Benefit Generative Pre-training Algorithms
tags: inference time scaling n generative pre-training algorithms
categories: sample-posts
chart:
  plotly: true
---

Yesterday, I came across an article by Jiaming Song and Linqi Zhou, two renowned researchers in the field of diffusion models. This post will summarise their ideas and share my own thoughts and commentary on their work.

TL,DR:

- Overall, their argument is that an inference-time methodology that focuses on scaling efficiency via `sequence length (in LLMs)` or `refinement steps (in diffusion models)` can inspire novel generative pre-training algorithms.

- For example, their recent work, **Inductive Moment Matching (IMM)**, addresses the inference efficiency challenges of diffusion models and, coincidentally, offers a stable single-stage algorithm that achieves superior sample quality.

Key Ideas:

- The dominance of autoregressive models for discrete signals and diffusion models for continuous signals has led to a stagnation in the development of new approaches.

- For novel and practical generative pre-training algorithms or models, one should consider two approaches: 

  - (i) develop algorithms that `scale nicely in the two axes (sequence length and sampling steps) during inference`. 
  - (ii) `design inference algorithm before training time` to optimally utilize the model capacity at inference time (much like thinking how they will generate data)`.

- Can we design such generative pre-training algorithms or models for mixed-modality data? Reflect on the inference perspective first.

- A list of current generative pre-training algorithms:

| Scalability                      | Methods                                                                                                                                             |
|----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| ❌ Sequence Length<br>❌ Refinement Steps | VAE [KW13], GAN [GPAM+14], Normalizing Flows [RM15]                                                                                                 |
| ✅ Sequence Length<br>❌ Refinement Steps | GPT [BMR+20], PixelCNN [vdOKK16], MaskGiT [CZJ+22], VAR [TJY+25]                                                                                     |
| ❌ Sequence Length<br>✅ Refinement Steps | Diffusion models [HJA20], Energy-based models [DM19], Consistency models [SDCS23],<br>Parallel nonlinear equation solving [SMLE21]                 |
| ✅ Sequence Length<br>✅ Refinement Steps<br>(Outer loop: Sequence Length) | AR-Diffusion [WFL+23], Rolling diffusion [RHSH24], MAR [LTL+25],<br>Blockwise parallel decoding [SSU18]                                             |
| ✅ Sequence Length<br>✅ Refinement Steps<br>(Outer loop: Refinement Steps) | Autoregressive distribution smoothing [MSS+21]                                                                                                       |

- Two instances of fixing inference algorithms: DDIM sampler by conditioning on the future time steps to correct the trajectory and the naïve conditional independence assumption in multi-token prediction.