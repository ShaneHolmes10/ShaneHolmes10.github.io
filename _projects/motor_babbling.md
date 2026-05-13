---
layout: page
title: Motor Babbling
description: Teaching a simulated robot arm to reach arbitrary targets through reinforcement learning
img: assets/img/motor-babbling-thumbnail.png
importance: 5
category: work
---

## Overview

For this project I built a reinforcement learning system that learns inverse kinematics from scratch. A simulated planar robot arm in MuJoCo gets a target position somewhere in its workspace, and has to figure out, on its own, how to actuate its joints to reach it. The agent observes joint angles, velocities, accelerations, end effector position, and target position. It outputs joint torques. Reward is the negative Euclidean distance to the target. Everything else has to be learned.

The point of the project was not to invent a new algorithm; It was to explore the some common RL algorithms and compare them against each other.

The full source is available on [GitHub](https://github.com/ShaneHolmes10/motor-babbling).

---

## Demo

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/motor_babbling_demo.mp4" class="img-fluid rounded z-depth-1" controls=false autoplay=true loop=true muted=true %}
    </div>
</div>
<div class="caption">
    The video above shows a demo, where the target is being moved around in real time with the arrow keys. We can see that the model is attempting to position the robot end effector at the target position.
</div>

---

## The Challenge

Inverse kinematics for a 2 link planar arm has a closed form solution. The hard part of this project is that the agent never gets to see the kinematics. It only sees joint torques going out and a scalar reward coming back, and it has to discover, over millions of timesteps, that there is structure connecting the two.

Three things make this difficult. 

1. To start that action space is intrinsically continuous. So for RL methods that can only handle a discrete output space the continuous torque must be quantized to a discrete output space. 

2. The number of degrees of freedom. A 1-DOF arm has a single joint to coordinate, but a 2-DOF arm has two joints whose torques have to be applied together to produce a useful motion. The right torque on joint one depends on what joint two is doing, and vice versa. The agent has to learn the joint policy across both, and the difficulty grows quickly as joints are added.


3. The target is randomized. Every episode the target appears at a different position in the workspace, so the agent cannot memorize a single trajectory. It has to learn a policy that generalizes across targets, mapping any given target position to the correct sequence of torques. This is the difference between learning one motion and learning to reach.

---

## What I Built

I used the [Reacher](https://gymnasium.farama.org/environments/mujoco/reacher/) environment in MuJoCo. The arm is configurable in the number of links, so the same code runs as 1-DOF or 2-DOF. The base hangs from a fixed point, the arm swings in a vertical plane under gravity, and the target is a point in the workspace that the end effector has to reach and hold for 100 consecutive simulation steps. The full target task is a 2-DOF arm reaching to randomized targets.
 
I started with a Deep Q-Network (**DQN**), which failed to learn the full 2-DOF task. To check whether DQN itself was capable of solving anything in this environment, I dropped the problem down to 1-DOF and to use a constant target position, where it converged cleanly. The problem was the discrete action space: DQN has to enumerate actions, and quantizing two continuous joints into 10 levels each gives 100 discrete actions, which was too many for DQN to handle on this task.
 
That pushed me to try Deep Deterministic Policy Gradient (**DDPG**), which works directly on continuous action spaces. DDPG learned faster than DQN on the simple 1-DOF task, but training was wildly unstable. Two runs with the same hyperparameters would diverge from each other, the reward curve swung erratically, and on formal evaluation it scored 0% because the result depended entirely on which episode training happened to end on. The algorithm worked in some sense but was not reliable enough to build on.
 
Finally I moved to Soft Actor Critic (**SAC**), which addresses DDPG's stability issues through a stochastic policy, twin critics, and automatic entropy tuning. SAC trained stably on the 1-DOF task, so I scaled the problem back up step by step, carrying the trained checkpoint forward each time: 1-DOF fixed → 1-DOF random targets → 2-DOF random targets. On the full target task it hit roughly 80% success. The policy reliably gets the end effector into the right region of the workspace, even if it does not always have the precision to settle within a tighter radius to the target.
 
The final results across the three algorithms:
- **DQN**: 100% success on 1-DOF fixed target, 0% success on 2-DOF
- **DDPG**: highly unstable, 0% success on formal evaluation
- **SAC**: ~80%  success on 2-DOF random target

---

## Technical Stack

| Layer | Technology |
|---|---|
| Physics simulation | MuJoCo |
| Environment API | Gymnasium |
| Neural networks | PyTorch (CPU only) |
| Algorithms | DQN, DDPG, SAC |
| Plotting and data | Matplotlib, NumPy |
| Languages | Python |

---

## Takeaways

The biggest thing I took from this project is that RL algorithms are actually pretty decent at learning this kind of kinematics. Even on a small arm, SAC was able to land the end effector in the right region of the workspace most of the time. That makes me curious to push the same setup further: more sophisticated algorithms, more complex robot forms (more joints, 3D motion, different morphologies entirely), and tasks beyond static reaching.

---

*Feb 2026 – Apr 2026 · Final Project*