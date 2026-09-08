# ros2-learning

Working repo for learning ROS 2 Jazzy from scratch.

## Why this exists

I am a mechanical engineer with an embedded C++ and control background and no prior ROS
experience. This repo is the fundamentals stage: nodes, topics, packages, the build
system and the tooling around them. It is also a way to learn Git version control through application.

Learning is completed through:
1. Following the official ROS 2 Documentation: Jazzy
2. Watching and reading the tutorial series from https://articulatedrobotics.xyz/
3. Being questioned and tested by Claude Code (Opus 5).

It is preparation for the actual project, a ROS 2 interface to an inverted pendulum
simulated in IsaacSim, where the controller runs as a separate node that does not know
whether it is talking to sim or to hardware. That work lives in its own future repo. This one is
kept deliberately as the learning trail.

## Requirements

- Ubuntu 24.04 (chosen for ROS 2 compatibility)
- ROS 2 Jazzy (`/opt/ros/jazzy`)

## Build and run

```bash
git clone https://github.com/termvato/ros2-learning.git
cd ros2-learning

# install declared dependencies from each package.xml
rosdep install --from-paths src -y --ignore-src

# --symlink-install so edits to Python source take effect without a rebuild
colcon build --symlink-install
source install/setup.bash
```

Then in two terminals, each with `source install/setup.bash` run first:

```bash
ros2 run my_first_pkg talker
```

```bash
ros2 run my_first_pkg listener
```

The listener prints each message the talker publishes on `/joint_states`. To watch the
traffic without the listener node:

```bash
ros2 topic echo /joint_states
```

`ros2 topic hz /joint_states` reports the actual publish rate, and
`ros2 topic info /joint_states --verbose` shows the QoS of every publisher and subscriber
on it, which is the first thing to check when a topic exists but no data arrives.

## Packages

- `my_first_pkg` - minimal publisher and subscriber, following the official Jazzy
  tutorials. Now publishes `sensor_msgs/JointState` with a stamped header rather than a
  plain string, as a rehearsal for the pendulum interface.
- `py_srvcli` - minimal service and client, following the official Jazzy tutorial.
  Run the service with `ros2 run py_srvcli service`, then call it with
  `ros2 run py_srvcli client 2 3`.

## Notes

`ROS_DOMAIN_ID` must match across every machine on the network that should see each
other. It is set in `~/.bashrc`, which is not tracked here.
