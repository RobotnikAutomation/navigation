> ![IMPORTANT] 
> Only Robotnik custom modifications of move_base are discussed below. For anything else, check the [original documentation](http://wiki.ros.org/move_base).

# Robotnik Custom Modification
move_base original implementation does NOT effectively implement **controller_patience**. Even though is a defined parameter that can be set on the node configuration, in the actual code, the parameter is not used for its intended purpose, which is giving the local planner (also known as controller) some courtesy time to avoid an obstacle by itself, without the need of calling the global planner.

To solve this problem, two extra parameters were included: **controller_obstacle_wait** and **controller_success_hysteresis**.
- **controller_obstacle_wait** [s]: Courtesy time given to the controller to avoid an obstacle by itself.
- **controller_sucess_hysteresis** [s]: How long the controller must be returning sucessfull trajectories to consider that obstacle has been avoided. This avoids the robot getting stuck when the controller output is unstable.

**controller_patience** must be bigger or equal than **controller_obstacle_wait**. If not, the global planner would be invoked after **controller_patience** instead of **controller_obstacle_wait**. This is forced when creating or reconfiguring the move_base node.

The original behavoir of move_base has been kept. It can be configured by setting set **controller_obstacle_wait** to **0.0**.


