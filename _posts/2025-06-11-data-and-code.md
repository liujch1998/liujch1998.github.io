---
layout: post
title: "Treating Data as Code: from linear algebra to agentic LLMs"
---

In both mathematics and computing, magic often begins when **data is treated as code**—when something passive and inert becomes something active and transformative.

### Layer 1: Linear Algebra — Data as Transformable

In linear algebra, a vector is just data—a set of values, a point in space. But when we apply a matrix to it, something changes. The matrix is a set of rules—**a linear transformation**—that converts this input vector into a new vector. Conceptually, the matrix acts like *code*, and the vector like *data*. One is active; the other is passive.

### Layer 2: Classical Computing — Code as Stored Data

In a computer, we store both data and instructions as binary. When we run a program, a CPU reads these instructions (code) and applies them to data, transforming it. Importantly, **code is just data until it’s interpreted**—a passive file becomes an active computation. But there's only one layer of interpretation: instructions are read once and executed.

### Layer 3: LLMs — Data That Generates Code

Now consider large language models (LLMs). When dormant, they are nothing more than data: a giant collection of weights. But when prompted, those weights become active—**interpreted by a forward pass through the network**—to generate new data: sequences of tokens.

In **agentic LLM frameworks**, we take this one step further. The output from the model—text—may itself be treated as **code**: commands to be executed, API calls to be invoked, or prompts to another model. This gives us **a second layer** of interpretation:

1. The model weights (data) are interpreted as a program to generate text.
2. The text (data) is then interpreted as executable code.

This resembles a stack of metaprogramming: data → code → data → code.

### Recursive Agency: More Layers?

This raises interesting questions:

* **Do we gain more expressive or computational power by stacking layers of interpretation?**
  Could recursive agentic frameworks—where LLMs call themselves or others based on their own outputs—yield qualitatively new behaviors, like open-ended self-improvement or emergent planning?

* **What are the risks of deeper layers of agency?**
  Each layer introduces ambiguity and potential failure: misinterpretation, prompt injection, hallucination. With more layers, failure modes compound. Worse, agency may blur: Who’s really in control when data generates code that generates data that generates code?

****

### Note

This blog post is expanded by ChatGPT, using the following prompt:

```
I want to write a short blog post. Below are some crude high-level ideas. I want to make an analogy between agentic LLMs and computer programs / linear algebra. Can you organize them into a blog post?

the magic begins when data is treated as code
1. in linear algebra, you multiply a matrix and a vector to get a new vector. the vector is the data, the matrix is the code (that's why it's also called a "linear transformation")
2. in computers, you execute a series of instructions to transform some data into some new data. the instructions are stored as "data" when not active, but is interpreted as code when being executed.
3. for LLMs, the model is data when dormant; the model (and the scaffolding code) transforms some data into some new data. in an agentic framework with LLMs, these new data are sometimes treated as code and get executed, and now there are two layers: the model weights gets interpreted as code which generates data, and then these data gets interpreted as code which gets executed. This is one more layer than computer programs.

now some questions arise:
1. Do we fundamentally get more from by having two layers of re-interpreting data as code? What happens if we have more layers, or (by making this process recursive) have effectively infinite layers?
2. What are the risks associated with having more layers?
```
