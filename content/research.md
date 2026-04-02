+++
draft = false
title = 'Research'
+++

My research interests are broadly in applied probability theory — in particular, how the
*microscopic* random behavior of an **interacting particle system** (IPS) can be organized
to recover specific *macroscopic* behaviors of the ensemble. Consider a collection of
diffusive particles $\{X_v^{G,\xi}\}_{v \in V}$, indexed by the nodes of a graph $G = (V, E)$,
with square-integrable initial conditions $\{\xi_v\}_{v \in V}$, evolving according to the
coupled system of SDEs
$$\mathrm{d}X_v^{G,\xi}(t) = b\left(t, X_v^{G,\xi}(t), \mu_v^{G,\xi}(t)\right)\mathrm{d}t + \sigma\left(t, X_v^{G,\xi}(t), \mu_v^{G,\xi}(t)\right)\mathrm{d}W_v(t), \quad X_v^{G,\xi}(0) = \xi_v,$$
where $\{W_v\}_{v \in V}$ are i.i.d. standard Brownian motions independent of $\{\xi_v\}_{v \in V}$,
with sufficiently regular coefficient functions $b, \sigma : \mathbb{R}_+ \times \mathbb{R}^d \times \mathcal{P}(\mathbb{R}^d) \to \mathbb{R}^d$,
and $\mu_v^{G,\xi}(t)$ is the **local empirical measure** of $v$ defined as
$$\mu_v^{G,\xi}(t) := \frac{1}{|\partial_G(v)|} \sum_{u \in \partial_G(v)} \delta_{X_u^{G,\xi}(t)}$$
where the neighborhood is denoted by $\partial_G(v)$.

The choice of graph $G$ distinguishes my two research programs. 
- When $G = K_N$ is the **complete graph**, every particle interacts with the full ensemble, and the system can be leveraged as a powerful computational tool for optimization and sampling. 
- When $G \sim \mathcal{G}(N, c/N)$ with $c > 0$ fixed is a **sparse graph**, each particle interacts with only $O(1)$ neighbors, and the large-particle behavior becomes a fundamentally harder analytical problem.

---

## IPS for Optimization and Sampling

Optimization and sampling are quintessential tasks in applied mathematics, both of
which arise naturally from solving inverse problems. In applications, $\mathcal{G}$
is **expensive** to evaluate (e.g., solving a PDE), and derivatives of $\mathcal{G}$
are **unavailable** — ruling out classical gradient descent and Langevin sampling.

These tasks arise from formulating the Bayesian inverse problem, where one wishes to
recover a parameter $\theta \in \mathbb{R}^d$ from data $y \in \mathbb{R}^m$, with
$\mathcal{G} : \mathbb{R}^d \to \mathbb{R}^m$ and $\eta \sim \mathcal{N}(0,\Gamma)$:

$$y = \mathcal{G}(\theta) + \eta.$$

For a Gaussian prior $\mathbb{P}(\theta)=\mathcal{N}(\theta_0,\Sigma)$, the posterior is

$$\mathbb{P}(\theta \mid y) \propto \exp\!\left(-\tfrac{1}{2}\lVert y-\mathcal{G}(\theta)\rVert_{\Gamma}^{2} - \tfrac{1}{2}\lVert \theta-\theta_0\rVert_{\Sigma}^{2}\right) =: \exp\big(-\Phi(\theta)\big),$$

where $\lVert x \rVert_{A}^{2} := \langle x, A^{-1}x\rangle$. Two natural questions arise:

1. **Optimization:** Find the MAP estimate (the maximizer of $\mathbb{P}(\theta \mid y)$).
2. **Uncertainty Quantification:** Sample from the posterior $\mathbb{P}(\theta \mid y)$.

Particle-based methods have emerged as a natural answer to both, as they avoid gradient
evaluations, are embarrassingly parallelizable, and enjoy convergence guarantees via
mean-field analysis. A canonical example is **ensemble Kalman sampling** (EKS), where
one runs a system of $N$ particles $\{\theta_t^i\}_{i=1}^N$ with dynamics

$$\mathrm{d}\theta_t^i = -C(\mu_t^N) \nabla \Phi(\theta_t^i) \, \mathrm{d}t + \sqrt{2 C(\mu_t^N)} \, \mathrm{d}W_t^i,$$

where $C(\mu_t^N) := \frac{1}{N}\sum_{i=1}^N (\theta_t^i - \bar{\theta}_t^N)(\theta_t^i - \bar{\theta}_t^N)^\top$
is the **empirical covariance** and $\bar{\theta}_t^N := \frac{1}{N}\sum_{i=1}^N \theta_t^i$
is the ensemble mean. The covariance preconditioning is what allows EKS to avoid
explicit gradient evaluations of $\mathcal{G}$, making it especially suitable for
the derivative-free setting.

My work in this direction focuses on the **algorithmic design and convergence analysis**
of such methods, with a current focus on particle swarm optimization (PSO).

---

## IPS on Sparse Graphs

To build intuition: a thermometer works because water is a **mean-field** system —
every molecule interacts equally with all others, so the bulk behavior reduces to that
of a single "average" particle. Mathematically, on a **complete graph** $K_N$, each
particle $i$ interacts with all others via the empirical measure, and one can show that
as $N \to \infty$, the empirical measure $\mu_t^N$ concentrates around a
deterministic limit $\mu_t \in \mathcal{P}(\mathbb{R}^d)$ satisfying the
**McKean-Vlasov PDE**:

$$\partial_t \mu_t = -\nabla \cdot (b(\cdot, \mu_t) \mu_t) + \tfrac{1}{2} \nabla^2 : (\sigma\sigma^\top(\cdot, \mu_t) \mu_t).$$

In particular, $\mu_t$ is **deterministic**: all randomness washes out in the $N \to \infty$ limit,
and the long-time behavior $\mu_t \to \mu_\infty$ as $t \to \infty$ can be studied via
classical PDE and functional-analytic tools (e.g., log-Sobolev inequalities, entropy methods).

Real-world networks, however, are rarely complete. Social networks, neural circuits,
and communication graphs are **sparse**: each particle $i$ interacts only with its
neighbors $\mathcal{N}(i)$ in a graph $G_N$, so the relevant empirical measure for
particle $i$ is the **local empirical measure**

$$\mu_t^{i,N} := \frac{1}{|\mathcal{N}(i)|} \sum_{j \in \mathcal{N}(i)} \delta_{X_t^{j,N}},$$

rather than the global $\mu_t^N$. When $G_N$ is sparse (e.g., an Erdős–Rényi graph
$\mathcal{G}(N, c/N)$ with $c > 0$ fixed), the degree $|\mathcal{N}(i)| = O(1)$, and
$\mu_t^{i,N}$ **does not concentrate** — it remains a random object even as $N \to \infty$.

This is the fundamental difficulty of the sparse setting: **the mean-field limit is no
longer a deterministic PDE, but a random process.** More precisely, the local empirical
measures $\{\mu_t^{i,N}\}$ converge (in an appropriate sense) to the law of a
**McKean-Vlasov process on the Poisson-weighted infinite random tree** (PWIT), the
local weak limit of $\mathcal{G}(N, c/N)$. The limiting object is an
infinite-dimensional, tree-indexed stochastic process, and classical PDE tools no
longer apply directly.

This raises several natural but difficult questions that my work investigates:

1. **Long-time asymptotics:** Does the system converge to a stationary distribution as
$t \to \infty$? On complete graphs, entropy methods and log-Sobolev inequalities give
quantitative rates. On sparse graphs, stationarity is not even well-posed at the level
of individual particles — one must instead ask whether the **tree-indexed process**
admits an invariant measure, and if so, at what rate it is approached.

2. **Large deviations:** How does $\mu_t^{i,N}$ fluctuate around its typical behavior,
and what is the cost of rare events? On complete graphs, the large deviation rate
function is given by a **relative entropy** functional. On sparse graphs, the
combinatorial structure of the graph itself contributes to the rate function, and
deriving a sharp LDP requires understanding the interplay between the graph geometry
and the particle dynamics.

These two questions are deeply intertwined — large deviation principles often govern
the exponential rate at which the system escapes metastable states, shedding light on
which equilibria are truly stable in the long run.