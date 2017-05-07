---
layout: post
title:  "Cooperative Reinforcement Learning"
date:   2017-05-07 18:10:00 +0200
categories: cooperative
---

Some researchers from Stanford compared DQN/DDPG (off-policy) with TRPO
(on-policy) on a multi-agent problem. The state-transition tuples are obviously
dependent on both environment and current policy, thus making data on-policy.
Strange to examine off-policy algorithms on this kind of data? See article
[here][stanford-coop].


[stanford-coop]: http://ala2017.it.nuigalway.ie/papers/ALA2017_Gupta.pdf
