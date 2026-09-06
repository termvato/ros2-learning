I'm learning ROS 2 from scratch and building a portfolio project with it. Help me work through both.

**Context on me.** 2026 Warwick BEng Mechanical Engineering, First Class. Strong on hardware: mechanism design in Fusion, four-layer PCB in EasyEDA, embedded C++ on a Teensy 4.0 with a 120 Hz sensor fusion loop and an independent 1 kHz safety interrupt. My final year project was a monopedal jumping robot, built solo, public at github.com/termvato/MRV. Python is around LeetCode-medium. I have IsaacSim and IsaacLab running with an inverted pendulum using parameters similar to my robot, plus a benchmarking suite and a PD baseline. I have never used ROS or ROS 2, and I know embedded and control rather than middleware or Linux tooling.

**Why.** ROS 2 appears in essentially every robotics job description I want and it's my biggest gap. I'm job hunting now, so the output needs to be a repo someone can click, not a certificate.

**The plan, argue with it if it's wrong.** Work through the Articulated Robotics ROS 2 series to learn the fundamentals, using the official docs alongside. Then build the actual portfolio piece: a ROS 2 interface to my IsaacSim pendulum. Standard message types, no custom ones unless justified. The controller runs as a separate node outside Isaac, so it doesn't know whether it's talking to sim or hardware. RViz for visualisation, ros2 bag for logging, official Isaac ROS 2 bridge rather than something homemade, and a README explaining the interface design and why it's structured that way.

I do not want to build another differential-drive robot in Gazebo as the portfolio piece. Fine as a learning exercise inside the course, but it's the most-built project in robotics and adds nothing.

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