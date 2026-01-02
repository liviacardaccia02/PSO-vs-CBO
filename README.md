# Comparative Analysis: Particle Swarm Optimization (PSO) vs Consensus-Based Optimization (CBO)

This repository contains the implementation, analysis, and performance comparison of two metaheuristic optimization algorithms: **Particle Swarm Optimization (PSO)** and **Consensus-Based Optimization (CBO)**.

The project focuses on optimizing non-convex objective functions in both low-dimensional (2D) and high-dimensional (10D) spaces, analyzing convergence dynamics, numerical stability, and parameter sensitivity.

## Repository Structure

The codebase is organized into Jupyter Notebooks to facilitate experiment reproducibility and result visualization.

- **`pso.ipynb`**: Contains the full implementation of the PSO algorithm. It includes logic for velocity and position updates, boundary handling (clamping), and strategies like linear inertia decay to balance exploration and exploitation.
- **`cbo.ipynb`**: Contains the implementation of the CBO algorithm. This notebook features advanced numerical techniques to ensure algorithmic stability (Shift Trick, weight normalization) and manages stochastic components (Drift and Diffusion).
- **`PSOvsCBO.pdf`**: A detailed academic report documenting theoretical foundations, experimental methodology, and a critical analysis of the obtained results.
- **`cbo_results.csv` / `pso_results.csv`**: Files containing raw data from experimental runs (success metrics, final error, execution time) for statistical analysis.

## Implementation Details

### Particle Swarm Optimization (PSO)
The implementation follows the standard "Global Best" topology. Key features include:
- **Adaptive Inertia**: Implementation of a linear decay for the inertia weight ($w$) to favor exploration in early phases and fine-tuning in later stages.
- **Velocity Clamping**: Limitation of the velocity vector to prevent particles from exploding out of the search space, which is critical for functions like Schwefel.

### Consensus-Based Optimization (CBO)
The CBO implementation is based on opinion dynamics and stochastic diffusion processes. To address the intrinsic numerical challenges of the algorithm, the following solutions were adopted:
- **Shift Trick & Normalization**: To prevent arithmetic underflow when calculating exponential weights ($e^{-\alpha C(x)}$), Min-Max cost normalization and current minimum subtraction were introduced.
- **Drift/Diffusion Balance**: Fine-tuning of $\lambda$ (Drift) and $\sigma$ (Diffusion) parameters to avoid premature swarm collapse (freezing) or divergence.

## Experimental Methodology

The algorithms were tested on a set of five standard benchmark functions:
1.  **Sphere** (Unimodal, Convex)
2.  **Rosenbrock** (Unimodal, narrow and curved valley)
3.  **Ackley** (Multimodal)
4.  **Schwefel** (Multimodal, minima at bounds)
5.  **Rastrigin** (Highly Multimodal)

Each experiment was conducted using a **Fixed-Cost** methodology, with a set budget of iterations and particles, measuring the success rate (tolerance $10^{-4}$) over 30 independent runs.

## Results

The comparative analysis highlighted distinct behaviors:

* **2D Scenario**: PSO demonstrated excellent robustness, converging in 100% of cases across all functions. CBO achieved good results on spherical and regular multimodal functions but consistently failed on the Rosenbrock function due to the consensus mechanism's difficulty in navigating flat, curved valleys.
* **10D Scenario**: Increased dimensionality negatively impacted both algorithms. PSO maintained convergence on simple functions (Sphere, Ackley) but showed limits on complex landscapes (Rastrigin, Schwefel). CBO suffered significantly from the "curse of dimensionality," requiring extremely specific parameter tuning and showing difficulties in reaching the required tolerance despite numerical corrections.

## Future Developments

To overcome current limitations, especially in high dimensions, future work will focus on:

1.  **Automatic Parameter Optimization**: Developing a meta-algorithm or hyperparameter tuning procedure (e.g., Grid Search or Bayesian Optimization) to automatically identify the optimal configuration ($\lambda, \sigma, \alpha$ for CBO; $w, c_1, c_2$ for PSO) specific to each objective function, removing the need for manual tuning to achieve convergence.
2.  **Adaptive Variants**: Implementing adaptive mechanisms that dynamically modify noise variance ($\sigma$) in CBO in response to population diversity to prevent stagnation in local minima.
