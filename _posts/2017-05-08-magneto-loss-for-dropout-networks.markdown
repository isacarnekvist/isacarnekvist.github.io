---
layout: post
title:  "Magneto Loss for Dropout Networks"
date:   2017-05-08 09:08:00 +0200
categories: dropout
---

Using dropout networks [(link)][dropout-bayesian] trained with mean squared error loss on multimodal
data, you get this result:

![Test]({{ site.url }}/assets/mse-result.svg)

Instead of having MSE as loss, I tried a loss function where the error decays for
points further away from the prediction:

\\[\frac{1}{N}\sum_{i=1}^N\frac{(\hat{y_i} - y_i)^2}{e^{\gamma(\hat{y_i} - y_i)^2}}\\]

The loss then gets this appearance:

![Test]({{ site.url }}/assets/magneto-curve.svg)

Unfortunately, the predictions instead gets stuck on one of the modes:

![Test]({{ site.url }}/assets/magneto-result.svg)


Maybe including the variance in the loss somehow can alleviate this?

[dropout-bayesian]: https://arxiv.org/abs/1506.02142
