# NOTES
- The most imp feature that distinguish reinforcement learning from other types of learning is that it uses training info that **evaluates** the action taken rather than **instructs** by giving correct action. 
- **Purely evaluative feedback** indicates how good the action taken was, but not whether it was the best or the worst action possible.
- **Purely instructive feedback**, on the other hand, indicates the correct action to take, independently of the action actually taken. This kind of feedback is the basis of supervised learning, which includes large parts of pattern classification, artificial neural networks, and system identification.

# A k-armed bandit Problem
- You are faced repeatedly with a choice among k different options, or actions. After each choice you receive a numerical reward chosen from a stationary probability distribution that depends on the action you selected. Objective is to maximize the expected total reward over some period of time.

## Action Values in the k-Armed Bandit Problem

In our *k*-armed bandit problem, each of the *k* actions has an expected (mean) reward given that the action is selected. We call this the **value** of that action.

Let:

- $A_t$ = action selected at time step $t$
- $R_t$ = reward received at time step $t$

The value of an arbitrary action $a$, denoted by $q_*(a)$, is the expected reward obtained when action $a$ is selected:

$$
q_*(a) = \mathbb{E}[R_t \mid A_t = a]
$$

If the value of each action were known exactly, solving the *k*-armed bandit problem would be trivial: always choose the action with the highest value.

In practice, the true action values are unknown. Instead, we maintain estimates of them.

The estimated value of action $a$ at time step $t$ is denoted by:

$$
Q_t(a)
$$

Our goal is for the estimate $Q_t(a)$ to become as close as possible to the true value $q_*(a)$.

- If you maintain estimates of the action values, then at any time step there is at least
one action whose estimated value is greatest. We call these the **greedy actions**. When you
select one of these actions, we say that you are exploiting your current knowledge of the
values of the actions. If instead you select one of the nongreedy actions, then we say you
are **exploring**, because this enables you to improve your estimate of the nongreedy action’s
value. 
- **Exploitation is the right thing to do to maximize the expected reward on the one
step, but exploration may produce the greater total reward in the long run.**
- It is not possible both to explore and to exploit with any single action selection, one often refers to the “conflict between exploration and exploitation".

## Action- Values Method


-Concept

The true action value $q_*(a)$ is generally unknown.

To estimate it, we use the average of all rewards received when action $a$ has been selected.

This approach is called the **Sample Average Method**.

---



The estimated value of action $a$ at time step $t$ is:

$$
Q_t(a)
=
\frac{\text{Sum of rewards received when } a \text{ was chosen}}
{\text{Number of times } a \text{ was chosen}}
$$

Equivalent mathematical form:

$$
Q_t(a)
=
\frac{\sum_{i=1}^{t-1} R_i \cdot \mathbf{1}_{A_i=a}}
{\sum_{i=1}^{t-1} \mathbf{1}_{A_i=a}}
$$

where:

- $R_i$ = reward received at step $i$
- $A_i$ = action selected at step $i$
- $\mathbf{1}_{A_i=a}$ = indicator function
  - 1 if action $a$ was selected
  - 0 otherwise

---

## Why Does It Work?

- It averages all observed rewards for an action.
- As more samples are collected, the estimate becomes more accurate.
- By the Law of Large Numbers:

$$
Q_t(a) \rightarrow q_*(a)
\quad \text{as} \quad t \rightarrow \infty
$$

---

## Greedy Action Selection

Once action values are estimated, the simplest strategy is to choose the action with the highest estimated value.

Formula:

$$
A_t = \arg\max_a Q_t(a)
$$

where:

- $\arg\max_a$ returns the action with the highest estimated value.
- If multiple actions have the same value, ties can be broken randomly.

---

## Key Takeaways

- True action values $q_*(a)$ are unknown.
- We estimate them using sample averages.
- Sample-average estimates converge to the true values over time.
- Greedy action selection chooses the action with the highest estimate.
- Pure greedy methods exploit current knowledge.
- Greedy methods do not explore potentially better actions.

---

- Greedy action selection always exploits current knowledge to maximize immediate reward; it spends no time at all sampling apparently inferior actions to see if they might really be better. A simple alternative is to behave greedily most of the time, but every once in a while, say with small probability $\epsilon$.

ε-greedy is a near-greedy action selection strategy.

At each time step:

- With probability **ε**, choose a random action (**exploration**).
- With probability **1 − ε**, choose the action with the highest estimated value (**exploitation**).

---

## Why Use ε-Greedy?

Pure greedy methods always exploit current knowledge and may miss better actions.

ε-greedy introduces occasional exploration, allowing the agent to:

- Discover potentially better actions.
- Avoid getting stuck with suboptimal estimates.
- Balance exploration and exploitation.

---

## Long-Term Properties

As the number of time steps increases:

- Every action is sampled infinitely many times.
- The estimated action values $Q_t(a)$ converge to the true action values $q_*(a)$.
- The agent becomes increasingly confident about which action is optimal.

---

## Convergence Result

Because the estimates become more accurate:

- The probability of selecting the optimal action approaches **greater than $1-\epsilon$**.
- For small values of ε, the optimal action is selected almost all the time.

---

## Advantages

- Simple to implement.
- Ensures continuous exploration.
- Prevents premature convergence to poor actions.
- Guarantees learning of all action values in the long run.

---

## Limitations

- Random exploration continues forever.
- Some non-optimal actions are still selected even after learning.
- Asymptotic guarantees do not necessarily imply strong short-term performance.

---

## Key Takeaways

- ε-greedy balances exploration and exploitation.
- Exploration occurs with probability ε.
- Exploitation occurs with probability 1 − ε.
- All actions continue to be sampled.
- $Q_t(a)$ converges to $q_*(a)$ over time.
- The probability of selecting the optimal action approaches $1-\epsilon$.

---
## Problem

In ε-greedy action selection, for the case of two actions and

$$
\epsilon = 0.5
$$

what is the probability that the greedy action is selected?

- Step 1: Recall the ε-Greedy Rule

In ε-greedy action selection:

- With probability $1-\epsilon$, the greedy action is selected.
- With probability $\epsilon$, a random action is selected.

Since:

$$
\epsilon = 0.5
$$

the probabilities become:

- Exploitation (greedy choice): $1 - 0.5 = 0.5$
- Exploration (random choice): $0.5$


- Step 2: Probability of Selecting the Greedy Action During Exploration

There are two actions.

During exploration, actions are selected uniformly at random.

Therefore:

$$
P(\text{greedy action during exploration})
=
\frac{1}{2}
$$


- Step 3: Compute Total Probability

The greedy action can be selected in two ways:

- Case 1: Exploitation

The greedy action is always selected.

$$
P = 0.5
$$

- Case 2: Exploration

The greedy action is selected randomly with probability:

$$
0.5 \times \frac{1}{2} = 0.25
$$


- Step 4: Add the Probabilities

$$
P(\text{greedy action})
=
0.5 + 0.25
$$

$$
P(\text{greedy action})
=
0.75
$$

- Answer

$$
\boxed{P(\text{greedy action}) = 0.75}
$$

The greedy action is selected **75% of the time**.

---

# The 10-armed Testbed


