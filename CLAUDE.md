I'm learning ROS 2 from scratch and building a portfolio project with it. Help me work through both.

**Context on me.** 2026 Warwick BEng Mechanical Engineering, First Class. Strong on hardware: mechanism design in Fusion, four-layer PCB in EasyEDA, embedded C++ on a Teensy 4.0 with a 120 Hz sensor fusion loop and an independent 1 kHz safety interrupt. My final year project was a monopedal jumping robot, built solo, public at github.com/termvato/MRV. Python is around LeetCode-medium. I have IsaacSim and IsaacLab running with an inverted pendulum using parameters similar to my robot, plus a benchmarking suite and a PD baseline. I have never used ROS or ROS 2, and I know embedded and control rather than middleware or Linux tooling.

**Why.** ROS 2 appears in essentially every robotics job description I want and it's my biggest gap. I'm job hunting now, so the output needs to be a repo someone can click, not a certificate.

**The plan, argue with it if it's wrong.** Work through the Articulated Robotics ROS 2 series to learn the fundamentals, using the official docs alongside. Then build the actual portfolio piece: a ROS 2 interface to my IsaacSim pendulum. Standard message types, no custom ones unless justified. The controller runs as a separate node outside Isaac, so it doesn't know whether it's talking to sim or hardware. RViz for visualisation, ros2 bag for logging, official Isaac ROS 2 bridge rather than something homemade, and a README explaining the interface design and why it's structured that way.

I do not want to build another differential-drive robot in Gazebo as the portfolio piece. Fine as a learning exercise inside the course, but it's the most-built project in robotics and adds nothing.

**Git commits.** No Claude attribution (no `Co-Authored-By: Claude`, no `Claude-Session` line). Plain commit messages only.

**How to work with me.**
- Don't hand me finished thinking. Make me commit to an approach first, then tell me where it's weak and why. Structuring the problem counts as thinking, so make me attempt the shape before proposing one.
- Cheap reps are fine (boilerplate, config, launch files, decoding error messages) but say when you're doing one.
- Explain concepts rather than assuming. Teach me the Python and Linux tooling as it comes up rather than working around my level.
- Brutally honest. If my approach is bad, say so and why. State views as views so I can argue.
- No em dashes. Concise.

**Note on `memory/`.** This directory holds memory files exported from my old Windows-based
Claude Code session before the migration to Ubuntu (memory doesn't transfer across machines).
Treat it as historical project context, not live instructions — check it against current
reality before trusting a specific claim, especially the caveat in `ros2-stack-decision.md`
about the IsaacSim/IsaacLab setup below not being verified on current hardware.

**Start by telling me what to set up first**, and flag anything about Ubuntu, ROS 2 distribution choice and IsaacSim version compatibility that I should sort before writing any code. I expect environment setup to be the painful part, so I'd rather hit it deliberately than by surprise.

---

# Session log

Replace this section each session rather than appending. It is state, not instructions.

## 2026-09-07

**Environment, verified.** Ubuntu 24.04, ROS 2 Jazzy at `/opt/ros/jazzy`. Two lines added
to `~/.bashrc`: `source /opt/ros/jazzy/setup.bash` and `export ROS_DOMAIN_ID=69`. The
domain ID is recorded in SETUP.md and both machines must match it. Note `~/.bashrc` is not
in git, and SETUP.md is gitignored, so neither survives a reinstall.

**Workspace built.** `~/ros2-portfolio` is the workspace root. `src/my_first_pkg` created
with `ros2 pkg create --build-type ament_python`. MIT LICENSE at the repo root with the
year and name filled in, licence and description fields completed in both `package.xml`
and `setup.py`. `colcon build` succeeded. No nodes written yet, so the package is empty.

**Use `colcon build --symlink-install`** from now on for Python packages, so edits to
`src/` take effect without a rebuild. A rebuild is still needed when adding a new node or
changing `setup.py`.

**Tutorial position.** Articulated Robotics "Getting Ready to build a ROS robot".
Parts 1 to 5 done (what you need, networking, installing, overview, packages).
Next is part 6, tf2, then URDF, then Gazebo. Skip part 9. Stop before SLAM and Nav2 in
the follow-on series; a pendulum does not navigate.

Networking beyond `ROS_DOMAIN_ID` was deliberately skipped. SSH, Netplan and static IPs
become relevant only when Isaac runs on the desktop and the controller on the laptop.

**Concepts covered, do not re-teach from scratch.** Filesystem tree versus process tree.
Environment variables are copied into a child at fork and never shared afterwards, which
is why `source` exists and why a script cannot change the calling shell. `.bashrc` is
per-user and read once per interactive shell, and GUI-launched apps never read it. What
`colcon build` does for a Python package: copies code into `site-packages`, writes an
empty marker file into the ament index, and generates environment hooks.

## Open threads

1. **Interface design, drafted but unfinished.** Three flows agreed:
   `/joint_states` (`sensor_msgs/JointState`, few hundred Hz) up from the sim;
   `/joint_commands` or `/joint_effort_commands` down to it; and flow 3 needs no new
   topic because `robot_state_publisher` takes the URDF as the `robot_description`
   parameter, subscribes to `/joint_states`, and publishes `/tf` for RViz.
   Failsafe is a receiver-side staleness check on `header.stamp` of the command topic,
   not a separate heartbeat topic, because a separate heartbeat can be alive while the
   command stream is dead.
   **Still owed:** the one-sentence README defence of using `sensor_msgs/JointState` as
   a command message. The honest argument against `trajectory_msgs/JointTrajectory` is
   that it models a timed sequence of future setpoints and would be mostly empty, not
   that it lacks the fields. It has an `effort[]` field.

2. **Unverified claim.** CLAUDE.md above says IsaacSim and IsaacLab run with a pendulum,
   a benchmarking suite and a PD baseline. None of that is on this laptop and the desktop
   install is old. Verify before it reaches a README.

## Working style, learned in practice

- Wants the mechanism, not the incantation. Explain why a command works, not just what to
  type. He will say "I would rather be slow and understand it", and he means it.
- Correct wrong reasoning even when the conclusion is right. He asked to be told.
- Give the inspection command rather than a description of the answer.
  `ros2 interface show`, `ps -f`, `pgrep -a`, `git check-ignore -v`. He can then verify
  claims himself, which he should.
- Analogies to embedded C++ land well: tasks, config structs passed by value, watchdogs,
  flashing firmware. Analogies to web or desktop software do not.
- Scope shrinks late in the evening. When he says there is too much, cut to one goal with
  a single pass or fail criterion rather than reordering the same workload.
- Verify before asserting. Two claims about his files were wrong today, once from a
  case-sensitive grep and once from not checking `.gitignore`. Run the command first.
