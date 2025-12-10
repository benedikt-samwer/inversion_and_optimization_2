# Inversion and Optimisation Assessment

**Imperial College London**

**MSc Applied Computational Science and Engineering (ACSE)**

**Module:** Inversion and Optimisation (2024/25)


---

## Overview

This repository contains the solutions for the second coursework assessment of the **Inversion and Optimisation** module. The assessment is divided into two parts, focusing on **Linear Inversion** (steady-state advection-diffusion) and **Non-Linear Inversion** (coupled nonlinear ODEs). The work demonstrates the implementation of numerical methods for solving partial differential equations (PDEs), constructing adjoint models for efficient gradient computation, and utilizing optimization algorithms to solve inverse problems.

## Repository Structure

- `Assessment2_pt1.ipynb`: Part 1 - Linear Inversion (Advection-Diffusion)
- `Assessment2_pt2.ipynb`: Part 2 - Non-Linear Inversion (Adjoint Method & Optimization)
- `Instructions.md`: Original coursework instructions.
- `references.md`: References used during the coursework.

---

## Part 1: Linear Inversion (Advection-Diffusion)

**Goal:** Solve the steady-state advection-diffusion equation using the Finite Difference Method (FDM) and analyze linear solvers.

### Key Implementations:
1.  **Discretization**:
    -   Implemented a linear system $\underline{\mathbf{A}} \mathbf{c} = \mathbf{b}$ representing the PDE.
    -   Used a **5-point stencil** for the diffusion term and an **upwind scheme** for the advection term to handle stability.
    -   Applied Dirichlet boundary conditions.

2.  **Linear Solvers**:
    -   **Direct Solver**: Utilized `scipy.sparse.linalg.spsolve` to obtain the reference solution for the sparse linear system.
    -   **Iterative Solver Analysis**: Evaluated the suitability of Krylov subspace methods.
        -   **Conclusion**: Identified **GMRES (Generalized Minimal Residual method)** with restarts as the optimal choice.
        -   **Reasoning**: The advection term introduces asymmetry to the system matrix $\underline{\mathbf{A}}$, making symmetric solvers like Conjugate Gradient (CG) unsuitable. GMRES effectively handles non-symmetric, indefinite matrices.

---

## Part 2: Non-Linear Inversion (Adjoint Method)

**Goal:** Solve an inverse problem for a system of coupled nonlinear ODEs (resembling predator-prey dynamics) to estimate initial conditions $\mathbf{u}^{(0)}$ that result in a specific final observed state $\hat{\mathbf{u}}$.

### Key Implementations:
1.  **Forward Model**:
    -   Implemented a time-stepping scheme to solve the system of coupled nonlinear ODEs over a defined time interval.

2.  **Adjoint Method**:
    -   Derived and implemented the **continuous adjoint equations** to efficiently compute the gradient of the objective function $\nabla \hat{f}(\mathbf{u}^{(0)})$.
    -   The adjoint method allows for gradient computation with a computational cost independent of the number of design variables, making it superior to finite difference methods for large-scale inversion.

3.  **Verification (Taylor Test)**:
    -   Implemented a **Taylor Test** to rigorously verify the correctness of the computed gradients.
    -   Confirmed **second-order convergence** of the remainder term, validating the adjoint implementation.

4.  **Optimization**:
    -   Solved the minimization problem: $\min_{\mathbf{u}^{(0)}} \frac{1}{2} \|\mathbf{u}^{(n)} - \hat{\mathbf{u}}\|^2$.
    -   Utilized `scipy.optimize.minimize` with advanced algorithms:
        -   **BFGS (Broyden–Fletcher–Goldfarb–Shanno)**: A Quasi-Newton method used for its superlinear convergence and ability to approximate the Hessian.
        -   **Newton-CG**: Used for its stability and ability to handle non-symmetric positive definite Jacobians.
    -   **Results**: Successfully recovered the initial conditions. Compared the performance of Adjoint-based gradients vs. Finite Difference gradients, demonstrating the efficiency and scalability of the adjoint approach.

---

## Technologies & Libraries

-   **Python 3**
-   **NumPy**: For high-performance vector and matrix operations.
-   **SciPy**:
    -   `scipy.sparse`: For handling sparse matrices in Part 1.
    -   `scipy.sparse.linalg`: For solving linear systems (`spsolve`).
    -   `scipy.optimize`: For the minimization algorithms (`minimize`, `BFGS`, `Newton-CG`).
-   **Matplotlib**: For visualizing the concentration fields, convergence plots, and solution trajectories.

## Usage

To run the notebooks:

1.  Ensure you have a Python environment with the required dependencies installed:
    ```bash
    pip install numpy scipy matplotlib
    ```
2.  Open the notebooks using Jupyter Lab or Jupyter Notebook:
    ```bash
    jupyter lab Assessment2_pt1.ipynb
    jupyter lab Assessment2_pt2.ipynb
    ```
3.  Run all cells to execute the code and generate the results.

