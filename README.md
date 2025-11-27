# Project Specification: The Kármán Engine
**Language Target:** Rust or C/C++
**Objective:** High-fidelity 6-DOF Aerospace Physics Simulation

## Module I: The Simulation Core (Time & Space)
This module handles the fundamental flow of time and the mathematical universe.

- [ ] **Configurable Time Step (`dt`)**
    - System must support a fixed time-step loop separate from the rendering frame rate.
    - System must maintain physics stability regardless of CPU speed.

- [ ] **Numerical Integration Solver**
    - Implement a solver capable of higher-order accuracy than standard Euler integration.
    - The solver must accept the current state (position/velocity) and derivatives to predict the next state.
    - *Constraint:* Error accumulation must be low enough to maintain a stable planetary orbit for at least 10 simulation revolutions.

- [ ] **Coordinate System Management**
    - System must define a "World Frame" (inertial) and a "Body Frame" (local to the vehicle).
    - Implement functions to transform vectors between World and Body frames.

## Module II: The Rigid Body (Dynamics)
This module defines the object being simulated.

- [ ] **Variable Mass Properties**
    - The object must be able to dynamically reduce its total mass over time (simulating fuel burn).
    - The Center of Mass (CoM) must be capable of shifting position as mass changes.

- [ ] **6-Degrees of Freedom (6-DOF) State Tracking**
    - Track 3 linear variables: Position $(x, y, z)$ and Velocity $(v_x, v_y, v_z)$.
    - Track 3 rotational variables: Angular Velocity $(\omega_x, \omega_y, \omega_z)$ and Orientation.

- [ ] **Quaternion Rotation System**
    - Implement orientation tracking using Quaternions (w, x, y, z) to avoid Gimbal Lock.
    - Implement logic to normalize quaternions at every step to prevent drift.

- [ ] **Inertia Tensor Implementation**
    - Define the Rotational Inertia Matrix (Tensor) for the vehicle.
    - The physics loop must account for resistance to rotational acceleration based on this tensor.

## Module III: The Environment (Planetary Physics)
This module defines the world the object interacts with.

- [ ] **Inverse-Square Gravity Model**
    - Gravity must be calculated as a vector pointing toward the planet's center.
    - The magnitude of gravity must decrease relative to the distance from the planet's center (not constant 9.81).

- [ ] **Atmospheric Model (ISA or Equivalent)**
    - Implement a function that returns Air Density ($\rho$), Pressure, and Temperature based on input Altitude.
    - The model must account for the distinct layers of the atmosphere (Troposphere vs. Stratosphere lapse rates).

## Module IV: Aerodynamics & Propulsion
This module calculates the forces applied to the rigid body.

- [ ] **Thrust Vectoring**
    - Implement a propulsion source that applies force relative to the Body Frame (e.g., out the back of the rocket), not the World Frame.
    - Allow for "Gimbaling" (offsetting the thrust angle) to generate torque.

- [ ] **Drag Equation Implementation**
    - Calculate drag force opposite to the velocity vector.
    - System must update the Drag Coefficient ($C_d$) dynamically based on Mach number (Subsonic vs. Transonic vs. Supersonic).

- [ ] **Lift & Angle of Attack (AoA)**
    - Calculate the Angle of Attack ($\alpha$) derived from the difference between the Heading Vector and the Velocity Vector.
    - Implement a lift lookup function that varies lift force based on current AoA.
    - Implement "Stall" behavior where lift decreases drastically past a critical AoA.

## Module V: Telemetry & Output
- [ ] **Data Logging**
    - Output a CSV or standard stream containing: Timestamp, Altitude, Velocity Magnitude, G-Force, and Current Mass.

- [ ] **Apogee Detection**
    - Logic to detect and flag the highest point of the flight path.

---

## Recommended "Search Keys"
*Use these terms to find the math required to implement the features above.*

1.  **Runge-Kutta 4 (RK4)** (For Module I)
2.  **Quaternions vs Euler Angles** (For Module II)
3.  **Moment of Inertia Tensor** (For Module II)
4.  **International Standard Atmosphere (ISA) formulas** (For Module III)
5.  **Newton's Law of Universal Gravitation Vector Form** (For Module III)
6.  **Aerodynamic Center vs Center of Pressure** (For Module IV)
7.  **Prandtl-Glauert singularity** (For Module IV - Mach effects)
