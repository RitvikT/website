+++
draft = false
title = 'Research'
+++

> "I regard as quite useless the reading of large treatises of pure analysis: too large a number of methods
> pass at once before the eyes. It is in the works of applications that one must study them; one judges their 
> ability there and one apprises the manner of making use of them."
> — J.-L. Lagrange

My research interests lie at the interface of PDEs and probability, with a focus on **interacting particle systems** (IPS), where collective behavior emerges from local randomness in ways that reflect both the dynamics and the geometry of the underlying interactions. 

Consider a collection of diffusive particles $\{X_v^{G,\xi}\}_{v \in V}$, indexed by the nodes of a graph $G = (V, E)$, with square-integrable initial conditions $\{\xi_v\}_{v \in V}$, evolving according to the
coupled system of SDEs
$$\mathrm{d}X_v^{G,\xi}(t) = b\left(t, X_v^{G,\xi}(t), \mu_v^{G,\xi}(t)\right)\mathrm{d}t + \sigma\left(t, X_v^{G,\xi}(t), \mu_v^{G,\xi}(t)\right)\mathrm{d}W_v(t), \quad X_v^{G,\xi}(0) = \xi_v,$$
where $\{W_v\}_{v \in V}$ are i.i.d. standard Brownian motions independent of $\{\xi_v\}_{v \in V}$,
with sufficiently regular coefficient functions $b, \sigma : \mathbb{R}_+ \times \mathbb{R}^d \times \mathcal{P}(\mathbb{R}^d) \to \mathbb{R}^d$,
and $\mu_v^{G,\xi}(t)$ is the local empirical measure of $v$ defined as
$$\mu_v^{G,\xi}(t) := \frac{1}{|\partial_G(v)|} \sum_{u \in \partial_G(v)} \delta_{X_u^{G,\xi}(t)},$$
where the neighborhood is denoted by $\partial_G(v)$. The choice of graph $G$ distinguishes my two research programs. 
- When $G = K_N$ is the **complete graph**, every particle interacts with the full ensemble, and the system can be leveraged as a powerful computational tool for optimization and sampling. 
- When $G$ is a **sparse graph** (i.e. an Erdős–Rényi graph), each particle interacts with only $\mathcal{O}(1)$ neighbors, and the large-particle behavior becomes a fundamentally harder analytical problem.

---

## IPS for Optimization and Sampling

Optimization and sampling are quintessential tasks in applied mathematics, where the model of interest $\mathcal{G}$ is **expensive** to evaluate (e.g., solving a PDE), and derivatives of $\mathcal{G}$
are **unavailable**, ruling out classical gradient descent and Langevin sampling. These tasks arise from formulating the Bayesian inverse problem, where one wishes to
recover a parameter $\theta \in \mathbb{R}^d$ from data $y \in \mathbb{R}^m$, with
$\mathcal{G} : \mathbb{R}^d \to \mathbb{R}^m$ and $\eta \sim \mathcal{N}(0,\Gamma)$:
$$y = \mathcal{G}(\theta) + \eta.$$
For a Gaussian prior $\mathbb{P}(\theta)=\mathcal{N}(\theta_0,\Sigma)$, the posterior is
$$\mathbb{P}(\theta \mid y) \propto \exp\!\left(-\tfrac{1}{2}\lVert y-\mathcal{G}(\theta)\rVert_{\Gamma}^{2} - \tfrac{1}{2}\lVert \theta-\theta_0\rVert_{\Sigma}^{2}\right) =: \exp\big(-\Phi(\theta)\big),$$
where $\lVert x \rVert_{A}^{2} := \langle x, A^{-1}x\rangle$. Two natural questions arise:
1. **Optimization:** Find the MAP estimate (the maximizer of $\mathbb{P}(\theta \mid y)$).
2. **Uncertainty Quantification:** Sample from the posterior $\mathbb{P}(\theta \mid y)$.

Particle-based methods have emerged as a natural answer to both, as they avoid gradient
evaluations, are parallelizable, and enjoy convergence guarantees via mean-field analysis. My work in this direction focuses on **algorithmic design and convergence analyses** of such methods, an example of which is the ensemble Kalman sampler (EKS) that runs a system of $N$ particles $\{\theta_t^i\}_{i=1}^N$ with dynamics
$$\mathrm{d}\theta_t^i = -\mathcal{C}(\mu_t^N) \cdot \nabla \Phi(\theta_t^i) \, \mathrm{d}t + \sqrt{2 \mathcal{C}(\mu_t^N)} \, \mathrm{d}W_t^i,$$
where $\mathcal{C}(\mu_t^N)$ is the empirical covariance. The covariance preconditioning is what allows EKS to avoid explicit gradient evaluations of $\mathcal{G}$, making it especially suitable for the derivative-free setting. 

One can also investigate the mean-field ($N \to \infty$) Fokker-Planck PDE of the above dynamics as 
$$\partial_t\rho = \nabla \cdot (\rho \cdot \mathcal{C}[\rho] \cdot \nabla \Phi(u)) + \mathcal{C}[\rho] : D^2\rho,$$
which opens a host of insightful questions regarding the geometry of covariance-modulated flows to be explored with techniques from optimal transport and PDE analysis. 

---

## IPS on Sparse Graphs

To provide intuition, a thermometer works because water is a **mean-field** system since every molecule interacts equally with all others, so the bulk behavior reduces to that of a single "average" particle. Mathematically, on a complete graph $K_N$, the empirical measure $\mu_t^N$ concentrates around a **deterministic** limit $\mu_t \in \mathcal{P}(\mathbb{R}^d)$ in the mean-field limit, and the long-time behavior can be studied via classical PDE and functional-analytic tools (e.g., log-Sobolev inequalities, entropy methods).

Real-world networks, however, are rarely complete. Social networks, neural circuits,
and communication graphs are **sparse**: each particle $i$ interacts only with its
neighbors $\partial_{G_N}(v)$ in a graph $G_N$, so the relevant empirical measure for
particle $i$ is the **local empirical measure**
$$\mu_v^{G_N,\xi}(t) := \frac{1}{|\partial_{G_N}(v)|} \sum_{u \in \partial_{G_N}(v)} \delta_{X_u^{G_N,\xi}(t)},$$
rather than the global $\mu_t^N$. When $G_N$ is sparse, the degree $|\partial_{G_N}(v)| = \mathcal{O}(1)$, and
the local empirical measure **remains a random object** even in the mean-field limit, which is the fundamental difficulty of the sparse setting. The limiting object is an infinite-dimensional, tree-indexed stochastic process, and classical tools no longer apply. This raises several natural questions that my work investigates:

1. **Long-time asymptotics:** Does the system converge to a stationary distribution as
$t \to \infty$? On complete graphs, entropy methods and log-Sobolev inequalities give
quantitative rates. On sparse graphs, stationarity is not even well-posed at the level
of individual particles, and one must instead ask whether the **tree-indexed process**
admits an invariant measure, and if so, at what rate it is approached.

2. **Large deviations:** How does $\mu_t^{i,N}$ fluctuate around its typical behavior,
and what is the cost of rare events? On complete graphs, the large deviation rate
function is given by a **relative entropy** functional. On sparse graphs, the
combinatorial structure of the graph itself contributes to the rate function, and
deriving a sharp LDP requires understanding the interplay between the graph geometry
and the particle dynamics.

These two questions are deeply intertwined, as large deviation principles often govern
the exponential rate at which the system escapes metastable states, shedding light on
which equilibria are truly stable in the long run. Further, these are of utmost importance 
in modeling to accurately capture the likelihood of rare, but catastrophic events. 

