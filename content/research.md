+++
draft = true
title = 'Research'
+++

My research interests are broadly in applied probability theory, in particular studying the *microscopic* random behavior of an interacting particle system (IPS) can be organized to recover specific *macroscopic* behaviors of the ensemble. Formally, in the diffusion setting, the dynamics on a graph $G$ can be specified by a system of $N$ coupled SDEs.
$$\mathrm{d}X_{t}^{i,N} = b(X_{t}^{i,N}, \mu_{t}^{N}) \ \mathrm{d}t + \sigma(X_{t}^{i,N}, \mu_{t}^{N}) \ \mathrm{d}W_{t}^{i}\ ,$$
with sufficiently regular $\sigma, b : \mathbb{R}^d \times \mathcal{P}(\mathbb{R}^d) \rightarrow \mathbb{R}^d$ and empirical measure $\mu_{t}^{N} := \frac{1}{N} \sum_{i=1}^{N} \delta_{X_{t}^{i,N}}$.

This is of practical interest in settings where one would like to estimate random processes influenced by 

I broadly outline some of my current projects below in two distinct programs, which are investigating . 

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