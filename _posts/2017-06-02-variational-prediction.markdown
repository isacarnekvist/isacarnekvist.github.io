---
layout: post
title:  "Variational Model Prediction"
date:   2017-06-02 20:30:00 +0200
categories: variational model prediction
---

![Test]({{ site.url }}/assets/model_prediction_pgm.svg)

Given a set of past state, action, and draw from the latent
variable $$Z_t$$, we should be able to construct the successor state $$x_{t+1}$$:

\\[ D(x_t, a_t, z_t \| \theta_D) = x_{t+1} \\]

We can also go backwards, that is encode the transition to the latent space $$z_t$$:

\\[ E(x_t, a_t, x_{t+1} \| \theta_E) = z_{t} \\]

\\[ \mathcal{L} = KL\left( E(x_t, a_t, x_{t+1}) \middle\|\middle\| \mathcal{N}(0, 1) \right)  - \log p(x_{t+1}\|x_t, a_t, z_t) \\]

We can instead replace the negative log-likelihood with an euclidean reconstruction error:

\\[ \mathcal{L}\_R = \left\| D\left(x_t, a_t, E(x_t, a_t, x_{t+1})\right) - x_{t+1} \right\|^2_2 \\]


[dropout-bayesian]: https://arxiv.org/abs/1506.02142
