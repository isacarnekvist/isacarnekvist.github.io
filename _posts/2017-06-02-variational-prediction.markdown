---
layout: post
title:  "Variational Model Prediction"
date:   2017-06-02 20:30:00 +0200
categories: variational model prediction
---

What kind of features do you want in a predictive model, in my case for RL?

1. High quality predictions, a representative model with accurate long-term predictions.
2. Sample predicted trajectories from the model
3. Samples should be from model uncertainty due to a complex, or unpredictable stochastic environment
4. Samples should also represent model uncertainty from missing data, this should preferrably make
   the model trainable, and valuable for RL purposes, even on small amounts of data


# Available methodologies
## 1. High quality predictions, a representative model with accurate long-term predictions.

For games and pixel prediction, [cnn's][video-pred-oh] and [recurrent
simulators][recurrent-sim] have been used with really impressive results. These
do prediction in a low-dimensional representation of the state. A question here
is; what if your observations are already low-dimensional? These results does
not show in particular any modelling of contact dynamics, this was however done
with a version of [WaveNet][wavenet], found [here][temporal-control].

## 2. Sample predicted trajectories from the model

The paper with [prediction and control with temporal segment
models][temporal-control] have nice ideas and good results, but have some
theoretical flaws.  During inference of future trajectories, the latent
dimension $$Z$$ is unknown, and sampling $$\mathcal{N}(0, I)$$ is not sound since if there
exists only one correct future trajectory, this has a corresponding latent space probably smaller
than a spherical gaussian.

## 3. Samples should be from model uncertainty due to a complex, or unpredictable stochastic environment

In progress.

# Graphical model, and an objective function

Sampling proper future trajectories should come from a distribution which is
conditioned on past states and actions (for non-markovian systems). We define a graphical model as follows:

![PGM]({{ site.url }}/assets/model_prediction_pgm.svg)

In our case, the $$X$$ can be seen as an initial state and action, $$Z$$ as
some latent stochastic variable that depends on the state we're in along with
what action we take. Inuitively, it makes sense that the stochasticity in an
environment depends on where you are, and what you do. The variable $$Y$$ is
the successor state coming from and performing the joint state and action
$$X$$. What we want to maximize is:

$$
\begin{align*}
    \log p(Y|X) &= \log \int_Z p(Y, Z|X) \\ 
                &\geq \int \log p(Y, Z|X) & \text{\{Jensen's inequality\}}\\
                &= \int \log p(Y, Z|X) \frac{q(Z|X)}{q(Z|X)} \\
                &= \mathbb{E}_{q(Z|X)} \left[  \frac{\log p(Y, Z|X)}{q(Z|X)} \right] \\
                &= \mathcal{L}
\end{align*}
$$

Starting from KL-divergence ($$\mathcal{D}$$), we can exactly derive the variational lower bound $$\mathcal{L}$$:

$$
\begin{align*}
    \mathcal{D}\left[ q(Z|X) || p(Z|Y, X) \right] &= \int_Z q(Z|X) \log \frac{q(Z|X)}{p(Z|Y, X)} \\
                                                  &= -\int_Z q(Z|X) \log \frac{p(Z|Y, X)}{q(Z|X)} \\
                                                  &= -\int_Z q(Z|X) \log \frac{p(Z, Y| X)}{p(Y|X)q(Z|X)} \\
                                                  &= -\int_Z q(Z|X) \left[ \log \frac{p(Z, Y| X)}{q(Z|X)} - \log p(Y|X) \right]\\
                                                  &= -\int_Z q(Z|X) \log \frac{p(Z, Y| X)}{q(Z|X)} + \int_Z q(Z|X) \log p(Y|X) \\
                                                  &= -\mathbb{E} \left[ \log \frac{p(Z, Y| X)}{q(Z|X)} \right] + \log p(Y|X) \\
                                                  &= -\mathcal{L} + \log p(Y|X)\\
\end{align*}
$$

Rearranging gives:

$$
    \mathcal{L} = \log p(Y|X) - \mathcal{D}\left[ q(Z|X) || p(Z|Y, X) \right]
$$

A reasonable thing to do from here is to let
$$ \log p(Y|X) = -\text{MSE}(f(X), Y)$$, where $$f$$ is some predictive model,
and let the prior
$$p(Z|Y, X) \sim \mathcal{N}(0, I)$$.
By maximizing these together, we maximize the variational lower bound $$\mathcal{L}$$.
Once converged, we have access to a distribution $$q$$ which is, as we required,
conditioned on $$X$$ and from which we can generate future trajectories one step at a time.

# Notes regarding experiments

- Do not model the goal as a part of the state if not necessary! Prediction
  might gradually change this over time, taking you into parts of the state
  space where a different goal (never seen) might influence the dynamics
  predictions.

<!--


Given a set of past state, action, and draw from the latent
variable $$Z_t$$, we should be able to construct the successor state $$x_{t+1}$$:

\\[ D(x_t, a_t, z_t \| \theta_D) = x_{t+1} \\]

We can also go backwards, that is encode the transition to the latent space $$z_t$$:

\\[ E(x_t, a_t, x_{t+1} \| \theta_E) = z_{t} \\]

\\[ \mathcal{L} = KL\left( E(x_t, a_t, x_{t+1}) \middle\|\middle\| \mathcal{N}(0, 1) \right)  - \log p(x_{t+1}\|x_t, a_t, z_t) \\]

We can instead replace the negative log-likelihood with an euclidean reconstruction error:

\\[ \mathcal{L}\_R = \left\| D\left(x_t, a_t, E(x_t, a_t, x_{t+1})\right) - x_{t+1} \right\|^2_2 \\]

-->


[wavenet]: https://arxiv.org/abs/1609.03499
[video-pred-oh]: https://arxiv.org/abs/1507.08750
[recurrent-sim]: https://arxiv.org/abs/1704.02254v2
[temporal-control]: https://arxiv.org/abs/1703.04070
[dropout-bayesian]: https://arxiv.org/abs/1506.02142
