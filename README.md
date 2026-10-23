# Comparative Analysis: Particle Swarm Optimization (PSO) vs Consensus-Based Optimization (CBO)

This project implements and compares two stochastic metaheuristic optimizers—**Particle Swarm Optimization (PSO)** and **Consensus-Based Optimization (CBO)**—on five standard objective functions. The notebooks are intended for learning, experimentation, and visualization; results depend on the random initialization and algorithm parameters.

## Repository contents

- `pso.ipynb`: PSO implementation, single-run visualization, and benchmark runner.
- `cbo.ipynb`: CBO implementation, single-run visualization, and benchmark runner.
- `PSOvsCBO.pdf`: accompanying academic report.

The CSV files `pso_results.csv` and `cbo_results.csv` are **generated when the corresponding benchmark runner is executed**; they are not included in the repository by default.

## Algorithms

### Particle Swarm Optimization (PSO)

The implementation uses a global-best topology, a linearly decreasing inertia weight, and velocity clamping. The global best is initialized from the evaluated starting population, and initial evaluations are included in the function-evaluation count.

### Consensus-Based Optimization (CBO)

The implementation updates a population using a weighted consensus point and stochastic drift/diffusion. Min-max normalization of the current population's objective values is used before computing exponential weights.

## Benchmark functions

Both notebooks implement these functions:

| Function | Characteristics |
|---|---|
| Sphere | Unimodal, convex |
| Rosenbrock | Narrow curved valley |
| Ackley | Multimodal |
| Schwefel | Multimodal |
| Rastrigin | Highly multimodal |

Search bounds vary by function. The current notebook defaults are 2 dimensions, 200 particles, 500 maximum iterations, 30 runs per function, a success tolerance of `1e-3`, and a maximum of 50,000 objective-function evaluations per run. The evaluation budget is enforced in population-sized batches. PSO counts its initial population evaluations; CBO evaluates the initial population in its first iteration, so both algorithms report evaluations using the same population-based convention.

A run is marked successful when its best objective value is within the configured tolerance of the known optimum (zero for the implemented benchmark functions). CPU time is measured with `time.process_time()` and will vary by machine and execution environment.

## Running the experiments

1. Install the dependencies used by the notebooks:

   ```bash
   python -m pip install -r requirements.txt
   ```

2. Open `pso.ipynb` or `cbo.ipynb` in Jupyter.
3. Run the notebook cells in order.
4. Uncomment `df_results = run_experiments()` in the final cell to run the benchmark suite and write the results CSV in the current working directory. This executes 30 runs for each of the five functions and can take some time.
5. Alternatively, uncomment `run_single_execution()` for an interactive 2D run and contour visualization.

The benchmark runner currently uses NumPy's global random generator without setting a seed, so repeated runs are not deterministic. Set and document a seed strategy before interpreting small differences as robust evidence.

## Interpreting results and limitations

The accompanying report discusses the observed behavior of the algorithms in 2D and 10D settings. The checked-in notebooks currently default to **2D only**; reproduce 10D results by setting `DIMENSIONS = 10` and rerunning the experiments. Existing report conclusions should be treated as results from the report's experiment configuration, not as results automatically reproduced by the notebooks' default settings.

The benchmark suite is a small educational comparison, not a comprehensive optimizer evaluation. Performance depends on parameter choices, random seeds, search bounds, dimensionality, and the success tolerance. The notebooks do not currently provide automated statistical tests or continuous-integration checks.
