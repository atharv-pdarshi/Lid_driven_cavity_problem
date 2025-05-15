# Simulating Fluid Flow in a Lid-Driven Cavity

- By Apoorv Bhardwaj - 23117026
-    Atharv Priyadarshi - 23117034

This project explores the classic lid-driven cavity problem, a standard test case in computational fluid dynamics (CFD). I've developed a Python code to simulate this 2D flow, using a finite difference approach to solve the governing Navier-Stokes equations.

## The Problem: A Stirred-Up Box of Fluid

Imagine a square box filled with fluid. The top lid of the box slides horizontally at a constant speed, while the other walls stay put.  This simple setup creates interesting swirling patterns within the fluid, the complexity of which depends on a key parameter called the Reynolds number (Re).  This simulation investigates how these patterns change as we tweak this number.

## My Approach: Breaking Down the Math

My code uses a finite difference method, essentially breaking down the continuous fluid domain into a grid of discrete points. I've chosen a second-order accurate scheme on a uniform, staggered grid, which balances accuracy and computational cost.  To simplify the math and enforce incompressibility, I'm using the vorticity-streamfunction formulation of the Navier-Stokes equations.  The simulation proceeds step-by-step in time until the flow settles into a steady state.

Here's the breakdown of each time step:

1. **No-Slip Walls:**  I enforce the no-slip condition – the fluid sticks to the walls – on all boundaries except the moving lid.
2. **Vorticity Tracking:**  I solve the vorticity transport equation to figure out how the fluid's rotation changes over time.  I've opted for an explicit finite difference method for this step.
3. **Streamlines:**  Next, I solve the Poisson equation for the streamfunction, which helps us visualize the flow's streamlines – imaginary lines that trace the fluid's path. I use an iterative solver, similar to a successive over-relaxation method, to find the solution here.
4. **Velocity Calculation:**  From the streamfunction, I calculate the fluid's velocity (u, v) at each point on the grid. Central difference approximations are used here.
5. **Checking for Stability:** I monitor how much the vorticity changes at each time step. If it's below a certain threshold (the `tolerance`), I consider the solution converged, and the simulation stops.

## The Equations Behind the Swirls

My code solves these key equations:

| Equation                      | What it means                                                              |
|-------------------------------|--------------------------------------------------------------------------|
| ∂ω/∂t + u∂ω/∂x + v∂ω/∂y = ν∇²ω | How vorticity changes due to fluid motion and viscosity.                |
| ∇²ψ = -ω                       | How vorticity relates to the streamfunction (which defines streamlines). |
| u = ∂ψ/∂y                      | How the x-velocity relates to the streamfunction.                      |
| v = -∂ψ/∂x                     | How the y-velocity relates to the streamfunction.                      |

Where:

* ω (omega) is vorticity (fluid rotation)
* ψ (psi) is the streamfunction
* u and v are the x and y components of velocity
* ν (nu) is kinematic viscosity (how resistant the fluid is to flow)
* t is time
* x and y are the spatial coordinates


## Tweaking the Simulation: Key Parameters

You can customize the simulation using these parameters:

| Parameter       | What it does                                                                                                  | Default Value |
|-----------------|--------------------------------------------------------------------------------------------------------------|---------------|
| `n_x`           | Controls the grid resolution (number of points in both x and y directions). Higher = finer grid, more accurate but slower. | 64            |
| `re`            | The Reynolds number: higher Re means less viscous, more turbulent flow.                                     | 100           |
| `l`             | The size of the square cavity.                                                                               | 1.0           |
| `u_lid`        | How fast the top lid is moving.                                                                             | 1.0           |
| `dt`            | The size of each time step. Smaller steps can improve stability but take longer.                                   | 0.001         |
| `max_iter`     | A safety net: the maximum number of time steps before the simulation stops, even if it hasn't converged.                 | 10000         |
| `tolerance`    | How small the change in vorticity needs to be before we consider the solution converged.                         | 1e-7          |
| `num_particles` | Number of virtual particles used to visualize the flow paths (particle tracking).                      | 100           |
| `mu`            | Dynamic viscosity (stickiness) of the fluid.  Related to kinematic viscosity by  `mu = rho * nu`, where rho (density) is assumed to be 1. | Calculated    |
| `nu`            | Kinematic viscosity of the fluid.                                                                     | Calculated    |

## How to Run the Code

1.  Make sure you have  `numpy`  and  `matplotlib`  installed:  `pip install numpy matplotlib`
2.  Copy the Python code into a Jupyter Notebook (recommended) or a Python script.
3.  Change the parameters in the main part of the code to your liking.
4.  Run the code! The plots will show up in the Jupyter Notebook, or you can save them as images if you're using a script.

## What You'll See

The code generates a bunch of helpful plots:

*   **Vorticity Contours:**  Shows how much the fluid is rotating at each point.
*   **Velocity Vectors:**  Arrows showing the speed and direction of the fluid.
*   **Particle Tracking:**  Traces the paths of imaginary particles dropped into the fluid.
*   **Shear Stress on Top Lid:** How much force the moving lid exerts on the fluid.
*   **U-velocity Contour:** A color map of the horizontal velocity.
*   **Centerline U-velocity:**  A plot of the horizontal velocity along the center of the box.
*   **Divergence Field:**  A check to ensure the fluid isn't compressible (it shouldn't be!).
*   **Streamfunction Contours:**  Lines that show the overall flow patterns.
