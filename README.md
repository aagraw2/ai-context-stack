# AIContextStack

A structured knowledge system to improve how LLMs like Claude and Cursor consume context.

Instead of passing large amounts of unstructured information into prompts, this system organizes knowledge into multiple layers and focuses on selecting only the most relevant context for a given problem.

This leads to:

* Lower token usage
* Faster responses
* Better signal to noise ratio
* More consistent system design outputs

---

## Core idea

LLMs are powerful, but their effectiveness depends heavily on the quality of the context they receive.

In most AI-assisted workflows, the main bottleneck is not generation. It is:

* Selecting the right information
* Avoiding irrelevant context
* Managing token and latency costs

This repository treats context selection as a system design problem.

---

## Structure

The knowledge is organized into the following layers:

* Fundamentals: core concepts and principles
* Capsules: reusable, atomic knowledge units
* Patterns: higher-level system design patterns
* Tradeoffs: decision-making guidance
* Anti-patterns: common pitfalls and failure modes
* Case studies: real-world applications
* Prompts: interaction layer for LLM usage

An indexing layer (`_index.md`) maps problems to relevant concepts.

---

## How it works

1. Start with a problem, for example designing a notification system
2. Use the index to identify relevant concepts and patterns
3. Select only the necessary files instead of the entire knowledge base
4. Provide that focused context to the LLM
5. Iterate based on gaps

This avoids prompt stuffing and leads to more targeted reasoning.

---

## Example

Query: Design a rate limiter for a distributed system

Selected context:

* rate-limiter (capsule)
* caching (capsule)
* distributed coordination (pattern)

Result:

* More focused reasoning
* Better tradeoff analysis
* Reduced token usage

---

## Extensibility

This approach can be extended using:

* Rule-based selection
* Embedding-based search
* Vector databases

---

## Why this matters

This repository reflects:

* Structured thinking about LLM limitations
* Separation of storage, indexing, and retrieval
* Focus on efficiency and scalability
* Application of system design principles to AI workflows
