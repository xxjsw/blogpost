---
title: 1. Introduction
layout: default
nav_order: 1
---
# A deep learning model for gas storage optimization

**Deep learning**, a subset of machine learning inspired by the structure and functionality of the human brain, has demonstrated extraordinary capabilities in handling intricate tasks. The inherent ability of deep learning to automatically learn and adapt from large amounts of data, coupled with their capacity to uncover subtle patterns, now leverage this technique to address challenges in many fields, ranging from computer vision and natural language processing to finance and healthcare, which were deemed hardly conquerable by conventional methods. The technical efficiency of deep learning marks a paradigm shift in problem-solving approaches and paving the way for innovation and discovery across a multitude of disciplines.

This blog post introduces the application of deep learning in the field of quantitative risk management in conformity with the paper - [A deep learning model for gas storage optimization]. The paper was published in 2021 from the working group of [Josef Teichmann] from ETH Zürich, whose research in recent years mainly focuses on machine learning in finance mathematics. In this article, they aimed to optimize the operation plans of underground natural gas storage facilities. Given the inherent complexity arising from the high-dimensional forward market, along with numerous constraints and frictions, conventional methods face substantial obstacles in handling such optimization tasks. Therefore, they tried to propose a theoretical framework to output strategy networks by virtue of deep learning and evaluated the numerical performance of the suggested method against the state-of-the-art [least-squares Monte-Carlo] approach. These breakthroughs create opportunities for enhanced energy storage and production strategies applicable to broad, non-Markovian markets. 

The blog post is structured as follows: In the subsequent section, a concise overview of algorithmic trading and quantitative risk management within the gas market will be presented, aiming to furnish basic domain knowledge and clarify the relevant issues that need to be tackled. Following this, the Sect.3 will elaborate on the mathematical formalization of the problem and provide a brief introduction to conventional methods and their limitations in addressing such tasks. The Sect.4 encapsulates the core of the blog post, introducing the concept of “**deep hedging**” and illustrating how this concept acts as the driving force for two distinct frameworks, each crafted to suit different scenarios: a straightforward intrinsic spot trading and a more complex one encompassing both spot and forward trading. Within this section, emphasis will be placed on describing the training setup. Then the Sect. 5 will discuss the validation results in comparison with the benchmark strategies in terms of terminal p&l as well as overall runtime performance. 


----



[A deep learning model for gas storage optimization]: https://arxiv.org/abs/2102.01980
[Josef Teichmann]: https://people.math.ethz.ch/~jteichma/
[least-squares Monte-Carlo]: https://www.informs-sim.org/wsc07papers/107.pdf
