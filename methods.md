---
title: 4. Methodology
layout: default
nav_order: 4
mathjax: true
---

This section will center on the framework introduced in the paper, which aims to optimize gas storage by means of deep hedging. It will begin with how to customize neural networks in order to develop hedging strategies, elucidating the concept of deep hedging. Following that, attention will be directed towards the two frameworks - SMod and SFMod, each tailored to specific scenarios: one focusing solely on spot trading, while the other adopts a more complex approach involving forward trading. Both of them are based on intrinsic valuation, which means assessing the fundamental worth of traded assets rather than short-term market fluctuations or speculative factors. Then an in-depth presentation of the training setup for the two scenarios of varying complexity will encompass crucial elements for neural network training setup including datasets, network architecture, loss functions, optimization algorithms and hyperparameters.

## 4.1 Deep Hedging
Hedging is a risk management strategy for protecting against market volatility and uncertainty, allowing investors to safeguard their portfolios and achieve more stable returns. For example, in gas trading, a company that anticipates a decrease in gas price may hedge its position by purchasing futures contracts to lock in a more favorable price. This strategy helps mitigate potential losses if prices decline, ensuring more predictable revenue streams for the company. 

Traditional methods for developing hedging strategies, such as statistical analysis and derivatives pricing methods, rely on historical data and heavy mathematical equations to identify risks and formulate positions. These methods may struggle to capture complex market patterns and nonlinear relationships, leading to less accurate predictions. In contrast, deep learning offers a powerful tool for processing large datasets and learning complex patterns directly from the data, making them more robust and adaptive in dynamic market conditions.

A popular approach within deep reinforcement learning centers on using (feed-forward) neural networks to approximate optimal actions, as neural networks are well-suited for such intricate tasks due to their versatility and efficient training capabilities. Then the storage optimization tasks should be reformulated such that it fits the structure of neural networks. Approximate each action $$h_k$$ in terms of a deep neural network $$g^\theta _k$$, parameters $$\theta$$ of these network strategies $$G^\theta = \{g_0^\theta, ...,g_{K-1}^\theta\}$$ are trained to maximize an estimate of the expected terminal utility, i.e., to solve $$\text{ max}{}_\theta \mathbb{E}_\mathbb{P}[U(W_{G^\theta})]$$

![NN](figs/feedforwardnn.png)
### Definition: Feed-Forward Neural Network
Let $$L\in \mathbb{N}$$, a feed-forward neural network $${g}^\theta$$ is defined as $$A^L \circ \phi \circ A^{L-1} \circ ... \circ \phi \circ  A^1(x)$$, where
* $$L\in \mathbb{N}$$ - number of layers                                                              
* $$\phi(\cdot)$$ -￼non-linear activation function
* $$A^l, l=1,...,L$$ -￼affine linear maps in the respective dimensions, whose parameters are stored in $$\theta$$

## 4.2 SMod: intrinsic spot trading
### 4.2.1 Scene Setting
**Spot trading**: immediate purchase or sale of financial instruments or commodities for instant delivery at the current market price. In the context of gas trading, day-ahead price can be seen as close proxy of the spot trading

### 4.2.2 Training Setup

## 4.3 SFMod: intrinsic spot and forward trading
### 4.3.1 Scene Setting

### 4.3.2 Training Setup
---