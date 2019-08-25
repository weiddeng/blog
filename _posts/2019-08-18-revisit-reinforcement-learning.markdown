---
layout: post
title:  "Revisit Reinforcement Learning"
date:   2019-08-18 11:32:00 -0800
---
A friend and me studied RL last year, and finally it is time to write a summary.


RL is about agent making decisions in MDP. Note MDP is a discrete process. It is agent's view that drives everything. In the agent's view, states and actions are parametrized. Agent observes a state, takes an action, receives reward, and observes the next state. Agent's parametrization of states falls under feature engineering, and it can evolve as the agent learns. In model free RL agent doesn't have to understand how states relate to each other. However, a model (model = state transition + reward) whether given or learnt can be very useful because it can be used in planning. Agent's parametrization of actions is usually straightforward. But actions can be finite (for simplicity discrete == finite) or continuous.


There are value based, policy based, and model based RL. In value based RL, the key is to learn a value function, which can be a DNN. We have Monte Carlo learning and TD learning to access perceived ground truth in order to update the value function. In MC learning, the value function is updated in batch at end of episode. In TD learning it is updated once per step. Because states and actions are parametrized, an update can affect all states. There are state value function and Q function. In model free RL, the agent should learn the Q function. How does the agent look forward and decide which actions to take while learning? - greedy as in SARSA (typical on policy learning) or epsilon-greedy as in Q-learning (typical off policy learning) are common choices; the agent can also learn a model and integrate learning and planning as in Dyna, Dyna-Q, and Monte Carlo Tree Search.


A policy is a function mapping each state to a distribution of actions. It can be stochastic or deterministic. If a policy is deterministic, then it maps each state to a deterministic action. In policy based RL, the agent learns a policy (parametrized by theta, often a DNN) directly through gradient ascent. We need to deep dive the Policy Gradient Theorem, but for now, just assume the gradient of the return on theta is the expectation of (Q function - baseline)\*(score function). Next, let's look at a few policy based RL agents using [TF-Agents][TF-Agents] as example.

[REINFORCE][REINFORCE]

[DDPG][DDPG]


[TF-Agents]:https://github.com/tensorflow/agents
[REINFORCE]:https://github.com/tensorflow/agents/blob/master/tf_agents/agents/reinforce/reinforce_agent.py
[DDPG]:https://github.com/tensorflow/agents/blob/master/tf_agents/agents/ddpg/ddpg_agent.py
