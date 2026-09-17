+++
draft = false
title = 'Research'
+++

> "I regard as quite useless the reading of large treatises of pure analysis: too large a number of methods
> pass at once before the eyes. It is in the works of applications that one must study them; one judges their 
> ability there and one apprises the manner of making use of them."
> — J.-L. Lagrange

My research interests lie at the interface of PDEs and probability, with a focus on **interacting particle systems** (IPS), where collective behavior emerges from local randomness in ways that reflect both the dynamics and the geometry of the underlying interactions.

Consider a collection of diffusive particles $\{X_v^{G,\xi}\}_{v \in V}$, indexed by the nodes of a graph $G = (V, E)$, with square-integrable initial conditions $\{\xi_v\}_{v \in V}$, evolving according to the coupled system of SDEs
$$\mathrm{d}X_v^{G,\xi}(t) = b\left(t, X_v^{G,\xi}(t), \mu_v^{G,\xi}(t)\right)\mathrm{d}t + \sigma\left(t, X_v^{G,\xi}(t), \mu_v^{G,\xi}(t)\right)\mathrm{d}W_v(t), \quad X_v^{G,\xi}(0) = \xi_v,$$
where $\{W_v\}_{v \in V}$ are i.i.d. standard Brownian motions independent of $\{\xi_v\}_{v \in V}$, with sufficiently regular coefficient functions $b, \sigma : \mathbb{R}_+ \times \mathbb{R}^d \times \mathcal{P}(\mathbb{R}^d) \to \mathbb{R}^d$, and $\mu_v^{G,\xi}(t)$ is the local empirical measure of $v$:
$$\mu_v^{G,\xi}(t) := \frac{1}{|\partial_G(v)|} \sum_{u \in \partial_G(v)} \delta_{X_u^{G,\xi}(t)}.$$
The choice of graph $G$ distinguishes my two research programs: when $G = K_N$ is the **complete graph**, every particle interacts with the full ensemble, and the system can be leveraged as a powerful computational tool for optimization and sampling; when $G$ is **sparse**, each particle interacts with only $\mathcal{O}(1)$ neighbors, and the large-particle behavior becomes a fundamentally harder analytical problem with real-world applications.

<figure style="text-align: center; margin: 2em auto; max-width: 700px;">
<img src="/graphs.png" style="width: 100%; border-radius: 6px;">
<figcaption style="margin-top: 0.6em; color: #888; font-style: italic; font-size: 0.9em;">
Complete, dense, and sparse Erdős–Rényi graphs. (<a href="https://arxiv.org/pdf/2401.00082" target="_blank">Ramanan (2024)</a>)
</figcaption>
</figure>

---

## IPS for Optimization and Sampling

Optimization and sampling are quintessential tasks in applied mathematics that arise naturally from solving inverse problems. The central difficulty is that the forward model $\mathcal{G}$ is typically **expensive** to evaluate (e.g., requires solving a PDE) and its derivatives are **unavailable**, ruling out classical gradient descent and Langevin sampling. Concretely, one wishes to recover a parameter $\theta \in \mathbb{R}^d$ from data $y \in \mathbb{R}^m$ via the observation model
$$y = \mathcal{G}(\theta) + \eta, \qquad \eta \sim \mathcal{N}(0,\Gamma).$$
For a Gaussian prior $\mathbb{P}(\theta)=\mathcal{N}(\theta_0,\Sigma)$, the posterior takes the form
$$\mathbb{P}(\theta \mid y) \propto \exp\!\left(-\tfrac{1}{2}\lVert y-\mathcal{G}(\theta)\rVert_{\Gamma}^{2} - \tfrac{1}{2}\lVert \theta-\theta_0\rVert_{\Sigma}^{2}\right) =: \exp\big(-\Phi(\theta)\big),$$
where $\lVert x \rVert_{A}^{2} := \langle x, A^{-1}x\rangle$. This immediately raises two tasks:

1. **Optimization:** Find the MAP estimate, i.e., the minimizer of $\Phi$.
2. **Uncertainty Quantification:** Sample from the posterior $\mathbb{P}(\theta \mid y)$.

Particle-based methods have emerged as a natural and scalable answer to both, since they require no gradient evaluations, are embarrassingly parallelizable, and enjoy convergence guarantees via mean-field analysis. Two canonical examples are the **ensemble Kalman inversion** (EKI) and **ensemble Kalman sampler** (EKS), which run a system of $N$ particles $\{\theta_t^i\}_{i=1}^N$ with dynamics
$$\mathrm{d}\theta_t^i = -\mathcal{C}(\mu_t^N) \cdot \nabla \Phi(\theta_t^i) \, \mathrm{d}t + \begin{cases} 0 & \text{(EKI)} \\ \sqrt{2\mathcal{C}(\mu_t^N)} \, \mathrm{d}W_t^i & \text{(EKS),} \end{cases}$$
where $\mathcal{C}(\mu_t^N)$ is the empirical covariance of the ensemble. The covariance preconditioning replaces explicit gradient evaluations of $\mathcal{G}$ with ensemble statistics, making both methods well-suited to the derivative-free setting: EKI drives the ensemble toward the MAP estimate, while EKS introduces diffusion to explore the full posterior. The mean-field ($N \to \infty$) limit of EKS satisfies the covariance-modulated Fokker-Planck PDE
$$\partial_t\rho = \nabla \cdot (\rho \cdot \mathcal{C}[\rho] \cdot \nabla \Phi) + \mathcal{C}[\rho] : D^2\rho,$$
whose geometry is intimately connected to optimal transport and opens rich questions about long-time convergence to the posterior $\exp(-\Phi)$. My work in this direction focuses on the **algorithmic design and convergence analysis** of such methods.

<figure style="text-align: center; margin: 2em auto; max-width: 500px;">
<img src="/CBORastrigin.gif" style="width: 100%; border-radius: 6px;">
<figcaption style="margin-top: 0.6em; color: #888; font-style: italic; font-size: 0.9em;">
Simulation of consensus-based optimization. (<a href="https://en.wikipedia.org/wiki/Consensus_based_optimization" target="_blank">Wikipedia</a>)
</figcaption>
</figure>

---

## IPS on Sparse Graphs

To provide intuition: a thermometer works because water is a **mean-field** system, as every molecule interacts equally with all others, so the bulk behavior reduces to that of a single "average" particle. Mathematically, on a complete graph $K_N$, the empirical measure $\mu_t^N$ concentrates around a **deterministic** limit $\mu_t \in \mathcal{P}(\mathbb{R}^d)$ satisfying a McKean-Vlasov PDE, and long-time behavior can be studied via classical PDE and functional-analytic tools such as log-Sobolev inequalities and entropy methods.

Real-world networks, however, are rarely complete. Social networks, neural circuits, and communication graphs are **sparse**: each particle $v$ interacts only with its neighbors $\partial_{G_N}(v)$, so the relevant quantity is the local empirical measure
$$\mu_v^{G_N,\xi}(t) := \frac{1}{|\partial_{G_N}(v)|} \sum_{u \in \partial_{G_N}(v)} \delta_{X_u^{G_N,\xi}(t)}.$$
When $G_N$ is sparse (e.g., Erdős–Rényi with $|\partial_{G_N}(v)| = \mathcal{O}(1)$), this measure **does not concentrate** even as $N \to \infty$, as it remains a random object. The limiting description is no longer a deterministic McKean-Vlasov PDE but rather an infinite-dimensional, tree-indexed stochastic process on the local weak limit of $G_N$, and classical tools no longer apply directly. This raises two natural questions my work investigates:

1. **Long-time asymptotics:** Does the system converge to a stationary distribution as $t \to \infty$? On complete graphs, log-Sobolev inequalities yield quantitative rates. On sparse graphs, stationarity is not even well-posed at the level of individual particles, and one must ask whether the tree-indexed process admits an invariant measure, and at what rate it is approached.

2. **Large deviations:** What is the cost of rare fluctuations of $\mu_v^{G_N,\xi}(t)$ away from its typical behavior? On complete graphs, the rate function is a relative entropy functional. On sparse graphs, the combinatorial geometry of $G_N$ contributes nontrivially to the rate function, and deriving a sharp LDP requires understanding the interplay between graph structure and particle dynamics.

These two questions are deeply intertwined: large deviation principles govern the exponential rate at which the system escapes metastable states, clarifying which equilibria are truly stable. Beyond their mathematical interest, precise large deviation estimates are essential for modeling rare but catastrophic events in physical and engineered systems.