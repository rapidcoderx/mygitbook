---
description: A Practical Learning Guide for Google's Agent Framework
icon: python
---

# Google ADK - Python Masterclass

### Why I Built This

There is no shortage of AI agent demos online.

When I started learning Google's Agent Development Kit (ADK), I couldn't find a single resource that covered the full stack — from a bare-minimum single agent all the way through multi-agent hierarchies, streaming, custom agent subclasses, workflow orchestrators, MCP tool integration, and session persistence — using one consistent pattern throughout.

So I built it myself.

This is not a polished course with certificates. It is a structured reference I actually use, and I'm sharing it so others don't have to piece together the same knowledge from scattered documentation.

***

### What Is Google ADK?

Google ADK (Agent Development Kit) is an open-source Python library for building production-grade AI agents.

It is not a chatbot SDK. It is a framework for:

* defining agents with tools, instructions, and sub-agents
* running agents as part of orchestrated pipelines
* managing conversational session state across turns
* integrating with the Model Context Protocol (MCP) for external tool access

The core abstraction is the `LlmAgent` (aliased as `Agent`), which wraps any LLM, holds a list of tools, and knows how to route calls through a `Runner`.

ADK is model-agnostic. You can point the same agent code at GPT-4o, Claude 3.7, Gemini 2.0, or a local Ollama model by changing a single line.

***

### The Golden Rule This Course Enforces

Every module uses `model=LiteLlm(model="provider/model-name")` — never a raw model string.

This is not a stylistic preference. It means:

* You can swap any LLM without touching agent logic
* All traffic goes through one abstraction for cost tracking, fallbacks, and retries
* No vendor lock-in at the framework level

If you want to use this guide in a team setting, LiteLLM is the only model entry point. That rule applies from Module 01 through Module 24.

***

### What the Masterclass Covers

The guide is structured into 25 modules across four layers.

#### Layer 0 — Setup (Module 00)

Environment setup, project layout, `.env` configuration, LiteLLM provider strings for OpenAI, Anthropic, Gemini, Azure, and Ollama.

Also covers a March 2026 security advisory about a compromised LiteLLM release.

#### Layer 1 — Core Agent Types (Modules 01–08)

| Module | Topic                                                                     |
| ------ | ------------------------------------------------------------------------- |
| 01     | Basic Simple Agent — smallest working ADK setup                           |
| 02     | Multiple Agents — parallel runners, independent sessions                  |
| 03     | Agent Team — manual routing without an LLM coordinator                    |
| 04     | Streaming Agent — async generators, server-sent events                    |
| 05     | LLM Agent Deep Dive — instructions, tools, sub\_agents, callbacks         |
| 06     | Custom Agent — subclassing `BaseAgent`, `run_async`, custom routing logic |
| 07     | Multi-Agent Systems — LlmAgent as orchestrator, `AgentTool` wrappers      |
| 08     | Workflow Agents — `SequentialAgent`, `ParallelAgent`, `LoopAgent`         |

#### Layer 2 — Tools, Sessions & Protocol (Modules 09–13)

| Module | Topic                                                                               |
| ------ | ----------------------------------------------------------------------------------- |
| 09     | Function Tools — Python callables, `ToolContext`, long-running tools, agent-as-tool |
| 10     | MCP Tools (Client) — `McpToolset`, connecting ADK to MCP servers over stdio         |
| 11     | Build Your Own MCP Server — defining tools, resources, and prompts in Python        |
| 12     | In-Memory Sessions — `InMemorySessionService`, state across turns                   |
| 13     | DB Persistence — `DatabaseSessionService` with SQLite via `aiosqlite`               |

#### Layer 3 — Adaptive Intelligence (Modules 14–24)

This layer covers patterns that most agent tutorials skip entirely:

* **Feedback Loops** — routing agent outputs back into evaluation cycles
* **Outcome Tracking** — recording decisions and results for later analysis
* **Persistent Behavioral Memory** — agents that learn across sessions
* **MCP for Intelligence** — using MCP as a memory layer, not just a tool layer
* **Adaptive Prompting** — modifying instructions based on tracked outcomes
* **LLM as Judge vs Guardrails** — using a second LLM to evaluate the first
* **Decision Optimization Loops** — iterative refinement with exit conditions
* **Exploration vs Exploitation** — balancing trying new paths vs using proven ones
* **Agent Memory Architectures** — short-term, long-term, episodic, semantic patterns
* **Multi-Agent Learning** — agents that share learned state
* **Production Patterns** — deployment, observability, failure handling

***

### Who This Is For

**You will get the most out of this if you:**

* know Python reasonably well (functions, async/await, classes)
* want to understand agent frameworks conceptually, not just copy-paste demos
* building something real with an LLM and need the architecture to hold

**You may find it less useful if:**

* You are completely new to Python and need syntax explained
* You want a GUI-based no-code agent builder
* You are looking only for Google's managed Vertex AI Agent Builder service (this is the open-source ADK, not the cloud console)

***

### What Makes This Different from the ADK Documentation

The official ADK docs are reference material. They explain what each class does.

This guide explains why you structure things a certain way, what breaks if you don't, and what the practical shape of each module looks like when it runs.

The repo at [github.com/rapidcoderx/adk-masterclass](https://github.com/rapidcoderx/adk-masterclass) contains working code for every module. Not pseudocode. Not abbreviated snippets. Code you can run.

***

### How to Start

If you are new to ADK, the shortest useful path is:

1. Read Module 00 (setup) and get your environment working
2. Run Module 01 (basic agent) end-to-end
3. Read Module 04 (streaming) — most real applications need this
4. Read Module 09 (function tools) — this is where agents become genuinely useful
5. Return to Modules 05–08 to understand the architectural patterns behind what you've already built

Layer 3 (Modules 14–24) is best approached after you have run through Layers 1 and 2. The patterns build on each other.

***

### The Live Guide

The interactive version of this guide is published at:\
[**adk-masterclass.vercel.app**](https://adk-masterclass.vercel.app)

Source code:\
[**github.com/rapidcoderx/adk-masterclass**](https://github.com/rapidcoderx/adk-masterclass)

***

_This is part of my ongoing series documenting AI tools, agent frameworks, and experiments I run as a Technical Presales Consultant. The goal is practical notes, not polished tutorials. If something is wrong or out of date, the repo is public — PRs welcome._
