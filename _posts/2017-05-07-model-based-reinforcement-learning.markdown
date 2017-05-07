---
layout: post
title:  "Model-Based Reinforcement Learning"
date:   2017-05-07 11:16:32 +0200
categories: model
---
Model-based should increase the information of the environment by explicitly
predicting the dynamics and maybe also the rewards. These could be used for
both training a policy on simulated data, and for tree-search during inference.
Even though this is the case, they don't seem to be used that much, why is that?
Especially for real-robotic learning these could enable more data-efficient
algorithms. Also, thinking about how I work, I constantly reason about effects
of my actions and build hypotheses about the consequences to my actions.

Questions:
- How to use model-based RL for non-stationary environments, e.g. environments
  with multiple learning agents where training naturally becomes on-policy.
- Is it possible to use drop-out networks that are heteroscedastic without
  assumption on the distribution? For example multi-modal, etc.

Related work:
- [Pilco paper][pilco]
- [Sparse GP][sparse-gp]
- [Yarin Gal Blog (dropout networks)][yarin-gal]
- [Dropout networks][dropout-bayesian]

[pilco]: http://mlg.eng.cam.ac.uk/pub/pdf/DeiRas11.pdf
[sparse-gp]: http://www.gatsby.ucl.ac.uk/~snelson/SPGP_up.pdf
[dropout-bayesian]: https://arxiv.org/abs/1506.02142
[yarin-gal]: http://mlg.eng.cam.ac.uk/yarin/blog.html
