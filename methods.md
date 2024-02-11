---
title: 4. Methodology
layout: default
nav_order: 4
mathjax: true
---

This section will center on the framework introduced in the paper, which aims to optimize gas storage by means of deep hedging. It will begin with how to customize neural networks in order to develop hedging strategies, elucidating the concept of deep hedging. Following that, attention will be directed towards the two frameworks - **SMod** and **SFMod**, each tailored to specific scenarios: one focusing solely on spot trading, while the other adopts a more complex approach involving forward trading. Both of them are based on intrinsic valuation, which means assessing the fundamental worth of traded assets rather than short-term market fluctuations or speculative factors. Then an in-depth presentation of the training setup for the two scenarios of varying complexity will encompass crucial elements for neural network training setup including datasets, network architecture, loss functions, optimization algorithms and hyperparameters.

## 4.1 Deep Hedging
[Hedging] is a risk management strategy for protecting against market volatility and uncertainty, allowing investors to safeguard their portfolios and achieve more stable returns. For example, in gas trading, a company that anticipates a decrease in gas price may hedge its position by purchasing futures contracts to lock in a more favorable price. This strategy helps mitigate potential losses if prices decline, ensuring more predictable revenue streams for the company. 

Traditional methods for developing hedging strategies, such as statistical analysis and derivatives pricing methods, rely on historical data and heavy mathematical equations to identify risks and formulate positions. These methods may struggle to capture complex market patterns and nonlinear relationships, leading to less accurate predictions. In contrast, deep learning offers a powerful tool for processing large datasets and learning complex patterns directly from the data, making them more robust and adaptive in dynamic market conditions.

<br/>
![deepRL](figs/deeprl.png)
The above graph shows the basic architecture of [deep reinforcement learning]. A popular approach within deep reinforcement learning centers on using (feed-forward) neural networks to approximate optimal actions, as neural networks are well-suited for such intricate tasks due to their versatility and efficient training capabilities. Then the storage optimization tasks should be reformulated such that it fits the structure of neural networks. Approximate each action $$h_k$$ in terms of a deep neural network $$g^\theta _k$$, parameters $$\theta$$ of these network strategies $$G^\theta = \{g_0^\theta, ...,g_{K-1}^\theta\}$$ are trained to maximize an estimate of the expected terminal utility, i.e., to solve $$\text{ max}{}_\theta \mathbb{E}_\mathbb{P}[U(W_{G^\theta})]$$. 

It is noteworthy that, in the formulation of their frameworks, as opposed to the depicted reinforcement learning architecture(see above graph), they avoided computing intermediate value functions and evaluating actions in states with low likelihood of occurrence. Such an approach significantly decreases the computational complexity. Furthermore, their frameworks are not bounded by temporal consistency requirements or by expected reward specifications.

![NN](figs/feedforwardnn.png)
### Definition: Feed-Forward Neural Network
Let $$L\in \mathbb{N}$$, a feed-forward neural network $${g}^\theta$$ is defined as $$A^L \circ \phi \circ A^{L-1} \circ ... \circ \phi \circ  A^1(x)$$, where
* $$L\in \mathbb{N}$$ - number of layers                                                              
* $$\phi(\cdot)$$ -￼non-linear activation function
* $$A^l, l=1,...,L$$ -￼affine linear maps in the respective dimensions, whose parameters are stored in $$\theta$$

## 4.2 SMod: intrinsic spot trading
### 4.2.1 Scene Setting
**Spot trading**: immediate purchase or sale of financial instruments or commodities for instant delivery at the current market price. In the context of gas trading, day-ahead price can be seen as close proxy of the spot trading.

Following variables are required to build the neural networks:

| Variable|  Interpretation|
| :----------- |: ----------- |
| $$s_k$$ | the $$\mathbb{F}$$-adapted gas spot price| 
| $${h}^S_k$$|  action on day $$k$$ | 
| $${H}^S=\{h^S_0,{h}^S_1,...,{h}^S_{K-1}\}$$| a trading strategy over the whole trading horizon| 
| $$({H}^s \cdot S)_{K-1} = \sum_{k=0}^{K-1}{h}^S_ks_k$$ | terminal value |  
| $${H}^S_n= \sum_{k=0}^{n-1}{h}^S_k$$ |storage level on day n (Initial condition: $${H}^s_0=0 $$) | 
| $$U(x)=(1-{e}^{-\gamma x})/\gamma$$ | an exponential utility function with risk aversion rate $$\gamma \in \mathbb{R}^+$$ (A higher risk aversion rate indicates a stronger preference for avoiding risk or uncertainty when making financial decisions.)| 


The optimization goal should be maximizing expected terminal utility $$\mathbb{E}_\mathbb{P}[U(W_{K-1})]$$ over all eligible $${H}^s$$ where $$W_{K-1}=-({H}^s \cdot S)_{K-1} = \sum_{k=0}^{K-1}-{h}^s_ks_k$$ denotes the terminal p&l.

Meanwhile, this optimization process must adhere to the following constraints:
1. $${H}^S_K =0$$, i.e., empty final storage(intuitive for any profit-seeking agents)
1. For all $$0 \leq {H}^S_k\leq c$$ (storage level on any day must not exceed storage capacity $$c$$) and daily action constraints $$l_k^* \leq {h}^S_k\leq u_k^*$$, where $$l_k^* = \text{max}\{l_k,-H_k^S\}$$ and $$u_k^* = \text{min}\{u_k,c-H_k^S\}$$. The withdrawal and injection rates on each trading day must be limited by the storage capacity $$c$$, storage level $${H}^S_K$$ and a predefined lower and upper bounds $$l_k$$ and $$u_k$$ compatible with the seasonal demands and supply patterns.
<br/><br/>
![limits](figs/limits.png)


### 4.2.2 Training Setup
<br/>

**-Training Data**

Time horizon of storage $$T=\{0, 1, 2, ..., K-1\}$$, $$M$$ trajectories of the spot price $$({S^i}_k)_{k \in T; i=1,...,M}$$

_Data provider: [Expo Solutions AG] in forms of $$M=10000$$ scenarios (6000 for training and 4000 for validation) and $$K=351$$ trading days, benchmark strategies as well._

**-Training Object**

**Input**: time $$k$$, current spot price $$S_k$$ and the latest storage fill level $$H^S_k$$, which iteratively depends on the previous network outputs.

**Output**: storage action(withdraw or injection rate) over the whole storage horizon, that is a neural network consisting of $$N \in \mathbb{N}$$ ($$N \leq K$$, as parameter sharing is allowed) distinct sub-networks whose output should be $${h}^S_k$$, each of which has $$L$$ layers.

**-Training Criterion**

Minimize an estimate of expected negative utility over batches $$B \subset \{1,...,M\}$$ of training data with standard Adam stochastic gradient descent, i.e., $$\text{min} \frac{1}{\|B\|}\sum_{i \in B}{-U({W}^i_{K-1})}$$

**-Implementation**

_TENSORFLOW.KERAS_ with _sigmoid_ activation function

**-Hardware**

A standard 8-core notebook
  
## 4.3 SFMod: intrinsic spot and forward trading
### 4.3.1 Scene Setting
**spot and forward trading**: extend the previous spot-only model by trading additionally on the front month rolling forwards with delivery period of a whole month.

![forward](figs/SFMod/rolling_strategy.png)

Compared to the scenarios outlined in SMod, in this context, it is imperative to aggregate the outcomes of forward trading on each trading day and consider the cumulative impact from that. They made following assumption for the **monthly forward contract**: It is only traded before its delivery period starts and no longer during the delivery period.

The above visualization shows the forward trading mechanisms. The arrow points to the current timeframe. The black box refers to the spot trading activites and the green box refers to the forward trading activities. In SFMod, let $$0=n_0 < n_1< ... < n_J <K$$ be the first days of the months $$\mathbb{J}=\{0, 1, ..., J\}$$ respectively, let $${h}^j_k$$ with $$j \in \mathbb{J}$$ be the action on day $$k$$ on the forward $$F(k, n_j, n_{j+1}-1)$$ whose delivery obligation is during the period $$[n_j, n_{j+1}-1]$$, then the action on day $$k$$ is $$({h}^S_k+{d}^j)$$ for $$ n_j \leq k \leq n_{j+1}$$, which combines both spot trading and forward trading. Of particular importance is that forward trading activities have a delayed effect on the spot trading in the following month, while spot trading does not affect forward trading. After the respective forward trading has already terminated, the delivery quantities of the upcoming days in the current month are fixed, but the spot trading activities of the current month is limited by the due forwards, as the sum of the spot trading and thedaily delivery quantities must not exceed the daily withdrawl and injection limits.

To describe this scenario, which integrates forward trading and thus becomes more complex, merely requires expanding the variables previously defined in SMOD:

| Variable|  Interpretation|
| :----------- |: ----------- |
| $$h^j_k$$ | the action on day $$k$$ on the forward $$F(k, n_j, n_{j+1}-1)$$ with the deliver period [n_j, n_{j+1}-1]| 
| $$d^j=\sum_{k=n_{j-1}}^{n_j-1}h^j_k $$|  daily delivery quantity fixed on day $$n_j-1$$, and $$d^0=0$$| 
| $$H_n= \sum^{n-1}_{k=0}h^S_{k} + \sum^{I-1}_{j=1}(d^j(n_{j+1}-n_j))+(d^{I-1}(n-n_{I-1}+1))$$ for $$n \in [n_{I-1}, n_I]$$| the storage level depends on both spot and monthly forward trading activities(Initial condition: $${H}_n=0 $$) | 

The terminal p&l generated from forward trading must also be considered as optimization objectives. In this context, the objective function expands to $$\mathbb{E}_\mathbb{P}[U(W^S_{K-1}+W^F_{K-1})]$$, where $$W^F_{K-1}=\sum^{J-1}_{j=1}\sum^{n_j-1}_{k=n_{j-1}}(-h^j_{k}F(k, n_j, n_{j+1}-1)(n_{j+1}-n_j))$$ denotes the terminal p&l from trading the monthly forward.


In addition to the two constraints mentioned in SMod, a regularization term is required to scale the balance between spot trading and forward trading: $$h_k^j \leq \alpha \frac{c}{n_{j+1}-n_j}$$ with the scaling factor $$\alpha \in [0,1]$$. When $$\alpha=0$$, it aligns with SMod. As $$\alpha$$ increases, the upper bound of forward trading becomes larger. Such a constraint is known as a "[liquidity constraint]". Liquidity in finance refers to the ease and speed with which assets can be bought or sold without significantly affecting their prices. Spot trading impacts liquidity by providing immediate access to assets, facilitating quick buying and selling transactions, thus enhancing market liquidity. On the other hand, forward trading can impact liquidity by diverting trading activity away from spot markets, potentially reducing immediate liquidity but providing opportunities for longer-term risk management.

### 4.3.2 Training Setup
The training setup for **SFMod** closely resembles that of **SMod**, with the spot trading component remaining consistent and introducing adjustments solely to incorporate the forward trading component.
<br/>

**-Training Data**

Time horizon of storage $$T=\{0, 1, 2, ..., K-1\}$$, $$M$$ trajectories of the spot price $$({S^i}_k)_{k \in T; i=1,...,M}$$ and of rolling month forward $$(F^i_k)_{k \in \mathbb{T}; i=1,...,M}$$ respectively

_Data provider: [Expo Solutions AG] in forms of $$M=10000$$ scenarios (6000 for training and 4000 for validation) of spot as well as monthly forward curves of 12 months), $$K=351$$ trading days and benchmark strategies_

**-Training Object**

**Input**: time $$k$$, current spot price $$S_k$$, forward $$F_k$$ and the latest storage fill level $$H_k$$, which iteratively depends on the previous network outputs.

**Output**: for each trading day $$k$$, trading strategy network with two-dimensional outputs for spot action $$h^S_k$$ and action in the rolling month forward $$h^j_k$$ respectively, consisting of $$N \in \mathbb{N}$$($$N \leq K$$, as parameter sharing is allowed) distinct sub-networks, each of which has $$L$$ layers.

**-Training Criterion**

Minimize an estimate of expected negative utility over batches $$B \subset \{1,...,M\}$$ of training data with standard Adam stochastic gradient descent, i.e., $$\text{min} \frac{1}{\|B\|}\sum_{i \in B}{-U({W}^{i,S}_{K-1}+{W}^{i,F}_{K-1})}$$

**-Implementation**

_TENSORFLOW.KERAS_ with _sigmoid_ activation function

**-Hardware**

A standard 8-core notebook


---
[Deep reinforcement learning]: https://www.semanticscholar.org/paper/Semisupervised-Deep-Reinforcement-Learning-in-of-Mohammadi-Al-Fuqaha/6b72f4d16532f4e8f5ddd7640262019eea641fd6

[Expo Solutions AG]: https://www.exposolutions.de/messebauwelt.html

[liquidity constraint]: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=1151569

[Hedging]: https://www.jstor.org/stable/3666367
