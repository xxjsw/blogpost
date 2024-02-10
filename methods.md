---
title: 4. Methodology
layout: default
nav_order: 4
---

This section will center on the framework introduced in the paper, which aims to optimize gas storage by means of deep hedging. It will begin with how to customize neural networks in order to develop hedging strategies, elucidating the concept of deep hedging. Following that, attention will be directed towards the two frameworks - SMod and SFMod, each tailored to specific scenarios: one focusing solely on spot trading, while the other adopts a more complex approach involving forward trading. Then an in-depth presentation of the training setup for the two scenarios of varying complexity will encompass crucial elements for neural network training setup including datasets, network architecture, loss functions, optimization algorithms and hyperparameters.

## 4.1 Deep Hedging
Hedging is a risk management strategy for protecting against market volatility and uncertainty, allowing investors to safeguard their portfolios and achieve more stable returns. For example, in gas trading, a company that anticipates a decrease in gas price may hedge its position by purchasing futures contracts to lock in a more favorable price. This strategy helps mitigate potential losses if prices decline, ensuring more predictable revenue streams for the company. 

Traditional methods for developing hedging strategies, such as statistical analysis and derivatives pricing methods, rely on historical data and heavy mathematical equations to identify risks and formulate positions. These methods may struggle to capture complex market patterns and nonlinear relationships, leading to less accurate predictions. In contrast, deep learning offers a powerful tool for processing large datasets and learning complex patterns directly from the data, making them more robust and adaptive in dynamic market conditions.

![NN](figs/feedforwardnn.png)

## 4.2 SMod: intrinsic spot trading
### 4.2.1 Scene Setting
### 4.2.2 Training Setup

## 4.3 SFMod: intrinsic spot and forward trading
### 4.3.1 Scene Setting
### 4.3.2 Training Setup
---