---
layout: post
title:  "Revisit Reinforcement Learning"
date:   2019-08-18 11:32:00 -0800
---
A friend and me studied RL last year. Now is time to write a summary.

RL is about agent making decisions in MDP. Note MDP is a discrete process. It is agent's view that drives everything. In the agent's view, states and actions are parametrized. Agent observes a state, takes an action, receives reward, and observes the next state. How does agent parametrize states? Usually this falls under feature engineering. Agent's parametrization of states can evolve as it learns. Agent doesn't have to understand how states relate to each other. This is called model free RL. But a model whether it is given or learnt can be very useful because agent can plan with it. Technically speaking, model = state transition + reward. How does agent parametrize actions? Usually this is trivial. But actions can be finite (for simplicity discrete == finite) or continuous.


There are value based, policy based, and model based RL. In value based RL, the key is to learn a value function. We have Monte Carlo learning and TD learning. In Monte Carlo learning, value function is updated (many times) at end of episode, while in TD learning it is updated once per step. Note because states are parametrized, a value function update can affect the values of all states. There are state value and Q value functions. In model free RL, agent should learn the Q value function. How does agent decide which actions to take while learning? Greedy (SARSA) and epsilon greedy (Q-learning) are common choices which are examples of on policy and off policy learning. The agent can integrate learning and planning, e.g. Dyna architecture.


[Next time I will talk about policy based RL and actor-critic methods.]
