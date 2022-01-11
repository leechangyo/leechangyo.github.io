---
layout: post
title: heuristic vs Metaheuristics
category: calculus
tag: calculus
---

As already written by some contributors of this thread, heuristics are problem-dependent techniques. As such, they usually are adapted to the problem at hand and they try to take full advantage of the particularities of this problem. However, because they are often too greedy, they usually get trapped in a local optimum and thus fail, in general, to obtain the global optimum solution.
Meta-heuristics, on the other hand, are problem-independent techniques. As such, they do not take advantage of any specificity of the problem and, therefore, can be used as black boxes. In general, they are not greedy. In fact, they may even accept a temporary deterioration of the solution (see for example, the simulated-annealing technique), which allows them to explore more thoroughly the solution space and thus to get a hopefully better solution (that sometimes will coincide with the global optimum). Please note that although a meta-heuristic is a problem-independent technique, it is nonetheless necessary to do some fine-tuning of its intrinsic parameters in order to adapt the technique to the problem at hand.
