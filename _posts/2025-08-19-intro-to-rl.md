---
title: Introduction to Reinforcement Learning
date: 2025-06-10 00:00:00 +0800
categories: [Blog, Robotics]
tags: [learning, rl]     
author: <author_id>
mermaid: true
pin: false
math: true
image: /assets/images/ldr.png
---

*In Progress*

# Introduction to Reinforcement Learning

This blog is a collection of my notes based on the book “Reinforcement Learning: An Introduction by Richard S. Sutton and Andrew G. Barto”. 

# Finite Markov Decision Processes

## Context

![The agent-environment interaction](Introduction%20to%20Reinforcement%20Learning%202530b449d41980ac9b8cc4af04f345ba/image.png)

The agent-environment interaction

Reinforcement learning is built around the interaction between an agent and an environment over time. At each step:

- The agent observes the current situation (state).
- It chooses an action based on a policy.
- The environment responds by providing a reward and transitioning to a new state.

This back-and-forth loop defines the learning problem.

Discounting: The agent tries to select actions so that the sum of the discounted rewards it receives over the future is maximized.

<aside>
💡

$$
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \cdots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}
$$

</aside>

where $0 \le \gamma \le 1,$  is called the discount rate.

The discount rate determines the present value of future rewards: a reward received $k$ time steps in the future is worth only $\gamma^{k-1}$ times what it would be worth if it were received immediately. If $\gamma = 0,$  the agent is “myopic” in being concerned only with maximizing immediate rewards.

In case of episodic tasks that have a “termination state”, the discounted rewards can be written as

<aside>
💡

$$
G_t =  \sum_{k=0}^{T-t-1} \gamma^k R_{t+k+1}
$$

</aside>

## Markov Decision Process

In the most general case, the environment’s response at time $t+1$ can depend on everything that has happened earlier. Thus, the dynamics can be defined by the complete probability distribution:

$$
\Pr\{ R_{t+1} = r , S_{t+1} = s' \mid S_0, A_0, R_1, \ldots, S_{t-1}, A_{t-1}, R_t, S_t, A_t \}
$$

If the state signal has the Markov property, then the environment’s response at time $t+1$ depends only on the current state and action. In this case, the environment’s dynamics are simplified to:

$$
p(s', r \mid s, a) = \Pr\{ R_{t+1} = r,  S_{t+1} = s' \mid S_t = s, A_t = a \}
$$

Given these dynamics, we can compute everything else we might want to know about the environment.

**Expected reward for a state–action pair:**

<aside>
💡

$$
r(s,a)= \mathbb{E}[R_{t+1}\mid S_t=s, A_t=a]  = \sum_{r\in\mathcal{R}} r \sum_{s'\in\mathcal{S}} p(s',r\mid s,a).
$$

</aside>

From definition of conditional expectation

$$
\mathbb{E}[R_{t+1}\mid S_t=s, A_t=a] = \sum_{r\in\mathcal{R}} r \Pr\{R_{t+1}=r \mid S_t=s, A_t=a\}
$$

Using law of total probability

$$
\Pr\{R_{t+1}=r \mid S_t=s, A_t=a\}

= \sum_{s'\in\mathcal{S}} \Pr\{S_{t+1}=s', R_{t+1}=r \mid S_t=s, A_t=a\} \\

= \sum_{s'\in\mathcal{S}} p(s',r\mid s,a).
$$

Plugging back in, we get

$$
r(s,a)=   \sum_{r\in\mathcal{R}} r \sum_{s'\in\mathcal{S}} p(s',r\mid s,a)
$$

**State transition probability:**

<aside>
💡

$$
p(s'\mid s,a)
= \sum_{r\in\mathcal{R}} \Pr\{S_{t+1}=s', R_{t+1}=r \mid S_t=s, A_t=a\}

= \sum_{r\in\mathcal{R}} p(s',r\mid s,a).
$$

</aside>

**Expected rewards for state–action–next-state triples:**

<aside>
💡

$$
r(s,a,s')  = \mathbb{E}[R_{t+1}\mid S_t=s, A_t=a, S_{t+1}=s']

= \frac{\sum_{r\in\mathcal{R}} r p(s',r\mid s,a)}{p(s'\mid s,a)}
$$

</aside>

Start from conditional expectation

$$
r(s,a,s') = \mathbb{E}[R_{t+1}\mid S_t=s, A_t=a, S_{t+1}=s'] \\  =\sum_{r\in\mathcal{R}} r \Pr\{R_{t+1}=r \mid S_t=s, A_t=a, S_{t+1}=s'\}
$$

Then using Bayes’ rule

$$
\Pr\{R_{t+1}=r \mid S_t=s, A_t=a, S_{t+1}=s'\}

=\frac{\Pr\{S_{t+1}=s', R_{t+1}=r \mid S_t=s, A_t=a\}}{\Pr\{S_{t+1}=s' \mid S_t=s, A_t=a\}}

=\frac{p(s',r\mid s,a)}{p(s'\mid s,a)}.
$$

Plugging this in above, we get

$$
r(s,a,s') 
= \frac{\sum_{r\in\mathcal{R}} r p(s',r\mid s,a)}{p(s'\mid s,a)}
$$

## Value Functions

### State-Value Function

<aside>
💡

$$
v_{\pi}(s) = \mathbb{E}_{\pi}[G_t \mid S_t = s]

= \mathbb{E}_{\pi}\left[ \sum_{k=0}^{\infty} \gamma^k R_{t+k+1} \middle| S_t = s \right]
$$

</aside>

This is the expected long-term return (sum of discounted rewards) if you start in state $s$ and follow policy $\pi$.

***Intuition:** “If I’m standing in this state, and I keep behaving according to my current policy, how good is this situation in the long run?”*

### Action-Value Function

<aside>
💡

$$
q_{\pi}(s,a) = \mathbb{E}_{\pi}[G_t \mid S_t = s, A_t = a]

= \mathbb{E}_{\pi}\left[ \sum_{k=0}^{\infty} \gamma^k R_{t+k+1} \middle| S_t = s, A_t = a \right]
$$

</aside>

This is the expected long-term return if you start in state $s,$ take action $a$ immediately, and then afterwards follow policy $\pi$.

***Intuition:** “If I’m in this state and I try this particular move right now, and then keep following my usual policy, how good will things turn out?”*

### **Relationship between state-value and action-value functions**

<aside>
💡

$$
v_\pi(s) = \sum_a\pi(a|s)\space q_\pi(s,a)
$$

</aside>

The value functions $v_\pi$ and $q_\pi$ can be estimated from experience. For example, if an agent follows policy $\pi$ and maintains an average, for each state encountered, of the actual returns that have followed that state, then the average will converge to the state’s value, $v_\pi(s),$ as the number of times that state is encountered approaches infinity. If separate averages are kept for each action taken in a state, then these averages will similarly converge to the action values, $q_\pi(s,a).$  These estimation methods are called Monte Carlo methods, and are discussed in the sections below.

### Bellman Equation

$$
v_{\pi}(s) = \mathbb{E}_{\pi}[G_t \mid S_t = s] =  \mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1} \mid S_t = s]
$$

Consider random variables $X,Y,Z$

$$
\mathbb{E}[X \mid Z] = \sum_y \mathbb{E}[X \mid Y=y, Z] \space \Pr(Y=y \mid Z)
$$

Let 

$$
X = R_{t+1} + \gamma G_{t+1}, \quad Y = A_t, \quad Z = \{ S_t=s \}
$$

Then

$$
\mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1}  \mid S_t=s] = \sum_a \mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1}  \mid S_t=s, A_t=a]  \Pr(A_t=a \mid S_t=s) \\ v_\pi(s)
 = \sum_a \pi(a \mid s) \space  \mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1}  \mid S_t=s, A_t=a] 
$$

Applying the same law again

$$
\mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1} \mid S_t=s, A_t=a]
\\
= \sum_{s'} \sum_r \mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1} \mid S_t=s, A_t=a, S_{t+1}=s', R_{t+1}=r] \space  p(s',r \mid s,a)
$$

Given $R_{t+1} = r$ we know the first term is just $r.$

For the second term, by Markov property, the future return $G_{t+1}$ depends on the future only through $S_{t+1},$ so

$$
\mathbb{E}_{\pi}[G_{t+1} \mid S_t=s, A_t=a, S_{t+1}=s', R_{t+1}=r] = \mathbb{E}_{\pi}[ G_{t+1} \mid S_{t+1}=s']  
$$

Therefore the inner expectation becomes

$$
r + \gamma \space \mathbb{E}_{\pi}[ G_{t+1} \mid S_{t+1}=s'] 
$$

Plugging this back in, we get

$$
v_{\pi}(s) = \sum_a \pi(a \mid s) \sum_{s',r} p(s',r \mid s,a) \Big[ r + \gamma \mathbb{E}_{\pi}[ G_{t+1} \mid S_{t+1}=s']  \Big]
$$

Finally, recognise that $\mathbb{E}_{\pi}[ G_{t+1} \mid S_{t+1}=s']  = v_\pi(s')$. So we have

<aside>
💡

$$
v_{\pi}(s) = \sum_a \pi(a \mid s) \sum_{s',r} p(s',r \mid s,a) \Big[ r + \gamma v_{\pi}(s') \Big]
$$

</aside>

**Intuition:**

In a nutshell, the Bellman equation says that:  The value of a state = immediate reward + future value. But instead of computing everything into the infinite future directly, it breaks the problem down recursively. 

The value of a state $v_\pi(s)$ is obtained by:

1. Looking at all possible actions $a$ that policy $\pi$ might choose
2. For each action, look at all possible next states $s'$ and rewards $r$ that the environment could produce
3. Weight each outcome by how likely it is
4. Add up the immediate reward $r$ plus the discounted value of the next state $\gamma v_\pi(s')$

## Optimal Value Functions

There exists at least one policy that is better than or equal to all other policies. This is called the optimal policy. 

**Optimal state-value function:**

<aside>
💡

$$
v_*(s) = \max_{\pi} v_\pi(s)
$$

</aside>

for all $s \in S$.

**Optimal action-value function:**

<aside>
💡

$$
q_*(s,a) = \max_\pi q_\pi(s,a)
$$

</aside>

for all $s \in S, \space a \in \mathcal{A}(s).$ 

For the state-action pair $(s,a)$, this function gives the expected return for taking action $a$ in state $s$ and thereafter following an optimal policy. Thus, we can write $q_*$ in terms of $v_*$ as:

<aside>
💡

$$
q_*(s,a) = \mathbb{E}_{\pi}[ R_{t+1} + \gamma v_*(S_{t+1}) \mid S_{t}=s, A_t=a] 
$$

</aside>

### Bellman Optimality Equation

**Bellman Optimality for state-value function:**

It expresses the fact that the value of a state under an optimal policy must equal the expected return for the best action from that state.

$$
{v_{*}(s)} = \max_{a \in \mathcal{A}(s)} q_{\pi_{}}(s,a) \newline = \max_{a} \mathbb{E}_{\pi_{*}}\left[G_t \middle| S_t = s, A_t = a \right] \newline 
= \max_{a} \mathbb{E}_{\pi_{*}}\left[ \sum_{k=0}^{\infty} \gamma^k R_{t+k+1} \middle| S_t = s, A_t = a \right] \newline
= \max_{a} \mathbb{E}_{\pi_{*}}\left[ R_{t+1} + \gamma \sum_{k=0}^{\infty} \gamma^k R_{t+k+2} \middle| S_t = s, A_t = a \right] \newline
= \max_{a} \mathbb{E}[R_{t+1} + \gamma v_{*}(S_{t+1}) \mid S_t = s, A_t = a] \newline
= \max_{a \in \mathcal{A}(s)} \sum_{s',r} p(s',r \mid s,a) \big[ r + \gamma v_{*}(s') \big]

$$

So,

<aside>
💡

$$
v_*(s)= \max_{a} \mathbb{E}[R_{t+1} + \gamma v_{*}(S_{t+1}) \mid S_t = s, A_t = a]\\ = \max_{a \in \mathcal{A}(s)} \sum_{s',r} p(s',r \mid s,a) \big[ r + \gamma v_{*}(s') \big]
$$

</aside>

**Bellman Optimality for action-value function:**

<aside>
💡

$$
q_*(s,a) = \mathbb{E}\big[R_{t+1} + \gamma \max_{a'}q_*(S_{t+1},a') \mid S_t=s, A_t=a\big] \\
= \sum_{s',r}p(s',r \mid s,a) \big[r + \gamma \max_{a'}q_*(s',a') \big]
$$

</aside>

![Backup diagrams for (a) v∗ and (b) q∗](Introduction%20to%20Reinforcement%20Learning%202530b449d41980ac9b8cc4af04f345ba/image%201.png)

Backup diagrams for (a) v∗ and (b) q∗

### Optimal Value Functions to Optimal Policies

**Using the Optimal State-Value Function $v_*$:**

Once we know $v_*,$ we can derive an optimal policy by:

- Looking at the actions that achieve the maximum in the Bellman optimality equation
- Any policy that assigns probability only to these maximizing actions is optimal

This process is like a one-step lookahead search — $v_*$ already accounts for all long-term returns, so just looking at the immediate action + expected next-state values is enough.

*Simply put, a policy that is greedy w.r.t $v_{\*}$ (chooses the best action based only on short-term consequences evaluated via $v_{\*}$) is optimal in the long run, because $v_{\*}$ already encodes all future returns.*

**Using the Optimal Action-Value Function $q_*$:**

With $q_\*$ the process is even simpler. For any state $s$, just pick the action $a$ that maximizes $q_\*(s,a)$.

This is easier because:

- $q_*$ already represents the cached results of the one-step lookahead
- No need to know transition probabilities $p(s',r \mid s,a)$ or successor states
- Directly gives the optimal expected return for each state–action pair

**Key insights:**

- $v_* \implies$need one-step lookahead to find best actions
- $q_* \implies$**no lookahead needed**, just take the $\max_a q_\*(s,a)$

## Practical Limits

1. **True optimality is rarely achievable** 
    - Computing the exact optimal policy is usually too expensive (requires solving the Bellman optimality equation exactly).
    - Even with a complete model of the environment, tasks like chess are too complex to solve optimally.
2. **Constraints in practice**
    - Computation per step is limited (agent can’t spend forever planning)
    - Memory is limited (can’t store values for every possible state)
3. **Tabular vs Function Approximation**
    - Tabular methods: possible when state/action space is small (store values in arrays/tables)
    - Function approximation: required when state space is huge or continuous (use compact parameterized functions, e.g. neural networks)
4. **Approximating optimal behavior**
    - Not all states matter equally
    - Agents can focus on frequent states and ignore rare states with little effect on overall performance

# Dynamic Programming

## Policy Evaluation

## Policy Improvement

## Policy Iteration

## Value Iteration

## Asynchronous Dynamic Programming

## Generalized Policy Iteration

## Efficiency

# Monte Carlo Methods

# Temporal Difference Learning