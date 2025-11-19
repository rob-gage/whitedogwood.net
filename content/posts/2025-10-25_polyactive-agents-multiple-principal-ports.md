+++
title = "Polyactive Agents: Multiple Principal Ports"
date = 2025-10-25
description = "An explanation of a flaw in standard interaction nets that makes them less than optimal for parallel evaluation, and a variation of them that solves this flaw"

[taxonomies]
tags = ["Interaction Nets", "Programming Languages"]
+++

## Interaction Nets

[Interaction Nets](@posts/2025-7-26_interaction-nets.md) are an elegant computation language with properties that make them optimal for parallel evaluation. They allow computations to be written as graphs of interconnected agents, with defined rules specifying how these agents are rewritten when they are connected via their principal ports. These rewrites, or interactions, can be evaluated in any order to produce the same final result, and can be evaluated concurrently by different processors.

## The Problem With Standard Interaction Nets

In the original paper by Yves Lafont, agents must have one and only one principal port. This provides the very useful guarantee that interaction nets will be evaluated deterministically. While this architecture is very sound at an abstract level, when implemented for computer languages it becomes less than optimal for massively parallel program evaluation. Any operation with more than one operand implemented in an interaction nets system can only process one operand at a time. For example, a binary operation like Boolean **AND** takes two booleans as input, but it only needs to know that one of them is false to produce a result. In a standard interaction net system, if the first operand (connected to the principal port) is not immediately available, the operation cannot continue even if the second operand is sufficient to determine result.

## Polyactive Agents

When toying around with a runtime implementation for standard interaction nets, I quickly realized how imperfect they are for modeling dependencies in a computation that may not be satisfied in a predetermined order. The natural extension of the system, it seemed, was to allow agents to interact on one of many "active" ports, rather than having a single principal port. Before learning that these had already been conceived of as "multiport" agents, I termed them as "polyactive", which I strongly prefer and will continue to call them by to avoid confusion (since "multiport" could accurately describe standard agents with any amount of auxiliary ports). These polyactive agents allow operation inputs to be received in any order, and in some cases, reduce the number of necessary steps to solve a computation.

## Example

This example uses the same agent types in [my previous post about interaction nets](@posts/2025-7-26_interaction-nets.md), but revised so that the Boolean operator agents have two principal ports to receive inputs, and only one auxiliary port to produce the result.

As shown below, an **AND** with two ports can be rewritten immediately if it is connected to **F** via either of its principal ports, connecting the **F** to the receiver of the **AND**'s output, but if either is connected to a **T**, the agent connected to the *other* principal port will be connected to the receiver of the **AND**'s output.

{{ image(
	path="/images/polyactive-interaction-net.png",
	alt="A polyactive interaction net agent representing a Boolean AND operator"
) }}

## Extra Reading

+ [Interaction Nets - Lafont](https://dl.acm.org/doi/pdf/10.1145/96709.96718)

+ [Multiport Interaction Nets and Concurrency - Mazza](https://www-lipn.univ-paris13.fr/~mazza/papers/mINSAndConcurrency-CONCUR05.pdf)

+ [Interaction Nets: Semantics and Concurrent Extensions - Mazza](https://lipn.fr/~mazza/papers/Thesis-1.0.pdf)
