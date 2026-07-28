# N-Body Gravitational Simulator

> A real-time gravitational simulation developed in C for the DE1-SoC platform, featuring interactive object creation, orbital motion, and real-time visualization.

> **Computer Organization Course Project**

> **Note:** The source code is not publicly available due to University of Toronto academic integrity policies.

---

## Overview

![tw](image/2body.png)

This project implements an interactive N-body gravitational simulator running on the DE1-SoC platform.

The simulator models gravitational interactions between multiple celestial bodies in real time while allowing users to create new objects and adjust their initial velocities through keyboard input. The simulation continuously updates object trajectories and visualizes the results using VGA graphics.

I was responsible for developing the physics engine and simulation logic, while my teammate implemented the VGA rendering system.

---


## Implementation Highlights

- Softened Gravity

  A softening factor is introduced into the gravitational model to prevent extremely large accelerations when bodies are very close together, improving the numerical stability of the simulation.

- Physics Sub-Stepping

  The physics engine performs four simulation sub-steps per rendered frame, producing smoother trajectories and reducing numerical errors compared with a single update per frame.

- Real-Time Interaction

  The simulator supports creating and deleting celestial bodies during runtime. Users can adjust the initial position, velocity, and mass of new bodies through keyboard input without restarting the simulation.

- Double Buffering

  The VGA display uses double buffering to eliminate screen tearing and provide smooth real-time animation.

- Circular Trace Buffer

  Each body maintains a fixed-size circular buffer that stores recent trajectory points, enabling continuous orbit visualization while keeping memory usage bounded.

- Memory-Mapped I/O

  The simulator communicates directly with the DE1-SoC hardware through memory-mapped I/O for keyboard input, VGA rendering, and frame buffer control.

---

## System Architecture

![arch](image/arch.png)

---
## Algorithm

### Gravitational Model

The simulation models the gravitational interaction between every pair of active bodies. To improve numerical stability when two bodies become very close, a softening factor is added to the squared distance before computing gravitational acceleration.

$$
\mathbf{a}_i = G \sum_{j \neq i} \frac{\mathbf{r}_j - \mathbf{r}_i}{\left(\|\mathbf{r}_j - \mathbf{r}_i\|^2 + \varepsilon^2\right)^{3/2}}
$$

where:

* $G$ is the gravitational constant.
* $\mathbf{r}_i$ is the position of body $i$.
* $\varepsilon$ is the softening factor used to improve numerical stability.

### Numerical Integration

Body positions and velocities are updated using the **Semi-Implicit (Symplectic) Euler** integration method.

$$
\mathbf{v}_{n+1} = \mathbf{v}_n + \mathbf{a}_n \Delta t
$$

$$
\mathbf{x}_{n+1} = \mathbf{x}_n + \mathbf{v}_{n+1} \Delta t
$$

Compared with the standard Explicit Euler method, the Semi-Implicit Euler scheme provides improved long-term stability while remaining computationally efficient for real-time simulation.

---

## My Contributions

### Physics Engine

- Implemented Newtonian gravitational force calculations
- Updated object positions and velocities in real time
- Designed the simulation update loop

### Simulation Logic

- Implemented collision detection and response
- Added orbital initialization logic
- Implemented interactive object creation with adjustable velocity

### Numerical Stability

- Applied softened gravity to avoid singularities
- Used fixed time-step simulation updates
- Implemented trajectory history buffers for continuous visualization

--


## Demonstration

2-body<br>
<img src="image/2body.png" width="400" alt="two">


3-body<br>
<img src="image/3body.png" width="400" alt="three">


4-body<br>
<img src="image/4body.png" width="400" alt="four">

---

## Technical Challenges

### Maintaining Stable Simulation

Simulating gravitational systems requires careful handling of numerical instability when objects become very close to each other. To improve stability, the simulator applies softened gravity and updates the system using a fixed simulation time step.

### Interactive Simulation

The simulator supports creating new celestial bodies during runtime while maintaining continuous simulation without restarting the program.

### Real-Time Performance

The physics engine continuously updates object motion while coordinating with VGA rendering to provide smooth visualization.



---

## Code Availability

The source code for this project is **not publicly available** due to University of Toronto academic integrity policies.

This repository serves as an engineering portfolio documenting the project architecture, implementation approach, and key design decisions.
