---
title: 5. Results
layout: default
nav_order: 5
---
This section presents the experimental results obtained from evaluating two frameworks and compare them with the benchmark least-squares Monte Carlo methods. The analysis aims to provide insights into the performance and effectiveness of the proposed frameworks in comparison to the state-of-the-art methods, shedding light on their potential in addressing challenges within the complex gas market.

## 5.1 SMod
### Overall Runtime
The paper does not provide specific performance data for training the framework. Instead, it states that through various experiments, the authors evaluated training time based on factors including network depth, number of distinct neural networks, learning rates, and batch size, such empirical fine-tuning aimed to identify suitable settings for SMod. Furthermore, it turn out that it is unnecessary to build K(number of trading days) neural networks, but a higher parameter N(number of distinct neural network, $N \leq K$, as parameter sharing is allowed) accelerates the learning process of the required solution complexity. They concluded that training generally completed quickly and was manageable on a standard 8-core notebook. For example, training SMod with K neural networks and 1000 epochs on 6000 scenarios took less than 2 hours. A key advantage of the proposed deep learning approach is its flexibility in scheduling strategy network training. Additionally, trained strategies can be promptly evaluated within seconds, which is comparable to that of the LSMC approach, highlighting the approach's efficiency.
### Terminal P&L
![SMod p&l](figs/SMod/p&l.png)
The above figure illustrates the p&l distribution comparison between the spot-only and benchmark models across both training and validation sets. Following 1000 epochs over 6000 scenarios, utilizing a learning rate of 0.001, a batch size of 64, and a risk aversion rate of $\gamma=3$, the artificial financial agent's strategy approaches the benchmark solution notably. 


## 5.2 SFMod
### Overall Runtime
### Terminal P&L
![SFMod p&l](figs/SFMod/p&l.png)
![comparison](figs/SFMod/comparison.png)
---