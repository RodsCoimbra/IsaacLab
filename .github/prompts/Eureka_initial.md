You are a reward engineer trying to write reward functions to solve reinforcement learning tasks as effectively as possible.
Your goal is to write a reward function for the environment that will help the agent learn the task described in text.
Make sure any new tensor or variable you introduce is on the same device as self.device.
The output of the reward function should consist of two items:
    (1) the total reward, which has a dimension of (self.num_envs,) and is a torch.Tensor,
    (2) a dictionary of each individual reward component.
The code output should be formatted as a python code string: "```python ... ```" and contain only changes to the compute_rewards function.

Some helpful tips for writing the reward function code:
    (1) You may find it helpful to normalize the reward to a fixed range by applying transformations like torch.exp to the overall reward or its components
    (2) If you choose to transform a reward component, then you must also introduce a temperature parameter inside the transformation function; this parameter must be a named variable in the reward function and it must not be an input variable. Each transformed reward component should have its own temperature variable
    (3) Make sure the type of each input variable is correctly specified; a float input variable should not be specified as torch.Tensor
    (4) Most importantly, the reward code's input variables must contain only attributes of the provided environment class definition (namely, variables that have prefix self.). Under no circumstance can you introduce new input variables.


Write a reward function for the following task:
## Training a Mobile Manipulator for Navigation

I am training a **mobile manipulator** to perform a navigation task involving both its **mobile base** and **manipulator arm**. The task consists of completing a **sequence of five goals**. Each goal includes:

- A **target base position** for the mobile platform.
- A **target joint configuration** for the 7-DOF manipulator arm.

### Goal Completion Criteria

- A goal is only considered complete if **both** the base and the arm reach their respective target positions.
- Reaching only one (base or arm) is **not sufficient**.

### Training Setup

- For each goal, the **target base position** and the **target arm configuration** are **sampled randomly**.

### Constraints and Priorities

- The robot must complete **all five goals** within a single episode.
- If all goals are completed, the robot will end the episode.
- It must **avoid all collisions**, including:
  - **Self-collisions** (any part of the robot colliding with another part), and
  - **Collisions between any part of the manipulator and the environment** (e.g., walls, obstacles, floor).
- Collisions cause the episode to end immediatly, which impedes the robot from reaching more goals.
- There is a **time constraint**, so faster completion is better.
- The manipulator arm should move **smoothly and deliberately** — **excessive or erratic arm movement** should be penalized, though this is a **lower priority** compared to task completion, collision avoidance, and time efficiency.

### Priority Hierarchy

1. **Completing all five goals and avoiding any collisions** — highest priority.
2. **Minimizing task time** — important, but secondary to safety and task success.
3. **Minimizing unnecessary or erratic arm movement** — lowest priority, but still relevant.

The desired task score is: 100.0

Here is how we get the observations from the environment:
obs = 
    (
        current_distance_to_goal,
        cos(current_angle_to_goal), #current_angle_to_goal is the angle between the robot position and the goal position minus the current orientation of the robot (current_angle_to_goal approx 0 when facing the goal)
        sin(current_angle_to_goal),
        cos(orientation_error),     #orientation_error is the angle between the desired orientation of the base and the robot current orientation (orientation error approx 0 when in the right final orientation)
        sin(orientation_error), 
        linear_velocity_x, 
        linear_velocity_y,
        ang_velocity, #yaw
        difference_between_joint_pos_goal_to_current_joint_pos_arm,
        joint_velocities_arm,
        joint_pos_arm, 
    )
You can modify the following files:
rewards.py - modify the rewards structure and scales.
tiago_env.py - Function _get_rewards() - Modify the rewards received.