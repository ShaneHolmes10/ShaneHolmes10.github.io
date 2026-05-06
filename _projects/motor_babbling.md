---
layout: page
title: Motor Babbling
description: Teaching a simulated robot arm to reach arbitrary targets through reinforcement learning
img: assets/img/motor-babbling-thumbnail.png
importance: 5
category: work
---

## Overview

For my final project in an AI course I built a reinforcement learning system that learns inverse kinematics from scratch. A simulated planar robot arm in MuJoCo gets a target position somewhere in its workspace, and has to figure out, on its own, how to actuate its joints to reach it. There is no analytical IK solver in the loop. The agent observes joint angles, velocities, accelerations, end effector position, and target position. It outputs joint torques. Reward is the negative Euclidean distance to the target. Everything else has to be learned.

The point of the project was not to invent a new algorithm. It was to implement three well known RL algorithms — DQN, DDPG, and SAC — against the same environment, and to actually feel the differences between value based, deterministic actor critic, and stochastic actor critic methods on a problem I built myself.

---

## See It In Action

<iframe
  src="https://www.youtube.com/embed/PLACEHOLDER_VIDEO_ID"
  class="w-100 rounded z-depth-1"
  style="aspect-ratio: 16/9; border: none;"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

---

## The Challenge

Inverse kinematics for a 2 link planar arm has a closed form solution. The hard part of this project is not the kinematics, it is that the agent never gets to see the kinematics. It only sees joint torques going out and a scalar reward coming back, and it has to discover, over millions of timesteps, that there is structure connecting the two.

Three things make this harder than it sounds. First, action spaces. A torque controlled arm is fundamentally continuous, but DQN only handles discrete actions. To use DQN at all I had to quantize torques into bins, and the size of the discrete action space grows as bins to the power of joints. Second, exploration. The reward signal only changes meaningfully when the end effector is near the target, and an agent that never happens to swing close to the target never sees the reward shape clearly. How exploration is handled — epsilon greedy in DQN, additive Gaussian noise in DDPG, entropy maximization in SAC — ends up mattering more than I expected. Third, stability. Actor critic methods are notoriously brittle, and small changes to learning rates or soft update coefficients can be the difference between a policy that converges and one that diverges silently.

---

## What I Built

I built the environment from scratch in MuJoCo using a generated MJCF XML. The arm is configurable in the number of links, so the same code runs as 1-DOF or 2-DOF. The base hangs from a fixed point and the arm swings in a vertical plane. Gravity is on, joint damping is set to 0.5 to keep the dynamics from being purely conservative. The environment exposes both a continuous action space (a Box of normalized torques per joint) and a discrete one (a single integer that gets unpacked into per joint torque bins). The same step function handles both, which kept the algorithm code clean and made the DQN versus DDPG and SAC comparison apples to apples on the underlying physics.

Episodes only terminate when the end effector has stayed inside a target radius for 100 consecutive simulation steps. This was a deliberate choice. An earlier version terminated on first contact with the target, and the agent learned a fling and pray policy where it would whip the arm through the target on momentum rather than learn to stabilize there. Requiring sustained contact forced it to actually slow down and hold position.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/motor_babbling_environment.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Above the environment I put a thin abstract base class for agents, with five required methods: select_action, store_transition, train_step, save, and load. The training script loads agents dynamically by name with importlib, so adding a new algorithm only requires creating a `{name}_agent.py` file with a class named `{NAME}Agent` that inherits from BaseAgent. No registry to update, no switch statement to grow. This sounds like over engineering for three algorithms, but it paid for itself the moment I wanted to swap SAC in after DDPG was already integrated, and again when I wanted to evaluate different agents without touching the eval code.

I started with DQN because it was the simplest of the three and the natural connection back to the course material. The Q network is a two layer MLP with 128 hidden units, exploration is epsilon greedy with exponential decay, and the target network is updated as a hard copy every 100 training steps. On a 1-DOF arm with a fixed target, DQN solved the problem cleanly with a 100% evaluation success rate. The reward curve is the textbook learning curve you hope for.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/motor_babbling_dqn.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Then I moved to 2-DOF. The action space jumped from 10 to 100 discrete actions, and DQN fell over completely. 0% success rate on evaluation. The Q values were noisy, the policy never settled, and adding episodes just delayed the same outcome instead of fixing it. This was the clearest motivation for moving to a continuous action method I could have asked for. The wall was real and predicted by the theory, and watching it happen on my own code made the abstract argument concrete.

DDPG was the next step. The actor is a deterministic policy network outputting torques in negative one to one through a tanh, and the critic is a Q network that takes state and action together. Both have target networks updated with soft Polyak averaging. Exploration is additive Gaussian noise on the actor output during training. DDPG technically worked, the reward curve trends upward and the actor loss decreases, but it was the least stable thing I have ever trained. Two runs with identical hyperparameters would diverge from each other after a few hundred episodes. On a formal 100 episode evaluation it scored 0% success. On informal watch sessions you could see it sometimes reach the target and sometimes flail past it. The policy was sensitive to exactly when training was stopped, which is not a property you want.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/motor_babbling_ddpg.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

This matched what the literature says about DDPG. Gaussian noise has no relationship to whether the current state is well explored or not, and the deterministic actor can collapse into local optima when the critic is wrong about the value of nearby actions. I left DDPG in the writeup as a cautionary result rather than burying it.

SAC fixes both of DDPG's main problems. The actor is stochastic, outputting the mean and log standard deviation of a Gaussian over actions with a tanh squash to bound them. Exploration is part of the policy itself rather than bolted on. The critic is two Q networks and the minimum is used as the target, which prevents the optimistic value estimates that destabilize single critic methods. The entropy coefficient is tuned automatically against a target entropy of negative action_dim, so I do not have to hand schedule exploration at all. SAC trained stably and converged on the 2-DOF random target task, hitting roughly 80% success at a 10cm threshold and roughly 55% at a 5cm threshold. The framing matters: the policy reliably gets the end effector into the correct workspace region, but it does not have the fine motor precision to settle within 5cm every time.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/motor_babbling_sac.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

After the SAC policy was working at 60Nm of max torque, I tried a curriculum step: take the trained policy and continue training with the max torque doubled to 120Nm. The hypothesis was that more torque headroom would give the policy more options for fine corrections near the target. It did not. Success rate dropped from about 71% to about 28% and never recovered in the additional training I gave it. The policy had clearly memorized something about the dynamics at 60Nm, the relationship between commanded torque and resulting acceleration, and doubling that relationship in a single jump invalidated whatever it had learned. The lesson, which I should have known going in, is that curriculum learning needs small steps. A 2x change in a parameter the policy has implicitly modeled is not a curriculum, it is a different problem.

The final results across the three algorithms:
- **DQN**: 100% on 1-DOF fixed target, 0% on 2-DOF
- **DDPG**: highly unstable, 0% on formal evaluation
- **SAC**: ~80% at 10cm threshold, ~55% at 5cm threshold on 2-DOF random targets

---

## Technical Stack

| Layer | Technology |
|---|---|
| Physics simulation | MuJoCo, MJCF XML |
| Environment API | Gymnasium |
| Neural networks | PyTorch (CPU only) |
| Algorithms | DQN, DDPG, SAC, all from scratch on a shared BaseAgent interface |
| Training script | Single CLI with `--agent` flag and train/eval/play subcommands |
| Plotting and data | Matplotlib, NumPy |
| Languages | Python |

---

## Takeaways

The single biggest thing I took from this project is that exploration is not a parameter, it is the design choice that determines whether your agent learns anything at all. Epsilon greedy on DQN, additive noise on DDPG, and entropy maximization on SAC are not three flavors of the same idea, they produce qualitatively different training behavior, and the algorithm that worked is the one whose exploration is integrated into the policy rather than added as a wrapper.

Building the environment myself made the algorithm comparison sharper than it would have been on a stock benchmark. When DQN failed at 2-DOF I knew exactly why, because I had picked the bins. When DDPG was unstable I had ruled out environment bugs because I had written every line of the environment. The cost of building from scratch was real, but it bought a level of confidence in the results that I would not have had with a black box gym. Negative results belong in the writeup too. The DDPG instability and the curriculum learning regression are arguably the most informative parts of the project, because they are the points where the textbook story did not survive contact with my actual code.

---

*Jan 2025 – Apr 2025 · Final Project*