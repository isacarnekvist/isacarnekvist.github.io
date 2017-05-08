---
layout: post
title:  "Magneto Loss for Dropout Networks"
date:   2017-05-08 09:08:00 +0200
categories: dropout
---

NOTE: This is a crazy idea that in general will now work very well :)

Using dropout networks [(link)][dropout-bayesian] trained with mean squared error loss on multimodal
data, you get this result:

![Test]({{ site.url }}/assets/mse-result.svg)

What I would want from a network I sample from is of course that the samples
are from the same distribution as the target distribution.  Instead of having
MSE as loss, I tried a loss function where the error decays for points further
away from the prediction:

\\[\frac{1}{N}\sum_{i=1}^N\frac{(\hat{y_i} - y_i)^2}{e^{\gamma(\hat{y_i} - y_i)^2}}\\]

The loss then gets this appearance:

![Test]({{ site.url }}/assets/magneto-curve.svg)

This will however not work since the network can also lower its loss by pushing
points such that MSE \\(\rightarrow \infty\\). If you have nice data however,
you can at least fit it to one of the modes:

![Test]({{ site.url }}/assets/magneto-result.svg)

[dropout-bayesian]: https://arxiv.org/abs/1506.02142
