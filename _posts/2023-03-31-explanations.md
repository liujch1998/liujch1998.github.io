---
layout: post
title: "Reflections on Commonsense Explanations"
---

To tackle the task of commonsense question answering, numerous work have proposed to ground the reasoning into explanations or relevant commonsense knowledge ([Liu et al., 2021](https://arxiv.org/pdf/2110.08387.pdf); [Liu et al. 2022](https://arxiv.org/pdf/2210.03078.pdf); [Wang et al., 2022](https://arxiv.org/pdf/2209.01232.pdf); inter alia). In this blog post, I reflect on whether these approaches are really logically sound and bullet-proof.

We take a hypothesis-verification formulation of commonsense problems. Given a hypothesis $H$, we want to determine if it is True or False. In the explanation-grounded approaches, a piece of explanation $E$ would be first retrieved or generated, and then a model makes a prediction on $H$ based on the correctness of $E$ and whether it supports or refutes $H$. If $E$ is correct (i.e. $E = 1$) and supports $H$ (i.e. $H \mid E = 1$), then we say $H$ is likely to be correct (i.e. $H = 1$). If such $E$ cannot be found, and what we have is either $E = 0 \text{ and } H \mid E = 1$, or $E = 1 \text{ and } H \mid E = 0$, then we say $H$ is likely to be incorrect (i.e. $H = 0$).

## What can be clearly defined, and what cannot?

But how do we exactly define "support" and "refute"? In NLI tasks (where people say "entail" and "contradict"), when we say a premise $P$ entails a conclusion $C$ (i.e. $C \mid P = 1$), we mean if we assume $P$ is True, then $C$ must be True. For example: (Adapted from [Liu et al., 2022](https://arxiv.org/pdf/2201.05955.pdf))

$$
P_1 = \text{5 percent probability that each part will be defect free.} \\
C_1 = \text{Each part has a 95 percent chance of having a defect.}
$$

It is clear that $P_1$ entails $C_1$. However, for conclusions that have grounds in commonsense, things become a bit tricky. Consider this example: (Adapted from [Tafjord et al., 2022](https://arxiv.org/pdf/2210.12217.pdf))

$$
P_2 = \text{Pennies are made of copper. Copper is not magnetic.} \\
C_2 = \text{A magnet cannot pick up a penny.}
$$

Most people would agree that $P_2$ supports $C_2$. But what if we remove one sentence from $P$?

$$
P_2' = \text{Pennies are made of copper.} \\
C_2 = \text{A magnet cannot pick up a penny.}
$$

Does $P_2'$ support $C_2$? Some may argue it does not, because we removed an important piece of information and now $P_2'$ is not a complete reasoning chain. But wait a second, is $P_2$ a complete reasoning chain? I can argue that it is also missing some information, which is completed in the following line:

$$
P_2'' = \text{Pennies are made of copper. Copper is not magnetic.} \\
\text{A magnet cannot pick up a non-magnetic item.} \\
C_2 = \text{A magnet cannot pick up a penny.}
$$

Now $P_2'' \rightarrow C_2$ is a strict deduction. But commonsense reasoning is far beyond strict deductions, and other reasoning processes like induction, abduction, analogy comes into play, and fuzziness is in its nature. For example, here is an example for analogous reasoning: (Adapted from [Liu et al., 2021](https://arxiv.org/pdf/2110.08387.pdf))

$$
P_3 = \text{Boats are used for transportation.} \\
C_3 = \text{Bicycles are used for transportation. Bicycles and boats serve for similar purposes.}
$$

An example with fuzziness is

$$
P_4 = \text{On university campuses, auditoriums are often used for lectures.} \\
\text{In university lectures, usually only a single person is speaking.} \\
C_4 = \text{On university campuses there would be an auditorium with only a single person speaking.}
$$

Therefore, in commonsense explanations, it is usually possible to argue that the supportive explanation is incomplete and has missing information. When we say a premise $P$ supports a conclusion $C$, we probably mean that, *if we assume that $P$ is True, and we know the rest of the commonsense knowledge of the world, then $C$ shall be True*. However, if we take things to the extreme and remove everything from the premise, under this definition an empty premise should also "support" the correct hypothesis, right? Viewed this way, the boundary between "support" and "not support" is very hard to define when the hypothesis is correct.

Why is this the case? [TODO]

In fact, sometimes NLI also needs to draw from extra commonsense knowledge, e.g. (Adapted from [Liu et al., 2022](https://arxiv.org/pdf/2201.05955.pdf))

$$
P_5 = \text{Salinger wrote similar letters to other young female writers.} \\
C_5 = \text{Other young female writers received similar letters from
Salinger as well.}
$$

where the implicit commonsense knowledge is, *If A writes a letter to B, then B would receive the letter from A*.

Meanwhile, what is a clear cut is when an explanation **refutes** a correct hypothesis:

$$
P_6 = \text{Copper is magnetic.} \\
C_6 = \text{A magnet cannot pick up a penny.}
$$

Together with other commonsense knowledge (e.g. *Pennies are made of Copper. A magnet cannot pick up a non-magnetic item.*), it is clear that $P_6$ refutes $C_6$ (despite that $P_6$ is wrong).

Another clear cut can be made when an explanation **supports** an incorrect hypothesis:

$$
P_7 = \text{Copper is magnetic.} \\
C_7 = \text{A magnet can pick up a penny.}
$$

Again, together with other commonsense knowledge (e.g. *Pennies are made of Copper. A magnet can pick up a magnetic item.*), it is clear that $P_7$ refutes $C_7$ (despite that they are both wrong).

Finally, another hard-to-define thing is when an explanation **refutes** an incorrect hypothesis:

$$
P_8 = \text{Copper is not magnetic.} \\
C_8 = \text{A magnet can pick up a penny.}
$$

While $P_8$ implies that *a magnet cannot pick up a penny through magnetism*, it is conceivable that it may do so through other mechanisms, and the explanation fails to rule out these possibilities.

To summarize, if a conclusion $C$ is correct, then it is usually unclear when an explanation supports it, but it is clear when an explanation refutes it. Conversely, if $C$ is incorrect, then it is usually unclear when an explanation refutes it, but it is clear when an explanation supports it. Put in logical terms, the limit of the capability of any relation model (i.e. model that classifies $C \mid P$) is

$$
C = 1 \rightarrow C \mid P \ne 1 \\
C = 0 \rightarrow C \mid P \ne 0
$$

## The Conventional Way of Explanation-grounded Reasoning

How have we been doing explanation-grounded reasoning? Say we're given a hypothesis $H$ to test, and our method produces an explanation $E$. We use $H$ as the conclusion. In order to show that $H$ is correct (i.e. $H = 1$), we would need to find $E$ such that $E = 1$ and $H \mid E = 1$. But this violates our rule for relation models: $C = 1 \rightarrow C \mid P \ne 1$. So our conventional way of doing explanation-grounded reasoning is flawed. We can also try finding $E$ such that $E = 0$ and $H \mid E = 0$, but this does not guarantee $H = 1$. For example, (Adapted from [Liu et al., 2021](https://arxiv.org/pdf/2110.08387.pdf))

$$
E = \text{Penguins are mammals.} \\
H = \text{Penguins have three wings.} \\
\text{In this case, } E = 0, H \mid E = 0, H = 0
$$

On the other hand, to show that $H$ is incorrect (i.e. $H = 0$), we would need to find $E$ such that $E = 1$ and $H \mid E = 0$. But again this violates our rule for relation models: $C = 0 \rightarrow C \mid P \ne 0$. We can also try finding $E$ such that $E = 1$ and $H \mid E = 0$, but this does not guarantee $H = 0$. For example,

$$
E = \text{Penguins are mammals.} \\
H = \text{Socrates is mortal.} \\
\text{In this case, } E = 0, H \mid E = 1, H = 1
$$

## Why are most existing explanation-grounded methods still okay?

Because instead of the hypothesis-verification formulation, they take a QA formulation of commonsense problems. This only requires (implicitly) comparing $p(H \mid E)$ for different $H$'s, and does not require giving an actual, absolute value of $p(H \mid E)$.

These work can be roughly categorized as following:

1. Post-hoc or joint generation of explanation, which is not part of the final decision-making process. ([Marasovic et al., 2021](https://arxiv.org/pdf/2111.08284.pdf); [Wiegreffe et al., 2021](https://arxiv.org/pdf/2112.08674.pdf); [Chen et al., 2022](https://arxiv.org/pdf/2210.04982.pdf))

1. Frozen knowledge generation model, frozen QA model. ([Bosselut et al., 2019](https://arxiv.org/pdf/1911.03876.pdf); [Shwartz et al., 2020](https://arxiv.org/pdf/2004.05483.pdf); [Paranjape et al., 2021](https://arxiv.org/pdf/2106.06823.pdf); [Liu et al., 2021](https://arxiv.org/pdf/2110.08387.pdf); [Betz et al., 2021](https://arxiv.org/pdf/2103.13033.pdf); [Yu et al., 2022](https://arxiv.org/pdf/2209.10063.pdf))

1. Trained knowledge generation model, frozen QA model. ([Liu et al. 2022](https://arxiv.org/pdf/2210.03078.pdf); [Gu et al., 2022](https://arxiv.org/pdf/2112.08656.pdf))

1. Frozen knowledge generation model, trained QA model. ([Talmor et al., 2020](https://arxiv.org/pdf/2006.06609.pdf))

1. Trained knowledge generation and QA model. ([Rajani et al., 2019](https://arxiv.org/pdf/1906.02361.pdf); [Latcinnik and Berant, 2020](https://arxiv.org/pdf/2004.05569.pdf); [Wang et al., 2022](https://arxiv.org/pdf/2209.01232.pdf); [Wang et al., 2022](https://arxiv.org/pdf/2211.01562.pdf))

Meanwhile, methods that take a hypothesis-verification formulation (e.g. [Jung et al., 2022](https://arxiv.org/pdf/2205.11822.pdf); [Tafjord et al., 2022](https://arxiv.org/pdf/2210.12217.pdf)) may be more likely suffer from the problem we discussed above.

## So what can we do about hypothesis-verification?

We revisit the rules for relation models. If $C$ is correct, then $P$ can be either refuting or not refuting $C$. In this case, if $P$ is refuting $C$, then we can be pretty sure that $P$ is incorrect. On the other hand, if $C$ is incorrect, then $P$ can be either supporting or not supporting $C$. In this case, if $P$ is supporting $C$, then we can also be pretty sure that $P$ is incorrect. Put formally,

$$
C = 1 \text{ and } C \mid P = 0 \rightarrow P = 0 \\
C = 0 \text{ and } C \mid P = 1 \rightarrow P = 0
$$

Then if we want to test a hypothesis $H$, we can put it as a premise and try to prove that it is incorrect! Formally, we can try to find an explanation $E$ such that

$$
E = 1 \text{ and } E \mid H = 0 \rightarrow H = 0 \\
E = 0 \text{ and } E \mid H = 1 \rightarrow H = 0
$$

If we can find such an $E$, then $H = 0$. If we try hard but still cannot find such an $E$, then we say we do not have sufficient evidence to reject $H$, and thus $H$ is seen to be correct.

