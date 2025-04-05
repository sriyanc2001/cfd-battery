# CFD Simulation for Battery Thermal Management

This repository contains a Computational Fluid Dynamics (CFD) simulation for battery thermal management using finite difference methods. The code solves coupled Navier-Stokes equations for fluid flow and energy equations for heat transfer, including battery heat generation effects.

## Features
- 2D incompressible flow simulation
- Staggered grid formulation (MAC scheme)
- Upwind advection and central diffusion schemes
- Pressure-velocity coupling (Projection Method)
- Heat transfer with conduction/advection
- Battery heat generation modeling
- Obstacle handling with masked arrays
- Animated visualization of results

## Numerical Methods

| Equation Type          | Discretized Form                                                                 | Numerical Scheme          |
|------------------------|---------------------------------------------------------------------------------|---------------------------|
| **X-Momentum**         | `∂u/∂t + u∂u/∂x + v∂u/∂y = ν(∂²u/∂x² + ∂²u/∂y²) - (1/ρ)∂p/∂x`                   | Upwind (advection), Central (diffusion) |
| **Y-Momentum**         | `∂v/∂t + u∂v/∂x + v∂v/∂y = ν(∂²v/∂x² + ∂²v/∂y²) - (1/ρ)∂p/∂y`                   | Upwind (advection), Central (diffusion) |
| **Pressure Poisson**   | `∇²p = ρ/Δt(∇·u*)`                                                              | Jacobi Iteration          |
| **Energy Equation**    | `ρc_p(∂T/∂t + u∂T/∂x + v∂T/∂y) = k(∂²T/∂x² + ∂²T/∂y²) + q_gen`                   | Upwind (advection), Central (conduction) |

## Grid Discretization

| Grid Type      | Staggering                        | Dimensions          | Note                                |
|----------------|-----------------------------------|---------------------|-------------------------------------|
| Pressure (P)   | Cell-centered                    | (ny+2) x (nx+2)     | Extended boundary cells            |
| U-Velocity     | Staggered in x-direction         | (ny+2) x (nx+1)     | Located at cell faces              |
| V-Velocity     | Staggered in y-direction         | (ny+1) x (nx+2)     | Located at cell faces              |
| Temperature    | Cell-centered                    | (ny+2) x (nx+2)     | Same as pressure grid              |

## Boundary Conditions

| Boundary Type  | Velocity Treatment               | Temperature Treatment       | Pressure Treatment          |
|----------------|-----------------------------------|------------------------------|------------------------------|
| **Inlet**      | Dirichlet (fixed velocity)       | Dirichlet (fixed temp)       | Neumann (∂p/∂n = 0)         |
| **Outlet**     | Neumann (zero gradient)          | Neumann (zero gradient)      | Dirichlet (p = 0)           |
| **Walls**      | No-slip (u=v=0)                  | Adiabatic (∂T/∂n = 0)        | Neumann (∂p/∂n = 0)         |
| **Obstacles**  | No-slip (u=v=0)                  | Heat generation source       | Solid cells masked          |

## Key Parameters

| Parameter              | Value               | Description                          |
|------------------------|---------------------|--------------------------------------|
| Domain Size (Lx x Ly)  | 0.1 m x 0.1 m       | Simulation domain dimensions        |
| Grid Resolution        | 100 x 100           | nx x ny cells                       |
| Time Step (Δt)         | 0.0001 s            | CFL-condition compliant             |
| Fluid Density          | 970 kg/m³           | Coolant density                     |
| Battery Heat Gen       | 0 W/m³              | Heat generation rate (configurable) |

## Results Visualization
The code generates three animations:
1. `battery_temperature.mp4` - Temperature field evolution
2. `velocity_field.mp4` - Velocity magnitude contours
3. `pressure_field.mp4` - Pressure distribution
