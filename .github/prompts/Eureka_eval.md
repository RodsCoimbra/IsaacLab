We trained a RL policy using the provided reward function code and tracked the values of the individual components in the reward function as well as global policy metrics such as success rates and episode lengths after every 10 epochs and the maximum, mean, minimum values. We also compared it to the oracle reward function, defined in the rewards.py as a comment, which serves as a baseline. The results were the following:

{Data}


Please carefully analyze the policy feedback and provide a new, improved reward function that can better solve the task. Some helpful tips for analyzing the policy feedback:
    (1) If the success rates are always near zero, then you must rewrite the entire reward function
    (2) If the values for a certain reward component are near identical throughout, then this means RL is not able to optimize this component as it is written. You may consider
        (a) Changing its scale or the value of its temperature parameter
        (b) Re-writing the reward component
        (c) Discarding the reward component
    (3) If some reward components' magnitude is significantly larger, then you must re-scale its value to a proper range
Please analyze each existing reward component in the suggested manner above first, and then write the reward function code.
Your reward function should use useful variables from the environment as inputs.
It must comply to the following signature exactly:

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

You can modify the following files:
rewards.py - modify the rewards structure.
tiago_env.py - Function _get_rewards() - Modify the rewards received.