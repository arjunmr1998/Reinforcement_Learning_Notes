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
## Test Setup
- **2000** randomly generated **10-armed bandit** problems.
- **k = 10** actions.
- True action values:
  \[
  q^*(a) \sim \mathcal{N}(0,1)
  \]
- Reward when action \(A_t\) is selected:
  \[
  R_t \sim \mathcal{N}(q^*(A_t),1)
  \]
- Each experiment:
  - **1000 time steps**
  - Averaged over **2000 runs**
- Action-value estimates updated using **Sample Average Method**.

---

# Methods Compared
- **Greedy (\(\epsilon=0\))**
- **\(\epsilon\)-Greedy (\(\epsilon=0.01\))**
- **\(\epsilon\)-Greedy (\(\epsilon=0.1\))**

---

# Results

### Greedy
✅ Learns quickly initially.

❌ Major problem:
- Can get stuck with a **suboptimal action**.
- If the first few rewards from the optimal action are unlucky, it may **never explore it again**.
- Found the optimal action in only **~1/3 of tasks**.

---

### ε-Greedy

Continues exploring.

Advantages:
- Eventually identifies the optimal action.
- Better long-term average reward.

#### ε = 0.1
- Explores more.
- Finds optimal action faster.
- But keeps exploring forever.
- Selects optimal action only about **91%** of the time.

#### ε = 0.01
- Explores less.
- Learns more slowly.
- Better long-term performance than ε = 0.1.

---

# Choosing ε

Large ε
- More exploration
- Faster discovery
- Lower final reward (because exploration never stops)

Small ε
- Less exploration
- Slower learning
- Better final performance

👉 Common idea:
- **Decay ε over time**
  - High initially
  - Small later

---

# Effect of Reward Variance

### High variance (noisy rewards)
- Need more exploration.
- ε-greedy performs much better than greedy.

### Zero variance (deterministic rewards)
- One sample reveals the true value.
- Greedy may perform best after trying each action once.

---

# Nonstationary Problems

If action values change over time:
- Exploration is always necessary.
- Even deterministic environments require exploration.
- Reinforcement Learning commonly deals with **nonstationary** settings.

---

# Key Takeaways

- **Greedy = Exploitation only**
- **ε-Greedy = Exploration + Exploitation**
- Exploration avoids getting trapped in poor actions.
- Small ε → Better long-term reward.
- Large ε → Faster learning.
- Decaying ε often gives the best balance.

# Incremental Implementation of Action-Value Estimation (Quick Revision)

## Problem with Sample Average

Estimated action value after selecting an action $n-1$ times:

$$
Q_n=\frac{R_1+R_2+\cdots+R_{n-1}}{n-1}
$$

### Drawback
- Need to store **all previous rewards**.
- Memory increases over time.
- Computation increases because the average must be recomputed every time.

---

# Incremental Update

Instead of storing every reward, update the estimate using only:
- Current estimate $Q_n$
- New reward $R_n$

Update equation:

$$
Q_{n+1}=Q_n+\frac{1}{n}(R_n-Q_n)
$$

This is called the **Incremental Sample Average Update**.

---

# Interpretation

$$
Q_{n+1}=Q_n+\frac{1}{n}(R_n-Q_n)
$$

where

- $Q_n$ = Old estimate
- $R_n$ = New reward (**Target**)
- $R_n-Q_n$ = Prediction (Estimation) Error
- $\frac{1}{n}$ = Step size

---

# General Reinforcement Learning Update Rule

Almost every RL algorithm follows this form:

$$
\boxed{
\text{New Estimate}
=
\text{Old Estimate}
+
\alpha(\text{Target}-\text{Old Estimate})
}
$$

where

- **Old Estimate** → Current value estimate
- **Target** → Desired value
- **Target − Old Estimate** → Error
- $\alpha$ (alpha) → Learning rate (step size)

---

# Step Size (Learning Rate)

For the sample-average method,

$$
\alpha=\frac{1}{n}
$$

Characteristics:
- Large initially
- Decreases as more samples are collected
- Eventually becomes very small

---

# Why Incremental Method?

✅ Constant memory: $O(1)$

✅ Constant computation: $O(1)$

✅ No need to store previous rewards

---

# ε-Greedy Bandit Algorithm

Repeat:

1. Choose action using **ε-greedy**
   - Probability $\epsilon$ → Explore (random action)
   - Probability $1-\epsilon$ → Exploit (best estimated action)

2. Receive reward $R$

3. Update estimate:

$$
Q(a)\leftarrow Q(a)+\frac{1}{N(a)}(R-Q(a))
$$

where

- $N(a)$ = Number of times action $a$ has been selected.

---

# Key Formulae to Memorize

### Sample Average

$$
Q_n=\frac{\sum_{i=1}^{n}R_i}{n}
$$

### Incremental Update

$$
Q_{n+1}=Q_n+\frac{1}{n}(R_n-Q_n)
$$

### General RL Update

$$
\boxed{
\text{New}
=
\text{Old}
+
\alpha(\text{Target}-\text{Old})
}
$$

---

# Intuition

Every new reward **nudges** the estimate toward the true value.

- Large error → Bigger update.
- Small error → Smaller update.
- As more samples are collected, updates become smaller because

$$
\alpha=\frac{1}{n}.
$$

---
# Nonstationary Bandits & Constant Step Size 

## Stationary vs Nonstationary

### Stationary Bandit
- True action values **do not change** over time.
- Sample-average method works well.

### Nonstationary Bandit
- True action values **change over time**.
- Recent rewards are more informative than old rewards.
- Use a **constant step size** instead of sample average.

---

# Constant Step-Size Update Rule

Instead of

$$
Q_{n+1}=Q_n+\frac{1}{n}(R_n-Q_n),
$$

use

$$
Q_{n+1}=Q_n+\alpha(R_n-Q_n),
$$

where

$$
0<\alpha\le1.
$$

- $\alpha$ is **constant**.
- Gives more importance to **recent rewards**.

---

# Expanded Form

Expanding recursively,

$$
Q_{n+1}
=
(1-\alpha)^nQ_1
+
\sum_{i=1}^{n}
\alpha(1-\alpha)^{n-i}R_i.
$$

This shows that:

- Initial estimate $Q_1$ still contributes.
- Every previous reward contributes.
- **Recent rewards receive larger weights.**

---

# Exponential Recency-Weighted Average

Weight assigned to reward $R_i$:

$$
\boxed{
\alpha(1-\alpha)^{\,n-i}
}
$$

Properties:

- Most recent reward → Largest weight
- Older rewards → Smaller weight
- Weights decrease **exponentially**

Hence,

> **Constant step-size update is called an Exponential Recency-Weighted Average.**

---

# Effect of α

### Large α

- Learns quickly
- Adapts rapidly to changes
- Higher variance (more noisy)

### Small α

- Learns slowly
- Smoother estimates
- Less responsive to changes

---

# Variable Step Size

Instead of a constant $\alpha$, use

$$
\alpha_n(a)
$$

which changes after every selection of action $a$.

Example:

$$
\alpha_n(a)=\frac{1}{n}
$$

This is the **Sample Average Method**.

---

# Convergence Conditions

For estimates to converge (with probability 1),

$$
\sum_{n=1}^{\infty}\alpha_n(a)=\infty
$$

and

$$
\sum_{n=1}^{\infty}\alpha_n^2(a)<\infty.
$$

---

# Meaning of the Conditions

### First Condition

$$
\sum \alpha_n=\infty
$$

- Steps never become **too small**.
- Eventually overcomes poor initialization and random fluctuations.

---

### Second Condition

$$
\sum \alpha_n^2<\infty
$$

- Steps eventually become **small enough**.
- Prevents oscillations.
- Ensures convergence.

---

# Sample Average vs Constant α

| Sample Average ($\alpha=\frac{1}{n}$) | Constant Step Size ($\alpha$) |
|----------------------------------------|-------------------------------|
| Stationary environments | Nonstationary environments |
| Equal weight to all rewards | More weight to recent rewards |
| Converges to true value | Does not fully converge |
| Slow adaptation | Fast adaptation |
| Meets convergence conditions | Does **not** satisfy second convergence condition |

---

# Why Doesn't Constant α Converge?

Since

$$
\alpha_n=\alpha,
$$

we get

$$
\sum \alpha^2=\infty.
$$

Therefore,

- Estimates never settle to a fixed value.
- They continue adapting to new rewards.

This is **desirable** when the environment keeps changing.

---

# Key Formulae to Memorize

### Sample Average

$$
Q_{n+1}
=
Q_n+\frac{1}{n}(R_n-Q_n)
$$

### Constant Step Size

$$
Q_{n+1}
=
Q_n+\alpha(R_n-Q_n)
$$

### Expanded Form

$$
Q_{n+1}
=
(1-\alpha)^nQ_1
+
\sum_{i=1}^{n}
\alpha(1-\alpha)^{n-i}R_i
$$

### Convergence Conditions

$$
\sum\alpha_n=\infty
$$

$$
\sum\alpha_n^2<\infty
$$

---

# Intuition

- **Sample Average** remembers the **entire history** equally.
- **Constant $\alpha$** gradually **forgets the past**.
- Forgetting is useful when the environment changes over time.

---

# The Problem with Initial Conditions

Some exploration methods encourage exploration **only at the beginning** of learning. Two common examples are:

* **Optimistic Initial Values**
* **Sample-Average Estimates**

These methods treat the **beginning of learning** as a special event.

---

## Optimistic Initial Values

The agent starts with artificially high estimates of action values.

Example:

```text
True Action Values

Action A : 5
Action B : 8
Action C : 3

Initial Estimates

Q(A) = 100
Q(B) = 100
Q(C) = 100
```

Since all actions initially appear highly rewarding, the agent naturally explores each one.

However, after sufficient interactions, these optimistic estimates are replaced by actual reward estimates, and the exploration incentive disappears.

---

## Sample-Average Method

The sample-average update rule is

$$
Q_{n+1} = Q_n + \frac{1}{n}(R_n - Q_n)
$$

Every observed reward contributes **equally** to the final estimate.

As more rewards are collected:

* The learning rate decreases.
* The estimate becomes increasingly stable.
* The algorithm becomes less responsive to new information.

---

## Why Are These Methods a Problem?

Both methods rely heavily on the **initial phase of learning**.

However,

> **The beginning of time occurs only once.**

Once the initial exploration is over:

* Optimistic values disappear.
* Sample-average updates become very small.
* The agent stops adapting quickly.

---

## What Happens in a Nonstationary Environment?

Suppose the environment changes after a long period.

Example:

| Time        | Best Action |
| ----------- | ----------- |
| 1–1000      | Action A    |
| 1001 onward | Action B    |

### Optimistic Initial Values

* Optimism existed only at the start.
* No new incentive to explore Action B.
* The agent may continue choosing Action A.

### Sample-Average Method

Since

$$
\alpha_n = \frac{1}{n},
$$

the learning rate becomes extremely small after many updates.

For example,

$$
n = 1000
\quad\Rightarrow\quad
\alpha = 0.001.
$$

New rewards have very little influence, making adaptation to the changed environment very slow.

---

## Better Approach for Nonstationary Problems

For changing environments, a **constant step-size** is preferred:

$$
Q_{n+1} = Q_n + \alpha(R_n - Q_n),
$$

where

$$
\alpha = 0.1 \quad \text{(or another constant)}.
$$

This gives more weight to recent rewards, allowing the agent to adapt when the environment changes.

---

## Key Takeaways

* Optimistic Initial Values encourage exploration **only at the start**.
* Sample-Average methods also give special importance to the beginning of learning.
* Neither method automatically renews exploration when the environment changes.
* In **nonstationary environments**, constant learning rates are preferred because they continually adapt to recent observations.
* Most practical reinforcement learning algorithms therefore use **constant or adaptive step sizes** instead of the sample-average method.

# Upper Confidence Bound (UCB) Action Selection

## Motivation

The **ε-greedy** algorithm explores by choosing a random action with probability ε.

Problem:

* Every non-greedy action is treated equally.
* Good actions and bad actions have the same exploration probability.
* Exploration is inefficient.

Instead, exploration should focus on actions that are **uncertain** and **may actually be optimal**.

---

# UCB Action Selection

The UCB action-selection rule is

$$
A_t
===

\arg\max_a
\left[
Q_t(a)
+
c
\sqrt{\frac{\ln t}{N_t(a)}}
\right]
$$

where

* $Q_t(a)$ = estimated reward of action $a$
* $N_t(a)$ = number of times action $a$ has been selected
* $t$ = current time step
* $c>0$ = exploration parameter

If

$$
N_t(a)=0,
$$

then action $a$ is selected immediately because it has never been explored.

---

# Components of the Equation

## Exploitation Term

$$
Q_t(a)
$$

Represents the current estimate of the action value.

Higher estimated reward makes an action more attractive.

---

## Exploration Bonus

$$
c
\sqrt{\frac{\ln t}{N_t(a)}}
$$

Measures the uncertainty of an action.

* Large bonus → highly uncertain action
* Small bonus → well-explored action

---

# Role of Each Variable

## Number of Selections

$$
N_t(a)
$$

Appears in the denominator.

* Large $N_t(a)$ → smaller exploration bonus.
* Small $N_t(a)$ → larger exploration bonus.

Thus, frequently selected actions become less attractive for exploration.

---

## Current Time

$$
\ln t
$$

Appears in the numerator.

As time increases,

* exploration bonus increases slowly,
* preventing actions from being ignored forever.

The logarithm grows very slowly, so exploration naturally decreases over time.

---

## Exploration Constant

$$
c>0
$$

Controls the balance between exploration and exploitation.

* Small $c$ → mostly greedy behavior.
* Large $c$ → more exploration.

---

# Example

Suppose

$$
t=100,
\qquad
c=2.
$$

| Action | $Q_t(a)$ | $N_t(a)$ |
| ------ | -------- | -------- |
| A      | 9        | 100      |
| B      | 8        | 10       |
| C      | 7        | 2        |

The UCB scores are

| Action | UCB Score |
| ------ | --------- |
| A      | 9.43      |
| B      | 9.36      |
| C      | 10.04     |

Although Action C has the lowest estimated reward, it is selected because its uncertainty is much larger.

---

# Why UCB Works

Whenever an action is selected:

* $N_t(a)$ increases.
* The exploration bonus decreases.
* The algorithm becomes more confident about that action.

Whenever other actions are selected:

* $t$ increases.
* $N_t(a)$ remains unchanged.
* The exploration bonus slowly increases.

Therefore, every action is explored eventually, but poor actions are explored less frequently over time.

---

# UCB vs ε-Greedy

| ε-Greedy                              | UCB                                    |
| ------------------------------------- | -------------------------------------- |
| Random exploration                    | Directed exploration                   |
| All non-greedy actions equally likely | Uncertain actions preferred            |
| Can waste exploration on poor actions | Focuses on potentially optimal actions |
| Simple                                | More efficient in stationary bandits   |

---

# Limitations of UCB

## Nonstationary Environments

As

$$
N_t(a)
$$

keeps increasing,

the exploration bonus approaches zero.

The algorithm becomes less responsive to changing reward distributions.

---

## Large State Spaces

In reinforcement learning,

every state has different actions.

Maintaining

$$
N_t(s,a)
$$

for every state-action pair becomes computationally expensive.

---

## Function Approximation

When using neural networks,

there is no explicit count for every action.

Therefore, the standard UCB formula cannot be applied directly.

---

# Key Takeaways

* UCB balances **exploitation** and **exploration** using an exploration bonus.
* The bonus decreases as an action is explored more often.
* The logarithmic term ensures every action is eventually revisited.
* UCB explores intelligently instead of randomly.
* It performs very well for **stationary multi-armed bandit problems**, but is less practical for large-scale or deep reinforcement learning.

# Action Preferences and Softmax Policy

## Key Idea

Instead of estimating the value of each action (`Q(a)`), we learn a **preference** for each action.

The preference is denoted by:

\[
H(a)
\]

Unlike action values, preferences **do not estimate expected rewards**.

They only determine **how likely an action is to be selected**.

---

## Action Values vs Preferences

| Action Values | Action Preferences |
|---------------|--------------------|
| Estimate expected reward | Represent relative desirability |
| Used in Q-learning | Used in policy gradient methods |
| Numerical meaning | No absolute meaning |
| Greedy action selection | Probabilistic action selection |

---

## Softmax Policy

Action probabilities are computed using the Softmax function:

\[
\pi(a)=\frac{e^{H(a)}}{\sum_b e^{H(b)}}
\]

where:

- `H(a)` = preference of action `a`
- `π(a)` = probability of selecting action `a`

---

## Why Softmax?

Softmax:

- Converts any real numbers into probabilities.
- Ensures probabilities are positive.
- Ensures probabilities sum to 1.
- Assigns higher probabilities to actions with larger preferences.

---

## Example

Preferences:

| Action | H(a) |
|--------|------|
| A | 2 |
| B | 1 |
| C | 0 |

Exponentials:

| Action | exp(H) |
|--------|---------|
| A | 7.39 |
| B | 2.72 |
| C | 1.00 |

Total:

7.39 + 2.72 + 1 = 11.11

Probabilities:

| Action | Probability |
|--------|-------------|
| A | 0.665 |
| B | 0.245 |
| C | 0.090 |

---

## Important Property

Adding the same constant to every preference does **not** change the probabilities.

Example:

Original:

```
A = 1
B = 2
C = 3
```

Shifted:

```
A = 1001
B = 1002
C = 1003
```

The resulting probabilities are identical because the common exponential factor cancels out.

---

## Initial Preferences

Usually,

```
H(a) = 0
```

for every action.

Softmax gives:

```
π(a) = 1 / number_of_actions
```

Every action is equally likely initially.

---

## Softmax vs ε-Greedy

### ε-Greedy

- Best action selected most of the time.
- Remaining actions are selected uniformly at random.

### Softmax

- Every action has a probability based on its preference.
- Better actions are chosen more often.
- Exploration is guided rather than uniform.

---

## Why This Matters

Action preferences form the basis of **policy-based reinforcement learning**.

Instead of learning:

```
Q(s,a)
```

we learn:

```
π(a|s)
```

This idea is the foundation of modern Deep RL algorithms such as:

- REINFORCE
- Actor-Critic
- PPO
- A2C
- A3C
- Soft Actor-Critic (SAC)

---

## Key Takeaways

- Learn **preferences**, not action values.
- Preferences have **no reward interpretation**.
- Convert preferences to probabilities using **Softmax**.
- Only **relative differences** in preferences matter.
- Softmax naturally balances exploration and exploitation.
- This is the first step toward **policy gradient methods**.

# Gradient Bandit Algorithm as Stochastic Gradient Ascent

---

# Big Picture

The Gradient Bandit Algorithm was previously introduced with the update rules:

For the **selected action**:

\[
H_{t+1}(a)=H_t(a)+\alpha(R_t-\bar{R}_t)(1-\pi_t(a))
\]

For **all other actions**:

\[
H_{t+1}(a)=H_t(a)-\alpha(R_t-\bar{R}_t)\pi_t(a)
\]

A natural question arises:

> **Why do these update rules work?**

Are they simply heuristics?

**No.**

Sutton proves that these updates are actually performing **Stochastic Gradient Ascent** on the expected reward.

---

# Goal of the Algorithm

The objective is to maximize the expected reward:

\[
J(H)=E[R_t]
\]

where:

- \(H\) = action preferences
- \(J(H)\) = performance objective

Relationship:

```text
Action Preferences H
          ↓
      Softmax Policy π
          ↓
     Selected Action
          ↓
         Reward
```

Changing the preferences changes the policy, which changes the expected reward.

---

# What is Gradient Ascent?

Gradient ascent is an optimization technique used to maximize a function.

General update rule:

\[
x_{new}=x+\alpha\nabla f(x)
\]

where:

- \(f(x)\) = objective function
- \(\nabla f(x)\) = gradient
- \(\alpha\) = learning rate

The gradient always points toward the direction of maximum increase.

---

# Applying Gradient Ascent to Reinforcement Learning

Our objective is:

\[
J(H)=E[R_t]
\]

Therefore the ideal update becomes

\[
H_{t+1}(a)
=
H_t(a)
+
\alpha
\frac{\partial E[R_t]}
{\partial H_t(a)}
\]

This is Equation (2.13) in Sutton.

---

# The Challenge

To compute

\[
\frac{\partial E[R]}{\partial H}
\]

we require the true action values

\[
q^*(a)
\]

However,

\(q^*(a)\) is **unknown**.

If we already knew the true action values, there would be nothing left to learn.

---

# Stochastic Gradient Ascent

Instead of computing the exact gradient,

we estimate it from samples.

Instead of

```text
Exact Gradient
```

we use

```text
Sample Gradient
```

This is exactly the same principle used when training neural networks with mini-batches.

---

# Step 1: Expected Reward

Expected reward is

\[
E[R]
=
\sum_x
\pi(x)q^*(x)
\]

where

- \(\pi(x)\) = probability of selecting action \(x\)
- \(q^*(x)\) = true expected reward of action \(x\)

This is simply the definition of expectation.

Example

| Action | Probability | Reward |
|---------|------------|--------|
| A | 0.5 | 10 |
| B | 0.3 | 5 |
| C | 0.2 | 1 |

Expected reward

\[
0.5(10)+0.3(5)+0.2(1)=6.7
\]

---

# Step 2: Differentiate the Objective

Take the derivative with respect to the action preference.

\[
\frac{\partial E[R]}
{\partial H(a)}
=
\sum_x
q^*(x)
\frac{\partial\pi(x)}
{\partial H(a)}
\]

Only the policy depends on the preferences.

The true rewards remain constant.

This is simply an application of the chain rule.

---

# Step 3: Introducing the Baseline

Sutton replaces

\[
q^*(x)
\]

with

\[
q^*(x)-B
\]

where \(B\) is called the **baseline**.

Why is this allowed?

Because

\[
\sum_x
\frac{\partial\pi(x)}
{\partial H(a)}
=
0
\]

The probabilities always sum to one.

If one probability increases,

another must decrease.

Therefore

adding or subtracting a constant does not change the gradient.

---

# Why Use a Baseline?

Suppose the rewards are

```text
1001
1003
998
1002
```

Instead of learning from

```text
1003
```

we learn from

```text
1003 - 1001 = 2
```

or

```text
998 - 1001 = -3
```

The algorithm now answers the question:

> "Was this reward better or worse than average?"

instead of using the raw reward.

Benefits:

- Lower variance
- More stable learning
- Faster convergence

---

# Step 4: Multiplying by π/π

Sutton multiplies by

\[
\frac{\pi(x)}{\pi(x)}
\]

This equals one, so nothing changes mathematically.

The reason is to rewrite the expression as an expectation.

Recall

\[
\sum_x
\pi(x)f(x)
=
E[f(x)]
\]

Now the gradient becomes an expectation,

which can be estimated from samples.

This is the key mathematical trick.

---

# Step 5: Replace q*(A) with Actual Reward

Sutton replaces

\[
q^*(A)
\]

with

\[
R_t
\]

Why?

Because

\[
E[R_t|A]
=
q^*(A)
\]

The actual reward is simply a noisy sample of the true expected reward.

Example

True reward

```text
10
```

Observed rewards

```text
9
11
10
12
8
```

Average

```text
10
```

Thus,

one reward sample is an unbiased estimate of the true value.

---

# Step 6: Derivative of Softmax

Sutton proves

\[
\frac{\partial\pi(x)}
{\partial H(a)}
=
\pi(x)
(\mathbf{1}_{a=x}-\pi(a))
\]

where

\[
\mathbf{1}_{a=x}
=
\begin{cases}
1,&a=x\\
0,&a\neq x
\end{cases}
\]

This is simply the derivative of the Softmax function.

Interpretation:

If the action is selected,

\[
1-\pi(a)
\]

is positive,

so its preference increases.

For every other action,

\[
-\pi(a)
\]

is negative,

so their preferences decrease.

---

# Final Update Rule

Substituting everything together gives the Gradient Bandit update.

### Selected Action

\[
H(a)
\leftarrow
H(a)
+
\alpha
(R-\bar R)
(1-\pi(a))
\]

---

### Non-selected Actions

\[
H(a)
\leftarrow
H(a)
-
\alpha
(R-\bar R)
\pi(a)
\]

These are exactly the update rules introduced earlier.

---

# Why This Proof Matters

This proof shows that the Gradient Bandit Algorithm is **not** an arbitrary heuristic.

Instead,

- We define an objective function \(J(H)=E[R]\).
- We compute its gradient.
- Since the exact gradient cannot be computed (because \(q^*\) is unknown),
  we estimate it using sampled rewards.
- The expected update equals the true gradient.

Therefore,

the algorithm performs **Stochastic Gradient Ascent**.

---

# Role of the Baseline

The baseline

\[
B=\bar R
\]

does **not** change the expected gradient.

It only changes the variance.

A good baseline:

- reduces variance,
- stabilizes learning,
- improves convergence speed.

Any constant independent of the selected action could be used.

Examples:

- 0
- 1000
- Running average reward (most common)

The running average reward is simple and works well in practice.

---

# Key Takeaways

- The objective is to maximize the expected reward.
- Action preferences are optimized using Gradient Ascent.
- The exact gradient is unavailable because \(q^*(a)\) is unknown.
- Sampled rewards provide an unbiased estimate of the true gradient.
- The Softmax derivative determines how preferences are updated.
- The baseline reduces variance but does not change the expected update.
- The Gradient Bandit Algorithm is an example of **Stochastic Gradient Ascent**.

---

# Connection to Deep Reinforcement Learning

This derivation is the foundation of modern policy optimization methods.

The same idea appears in:

- REINFORCE
- Actor-Critic
- A2C
- A3C
- PPO
- TRPO
- Soft Actor-Critic (SAC)

General workflow:

```text
Define Objective J(θ)

        ↓

Compute Gradient

        ↓

Estimate Gradient Using Samples

        ↓

Update Parameters

        ↓

Improve Policy
```

Almost every modern policy gradient algorithm follows this same principle.


# From Multi-Armed Bandits to Contextual Bandits and Reinforcement Learning

---

# Overview

This section explains the transition from **non-associative bandit problems** to **associative search tasks (Contextual Bandits)**, which serve as a bridge to full Reinforcement Learning.

---

# 1. Non-Associative Task (Multi-Armed Bandit)

In a standard k-armed bandit problem:

- There is only one situation.
- The learner repeatedly chooses among k actions.
- The objective is to find the action with the highest expected reward.

```
One Situation
      ↓
 Choose Action
      ↓
 Receive Reward
```

The environment never changes.

Example:

| Action | Reward |
|--------|--------|
| Arm 1 | ? |
| Arm 2 | ? |
| Arm 3 | ? |

The learner only needs to discover the best arm.

---

# 2. Why This is Limited

In real-world problems, the best action often depends on the current situation.

Example:

| Weather | Best Drink |
|----------|------------|
| Summer | Juice |
| Winter | Tea |
| Rain | Coffee |

The optimal action changes with the context.

---

# 3. Associative Search Task

Now multiple situations exist.

The learner must associate each situation with its best action.

```
Situation
     ↓
Choose Action
     ↓
Receive Reward
```

Instead of learning a single best action, the learner learns a **policy**.

---

# 4. Policy

A policy maps situations to actions.

Mathematically:

\[
\pi(s)=a
\]

where

- \(s\) = state (context)
- \(a\) = action

Example:

| Context | Best Action |
|----------|-------------|
| Red | Arm 1 |
| Green | Arm 2 |
| Blue | Arm 3 |

---

# 5. Sutton's Slot Machine Example

Suppose the slot machine changes its display color.

| Display Color | Best Arm |
|---------------|----------|
| Red | Arm 1 |
| Green | Arm 2 |
| Blue | Arm 5 |

If the learner ignores the color, rewards appear to change randomly and the problem looks highly non-stationary.

If the learner observes the color, it can learn a separate policy for each context.

---

# 6. Contextual Bandits

A contextual bandit follows this sequence:

```
Observe Context
        ↓
Choose Action
        ↓
Receive Reward
        ↓
Episode Ends
```

Characteristics:

- Multiple contexts.
- Immediate reward only.
- Actions do **not** affect future contexts.
- Each decision is independent.

Modern examples:

- News recommendation
- Advertisement selection
- Product recommendation
- Email subject line optimization

---

# 7. Full Reinforcement Learning

Full RL introduces **state transitions**.

```
State
   ↓
Action
   ↓
Reward
   ↓
Next State
   ↓
Action
   ↓
Reward
```

Here:

- Actions influence the next state.
- Future rewards depend on current decisions.
- Long-term planning becomes essential.

This framework is modeled using a **Markov Decision Process (MDP)**.

---

# Comparison

| Property | Multi-Armed Bandit | Contextual Bandit | Reinforcement Learning |
|-----------|--------------------|-------------------|-------------------------|
| Number of states | 1 | Many | Many |
| Learn policy | No | Yes | Yes |
| Immediate reward | Yes | Yes | Yes |
| State transitions | No | No | Yes |
| Action changes future | No | No | Yes |
| Long-term planning | No | No | Yes |

---

# Autonomous Driving Example

### Multi-Armed Bandit

Choose one controller.

↓

Receive reward.

---

### Contextual Bandit

State:

```
Highway
```

↓

Choose lane.

↓

Receive reward.

Decision ends.

---

### Reinforcement Learning

```
Highway
    ↓
Accelerate
    ↓
Merge
    ↓
Change Lane
    ↓
Exit
```

Each action changes the future state.

---

# Key Takeaways

- **Multi-Armed Bandit:** Learn one best action.
- **Contextual Bandit:** Learn the best action for each context.
- **Reinforcement Learning:** Learn the best sequence of actions because actions influence future states.
- A **policy** maps states to actions.
- Contextual Bandits are the bridge between Bandits and full Reinforcement Learning.



