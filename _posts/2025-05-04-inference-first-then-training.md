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

Yesterday, I came across an insightful article by Jiaming Song and Linqi Zhou, both are leading researchers pushing the boundaries of generative modelling. This blog post summarises their main arguments, explores some key concepts, and includes my own reflections on how their work may shape the future of generative pre-training.

---

## 🔍 TL;DR

- **Main Idea**: Inference-time efficiency—scaling along *sequence length* (in LLMs) and *refinement steps* (in diffusion models)—should inform the *design of generative pre-training algorithms*.
- **Highlighted Contribution**: Their recent work, **Inductive Moment Matching (IMM)**, introduces a stable, single-stage generative method that outperforms traditional diffusion models in sample quality while also improving inference efficiency.

---

## 🧠 Key Insights from the Article

### 1. Breaking the Stagnation in Generative Models  
- **Observation**: Discrete data is dominated by *autoregressive models*; continuous data by *diffusion models*.
- **Problem**: This paradigm has reached a plateau—limiting exploration into new architectures.

### 2. Two Principles for Future Generative Algorithms  
To build more practical and scalable generative pre-training algorithms:
- **(i)** **Design to scale at inference time** in both **sequence length** and **refinement steps**.
- **(ii)** **Start with the inference algorithm**, and design training to optimise that inference (i.e., *think about how the model will generate* before you train it).

### 3. Mixed-Modality Generation: A Call to Action  
- The inference-first mindset might be particularly valuable in designing **unified models for mixed modalities** (e.g., image + text + audio).
- **Question posed**: Can we invent *scalable, efficient* pre-training methods for such settings?

---

## 📊 Scalability Matrix of Generative Models

| **Scalability**                                                            | **Methods**                                                                                                                        |
|----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| ❌ Sequence Length<br>❌ Refinement Steps                                   | VAE [KW13], GAN [GPAM+14], Normalizing Flows [RM15]                                                                                |
| ✅ Sequence Length<br>❌ Refinement Steps                                   | GPT [BMR+20], PixelCNN [vdOKK16], MaskGiT [CZJ+22], VAR [TJY+25]                                                                   |
| ❌ Sequence Length<br>✅ Refinement Steps                                   | Diffusion [HJA20], Energy-based [DM19], Consistency [SDCS23], Parallel Equation Solving [SMLE21]                                  |
| ✅ Sequence Length<br>✅ Refinement Steps<br>*(Outer: Sequence)*            | AR-Diffusion [WFL+23], Rolling Diffusion [RHSH24], MAR [LTL+25], Blockwise Parallel Decoding [SSU18]                              |
| ✅ Sequence Length<br>✅ Refinement Steps<br>*(Outer: Refinement)*          | Autoregressive Distribution Smoothing [MSS+21]                                                                                     |

---

## 🛠 Examples of Fixing the Inference Algorithm

Two notable strategies where inference design influences training:
- **DDIM Sampler**: Uses future-time conditioning to refine generation trajectories, e.g., by modeling \( v_{\theta}(x_t, t, s) \), where \( s \) is a future time step.
- **Multi-token Prediction**: Often assumes conditional independence, which simplifies inference but may limit expressiveness.

---

## 💭 My Thoughts and Takeaways

- Generative algorithms are fundamentally about predicting the next X. Here, X could be the next token in language models (LLMs), the next sample at a future timestep in diffusion models, or the next scale in methods like VAR (NeurIPS 2024). Recently, a paper titled "Next-X Prediction" proposed a generalisation of this idea, unifying various forms of X—including tokens, cells, subsamples, images, and scales—under a single framework. From a slightly different perspective, models like AlphaEvolve can be viewed as predicting the next program or algorithm, while GFlowNets or Mamba correspond to next action-state prediction. MCP, on the other hand, focuses on selecting the next action over a planning horizon.

- In essence, all generative algorithms share one core principle: predicting the next one.

- To design new generative algorithms, we can think in terms of "next-X*-prediction"—where X* is any unit of generation—and explore whether the approach scales well across dimensions such as time, resolution, or abstraction. This idea can also be extended to higher levels, such as predicting at the distributional level (e.g., cluster or segment) or at the probabilistic level (e.g., generating 80% of X and 20% of Y). 

- What about discriminative algorithms? Yann LeCun often champions these over generative ones. However, I believe they can—and should—be integrated. Discriminative insights (e.g., object relationships in images or structural patterns in text) can enrich generative models by informing the structure and coherence of the next-X* prediction. This combination will be very helpful in the multimodal setting.

- My thought is that for text-image models, X* can be smartly designed so that the text must be aligned with the image throughout the generation process.