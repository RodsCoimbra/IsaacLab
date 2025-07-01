## Training a Mobile Manipulator for Navigation

I am training a **mobile manipulator** to perform a navigation task involving both its **mobile base** and **manipulator arm**. The task consists of completing a **sequence of five goals**. Each goal includes:

- A **target base position** for the mobile platform.
- A **target joint configuration** for the 7-DOF manipulator arm.

### Goal Completion Criteria

- A goal is only considered complete if **both** the base and the arm reach their respective target positions.
- Reaching only one (base or arm) is **not sufficient**.

### Training Setup

- For each goal, the **target base position** and the **target arm configuration** are **sampled randomly**.
- The robot can access its current observations through the `_get_observations` function defined in the `tiago_env.py` file.

### Constraints and Priorities

- The robot must complete **all five goals** within a single episode.
- It must **avoid all collisions**, including:
  - **Self-collisions** (any part of the robot colliding with another part), and
  - **Collisions between any part of the manipulator and the environment** (e.g., walls, obstacles, floor).
- There is a **time constraint**, so faster completion is better, but priorities must be balanced as follows:

### Priority Hierarchy

1. **Completing all five goals** — highest priority.
2. **Avoiding any collisions** — more important than speed.
3. **Minimizing task time** — important, but secondary to safety and task success.

### Objective

Design a reward function or training strategy that:

- Rewards successful completion of all goals,
- Penalizes any form of collision heavily,
- Encourages faster completion — **but only after** ensuring safety and full task completion.
