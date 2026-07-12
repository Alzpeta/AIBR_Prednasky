# Agentic Systems & AI Workflows

## AI Workflows

An **AI workflow** is a sequence of predefined steps where AI models transform an input into the desired output.

The workflow itself is **deterministic** and controlled by an orchestrator (code, workflow engine or business logic).

Typical characteristics:

- predefined execution flow (linear or branching)
- AI performs individual tasks (classification, extraction, summarization, ...)
- integration with external services (APIs, databases)
- shared memory for intermediate results
- repeatable and controllable execution

Example:

```
Invoice PDF

↓

OCR

↓

LLM Information Extraction

↓

Validation

↓

Database
```

AI workflows are best suited when:

- the process is well known
- all steps are predefined
- predictable execution is required

---

# AI Agents

An **AI agent** is a system that autonomously decides how to achieve a goal.

Unlike workflows, an agent is given **a goal rather than a predefined sequence of steps**.

Typical characteristics:

- goal-driven
- planning
- iterative execution (loop)
- tool usage
- adapts to the environment

Typical agent loop:

```
Task

↓

Planning

↓

Tool Usage

↓

Evaluation

↓

Repeat
```

A common implementation pattern is **ReAct (Reasoning + Acting)**.

AI agents are useful when:

- the solution path is unknown
- the required steps may change
- decisions depend on context
- flexibility is more important than determinism

Disadvantages:

- less predictable
- more expensive (multiple LLM calls)
- harder to debug

---

# Agentic Systems

An **agentic system** is the complete infrastructure around an AI agent.

It typically consists of:

- agent
- orchestrator
- tools
- memory
- execution loop

The distinction is:

> **Agent = the reasoning component**

> **Agentic System = the complete architecture**

---

# Multi-Agent Systems

Instead of a single agent, multiple agents collaborate to solve a problem.

Typical applications:

- software engineering
- research
- planning
- business automation

Use multi-agent systems when:

- the problem can be decomposed
- tasks can run in parallel
- different expertise is beneficial

Prefer a single agent when:

- the workflow is sequential
- coordination overhead outweighs the benefits

Disadvantages:

- higher cost
- coordination complexity
- communication overhead

---

# Multi-Agent Architectures

## Independent

Multiple agents work independently.

No communication.

```
Agent A

Agent B

Agent C
```

---

## Centralized

One coordinator delegates work.

```
Manager

↓

Agent A

Agent B

Agent C
```

---

## Decentralized

Agents communicate directly without a central controller.

```
Agent A ↔ Agent B ↔ Agent C
```

---

## Hierarchical

Agents are organized into multiple levels.

Higher-level agents coordinate lower-level agents.

---

## Hybrid

Combination of multiple architectures.

Most production systems use hybrid approaches.

---

# Agent Communication

Agents exchange information using:

- direct messages
- shared memory (blackboard)
- environment interaction

Historically, one standard was **FIPA ACL**.

Example:

```text
(query-if
   :sender AgentA
   :receiver AgentB
   :content "(weather Prague)"
)

↓

(inform
   :sender AgentB
   :receiver AgentA
   :content "(temperature Prague 20)"
)
```

Today, most modern systems exchange structured JSON messages instead.

---

# Model Context Protocol (MCP)

**Model Context Protocol (MCP)** is an open standard for communication between AI applications and external tools.

It standardizes:

- tool discovery
- tool descriptions
- tool invocation
- context exchange

Instead of every AI application implementing its own tool interface, MCP provides a common protocol.

Tool description:

```json
{
  "name": "get_user",
  "description": "Get user by ID",
  "input_schema": {
    "type": "object",
    "properties": {
      "user_id": {
        "type": "string"
      }
    },
    "required": ["user_id"]
  }
}
```

Tool invocation:

```json
{
  "name": "get_user",
  "arguments": {
    "user_id": "123"
  }
}
```

https://modelcontextprotocol.io/

---

# MCP Components

An MCP system typically contains:

- MCP Client (inside the AI application)
- MCP Server
- Backend systems

Backend examples:

- REST APIs
- databases
- enterprise systems
- local tools

Responsibilities of the MCP Server:

- expose available tools
- provide context
- execute actions
- return results

Typical flow:

```
User

↓

LLM

↓

Tool Selection

↓

Orchestrator

↓

MCP Client

↓

MCP Server

↓

Backend

↓

Result

↓

LLM
```

Advantages:

- abstraction
- reusable tools
- interchangeable backends
- plug-and-play integrations

---

# Building AI Workflows

## n8n

https://n8n.io/

Node-based workflow automation.

Features:

- visual editor
- AI integrations
- APIs
- databases
- scheduling

Best suited for:

- business automation
- AI workflows
- integrations

---

## Make

https://www.make.com/

Visual automation platform.

Compared to n8n:

- easier for non-programmers
- rich ecosystem
- cloud-first

---

## Zapier

https://zapier.com/

Simple workflow automation.

Best for:

- linear workflows
- SaaS integrations
- no-code automation

---

# AI Agent Frameworks

## LangChain

General-purpose framework for building LLM applications.

Provides:

- prompt management
- tool integration
- memory
- document processing

Today, LangChain is primarily used as a **library**, while more complex agent systems are typically built using LangGraph.

Example:

```python
from langchain.chat_models import ChatOpenAI
from langchain.agents import initialize_agent, Tool

llm = ChatOpenAI()

tools = [
    Tool(...)
]

agent = initialize_agent(tools, llm)
```

---

## LangGraph

State-based framework for building complex agent workflows.

Designed for:

- explicit orchestration
- agent loops
- branching logic
- production systems

Example:

```python
graph = StateGraph(State)

graph.add_node(...)

graph.add_edge(...)
```

One of the most widely adopted frameworks for production AI agents.

---

## Hermes

Modern open-source framework for building AI agents.

Focus:

- lightweight architecture
- MCP integration
- tool orchestration
- production-ready agent systems

Example:

```python
from hermes import Agent

agent = Agent(
    model="gpt-5",
    tools=[weather_tool]
)

response = agent.run(
    "What's the weather in Prague?"
)
```

Suitable for:

- autonomous agents
- AI assistants
- production deployments

https://github.com/Entropy-AI/hermes

---

## CrewAI

Framework based on role-oriented agents.

Each agent has:

- role
- goal
- responsibilities

Example:

```python
researcher = Agent(...)
writer = Agent(...)

task1 = Task(...)
task2 = Task(...)
```

Suitable for:

- research
- content generation
- business workflows

---

## Semantic Kernel

Microsoft framework for enterprise AI.

Focus:

- enterprise integration
- orchestration
- memory
- plugins
- security

Popular in Microsoft ecosystems.

---

# Summary

## AI Workflow

- predefined process
- deterministic
- repeatable
- highly controllable

↓

Best when the process is already known.

---

## AI Agent

- receives a goal
- plans autonomously
- iterates
- uses tools
- adapts to changing situations

↓

Best when the solution path is unknown.

---

## Agentic System

Complete infrastructure around one or more AI agents.

Components:

- agent
- orchestrator
- tools
- memory
- execution loop

---

## Multi-Agent System

Multiple specialized agents collaborate to solve complex problems.

---

## MCP

Standard protocol connecting AI applications with external tools and services.

---

## Modern Frameworks

- LangChain
- LangGraph
- Hermes
- CrewAI
- Semantic Kernel

These frameworks simplify the development of modern AI workflows and agentic systems.