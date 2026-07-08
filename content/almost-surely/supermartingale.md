+++
draft = false
title = "Supermartingales"
subtitle = "and why life should be unfair."
date = 2026-07-02
+++

I will start this blog post with some technical stuff (oops!) by defining a continuous-time supermartingale and explain the broad intuition of this object. For all times $s \geq t$, a supermartingale is one in which 
$$\mathbb{E}[X_t \mid \{X_{\tau}\}_{\tau \leq s}] \leq X_s$$

Intuitively, this says that the state of the process tends to decrease over time
