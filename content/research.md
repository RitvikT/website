+++
draft = true
title = 'Research'
+++

My research interests are broadly in applied probability theory, in particular studying the *microscopic* "random" behavior of an interacting particle system (IPS) can be organized to recover specific *macroscopic* behaviors of the ensemble. I broadly outline some of my current projects below in two programs, algorithmically and probabilistically. 

## IPS for Optimization and Sampling

Optimization and sampling are quintessential tasks in applied mathematics, both of
which arise naturally from solving inverse problems. In applications, $\mathcal{G}$
is **expensive** to evaluate (e.g., solving a PDE), and derivatives of $\mathcal{G}$
are **unavailable**.

These tasks arise from formulating the Bayesian inverse problem, where one wishes to
recover a parameter $\theta \in \mathbb{R}^d$ from data $y \in \mathbb{R}^m$, with
$\mathcal{G} : \mathbb{R}^d \to \mathbb{R}^m$ and $\eta \sim \mathcal{N}(0,\Gamma)$.
$$y = \mathcal{G}(\theta) + \eta$$
For a Gaussian prior $\mathbb{P}(\theta)=\mathcal{N}(\theta_0,\Sigma)$, the posterior
$\mathbb{P}(\theta \mid y)$ can be written
$$\mathbb{P}(\theta \mid y) \propto \exp\left(-\tfrac{1}{2}\lVert y-\mathcal{G}(\theta)\rvert_{\Gamma}^{2} - \tfrac{1}{2}\rVert \theta-\theta_0\rvert_{\Sigma}^{2}\right) = \exp\big(-\Phi(\theta)\big),$$
where $\lVert x \rVert_{A}^{2} := \langle x, A^{-1}x\rangle$. Some natural questions are:
\begin{enumerate}
\item **Optimization:** Find the MAP estimate (the maximizer of $\mathbb{P}(\theta \mid y)$).
\item **Uncertainty Quantification:** Sample from the posterior $\mathbb{P}(\theta \mid y)$.
\end{enumerate}

## IPS for Sparse G