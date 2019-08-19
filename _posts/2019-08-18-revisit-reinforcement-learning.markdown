---
layout: post
title:  "Revisit Reinforcement Learning"
date:   2019-08-18 11:32:00 -0800
---
A friend and me studied RL last year. Now is time to write a summary.


RL is about agent making decisions in MDP. Note MDP is a discrete process. It is agent's view that drives everything. In the agent's view, states and actions are parametrized. Agent observes a state, takes an action, receives reward, and observes the next state. Agent's parametrization of states falls under feature engineering, and it can evolve as the agent learns. The agent doesn't have to understand how states relate to each other - this is called model free RL. However, a model whether given or learnt can be very useful because it can be used in planning. Technically speaking, model = state transition + reward. The parametrization of actions is usually trivial. But actions can be finite (for simplicity discrete == finite) or continuous.


There are value based, policy based, and model based RL. In value based RL, the key is to learn a value function. We have Monte Carlo learning and TD learning. In Monte Carlo learning, value function is updated (many times) at end of episode, while in TD learning it is updated once per step. Note because states and actions are parametrized, an update can affect the values of all states. There are state value function and Q value function. In model free RL, the agent should learn the Q value function. How does the agent decide which actions to take while learning? greedy (SARSA, on policy learning), epsilon-greedy (Q-learning, off policy learning) are common choices. The agent can integrate learning and planning, e.g. Dyna architecture.


Next time I will talk about policy based RL and actor-critic methods using [TF-Agents][TF-Agents] as example.


[TF-Agents]:https://github.com/tensorflow/agents
