Project 2 - Continous Control
===

Train a Set of Robotic Arms :robot_face:

Udacity Deep Reinforcement Learning Nanodegree

## Table of Contents
1. [ Summary ](#sum)
2. [ Learning Algorithm ](#algo)
3. [ Result ](#res)
4. [ Ideas of Future Work ](#fut)

<a name="sum"></a>
## Summary

The project is based on Unity Environment. The agent is trained to follow target location!

A reward of +0.1 is provided for each step that the agent's hand is in the goal location. Thus, the goal of your agent is to maintain its position at the target location for as many time steps as possible.

The observation space consists of 33 variables corresponding to position, rotation, velocity, and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

<a name="algo"></a>
## Learning Algorithm
---

#### Deep Deterministic Policy Gradient (DDPG)

The algorithm I chose to model my project on is outlined in the paper, Continuous Control with Deep Reinforcement Learning, by researchers at Google Deepmind. In this paper, the authors present "a model-free, off-policy actor-critic algorithm using deep function approximators that can learn policies in high-dimensional, continuous action spaces." They highlight that DDPG can be viewed as an extension of Deep Q-learning to continuous tasks.

I used this vanilla, single-agent DDPG as a template. I further experimented with the DDPG algorithm based on other concepts covered in Udacity's classroom and lessons. My understanding and implementation of this algorithm (including various customizations) are discussed below.

#### Actor-Critic Method

The basic algorithm lying under the hood is an actor-critic method. Policy-based methods like REINFORCE, which use a Monte-Carlo estimate, have the problem of high variance. TD estimates used in value-based methods have low bias and low variance. Actor-critic methods marry these two ideas where the actor is a neural network which updates the policy and the critic is another neural network which evaluates the policy being learned which is, in turn, used to train the actor.


There are also a few techniques which contributed significantly towards stabilizing the training:
- Fixed Q-target: 2 different networks are combined in order to keep the method off-policy. A target network is updated at every iteration while an evalution network at the end of updating phase.
- Experience Replay: In order to decouple sequential states of each episode, Replay buffer <S,a,R,S'> is created. At each iteration a random batch is pulled from buffer.
- Soft Updates: In DQN, the target networks are updated by copying all the weights from the local networks after a certain number of epochs. However, in DDPG, the target networks are updated using soft updates where during each update step, 0.01% of the local network weights are mixed with the target networks weights.

I used Linear Neural Network architecture. Also, in my experience, I have found Batch normalization to have always improved training and hence, I added Batch normalization layer in both actor and critic.

Parameters used in DQN algorithm:
- Maximum steps per episode: 1000
- Batch Size: 128
- Buffer size: 1e5
- Gamma: 0.99
- Learning rate (Actor): 2e-4
- Learning rate (Critic): 2e-4
- tau: 1e-3
- Weight Decay: 0

<a name="res"></a>
Result
---

The agent's performance within the Reacher environment was measured by the average score based on 1000 episodes.

![](https://github.com/Ansheel9/P2-Continous-Control-DeepRL/blob/master/Images/plot.png)

Environment ended on 147 episode, (having average score of 30.03).

<a name="fut"></a>
Ideas of Future Work
---

- Other algorithms like TRPO, PPO, A3C, A2C that have been discussed in the course could potentially lead to better results as well.
- Using Prioritized Replay which has generally shown to have been quite useful. It is expected that it'll lead to an improved performance here too.
- The Q-prop algorithm, which combines both off-policy and on-policy learning, could be good one to try.
