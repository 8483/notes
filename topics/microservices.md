# Microservices

Developing a single application as a suite of small services, each running its own process and communicating with lightweight mechanisms, often an HTTP resource API.

Think of it like an organs system, where each organ has a purpose, the organs form a system, which form an organism.

# Misconceptions

Misconception 1

Micro services enable our teams to choose the best programming languages and frameworks for their tasks

Reality

It's super expensive to do this. Team size and investment are critical inputs.

Misconception 2

Code generation is evil

What's important is creating a defined schema that is 100% trusted.

Misconception 3

The event log must be the source of truth

Events are critical parts of an interface. But it's okay for services to be the system of record for their resources.

Misconception 4

Developers can maintain no more than 3 services each.

Wrong metric.
