
# AI Agents – Basics

## 1. What are AI Agents?

An **AI Agent** is a system that uses a **Large Language Model (LLM)** to **reason, make decisions, and take actions to achieve a goal**.

Unlike a simple chatbot that only generates text responses, an AI agent can:

* **Understand a task**
* **Break it into steps**
* **Use external tools**
* **Observe results**
* **Decide the next action**

This makes agents capable of performing **multi-step problem solving**.

### Basic Components of an AI Agent

1. **LLM (Brain)**
   Responsible for reasoning and decision making.

2. **Tools**
   External capabilities the agent can use such as:

   * APIs
   * Database queries
   * Web search
   * Code execution

3. **Memory (optional)**
   Stores previous interactions or state.

4. **Planning / Reasoning Loop**
   The agent repeatedly:

   * thinks
   * chooses an action
   * observes results
   * continues until the goal is achieved.

### Simple Agent Loop

```
User Query → Agent Reasoning → Tool Usage → Observation → Next Step → Final Answer
```

Example:

User asks:

> “Find the weather in Delhi and tell me if I should carry an umbrella.”

Agent may:

1. Decide to call a **weather API**
2. Receive weather data
3. Analyze rain probability
4. Produce the final answer.

---

# 2. ReAct Architecture

**ReAct** stands for:

**Reason + Act**

It is an architecture that allows an LLM to **combine reasoning with tool usage**.

Instead of generating only the final answer, the model produces **intermediate reasoning steps**.

The agent alternates between:

1. **Thought (reasoning)**
2. **Action (tool usage)**
3. **Observation (result of tool)**

This loop continues until the task is completed.

---

### ReAct Loop

```
Thought → Action → Observation → Thought → Action → Observation → Final Answer
```

---

### Example ReAct Workflow

User asks:

> “Who is the CEO of Tesla and what is his age?”

Agent reasoning:

```
Thought: I need to find the CEO of Tesla
Action: Search("CEO of Tesla")

Observation: Elon Musk

Thought: Now I need Elon Musk's age
Action: Search("Elon Musk age")

Observation: 52

Final Answer: Elon Musk is the CEO of Tesla and he is 52 years old.
```

---

### Why ReAct is Powerful

ReAct enables agents to:

* perform **multi-step reasoning**
* interact with **external tools**
* solve **complex tasks**
* reduce hallucinations by **checking tools**

This architecture became the **foundation for most modern AI agents**.

---

# 3. Evolution of ReAct Agents

The ReAct architecture evolved through several stages as LLM tooling improved.

---

# Stage 1 — ReAct Prompting (Original Method)

In the beginning, ReAct was implemented using **prompt engineering**.

The prompt explicitly instructed the model to follow the format:

```
Thought:
Action:
Observation:
```

Example prompt:

```
You are an AI agent that can reason and act.

Use the following format:

Thought:
Action:
Observation:
Final Answer:
```

### Problems with Prompt-Based ReAct

* tools were parsed from **text**
* **fragile parsing**
* models sometimes **break format**
* difficult to scale

This led to **structured tool calling**.

---

# Stage 2 — Tool Calling ReAct (Function Calling)

LLM providers like:

* OpenAI
* Anthropic
* Groq

introduced **function calling / tool calling**.

Instead of writing actions as text, the model can now **call structured functions**.

Example tool definition:

```python
{
  "name": "get_weather",
  "description": "Get weather for a city",
  "parameters": {
    "city": "string"
  }
}
```

Now the model produces structured output:

```
{
  "tool_call": "get_weather",
  "arguments": {
     "city": "Delhi"
  }
}
```

Advantages:

* **structured output**
* **no parsing errors**
* **more reliable tool execution**
* **better developer control**

This became the **standard for modern agents**.

---

# Stage 3 — LangGraph ReAct Agent

As agent workflows became more complex, **LangGraph** was introduced.

LangGraph allows developers to build agents as **graphs of nodes** instead of a simple loop.

### Why LangGraph?

Traditional agents have a fixed loop:

```
LLM → Tool → LLM → Tool
```

But real applications require:

* branching logic
* retries
* memory
* human-in-the-loop
* long-running workflows

LangGraph solves this by modeling agents as **state machines / graphs**.

---

### LangGraph ReAct Flow

```
User Input
     ↓
Reasoning Node (LLM)
     ↓
Tool Node
     ↓
Observation
     ↓
Loop or Finish
```

Benefits:

* **durable execution**
* **better debugging**
* **state management**
* **complex workflows**

LangGraph is currently the **recommended architecture for production agents**.

---

# Stage 4 — LangChain `create_agent()` (Built on LangGraph)

LangChain simplified agent creation using the `create_agent()` API.

Internally:

```
create_agent() → builds a LangGraph ReAct agent
```

So developers get the power of LangGraph **without manually building the graph**.

Example:

```python
from langchain.agents import create_agent

agent = create_agent(
    model="gpt-4",
    tools=[search_tool, weather_tool]
)
```

The agent automatically:

1. reasons with the LLM
2. selects tools
3. executes them
4. loops until completion.

---

# Summary of ReAct Evolution

| Stage | Method                   | Key Idea                                   |
| ----- | ------------------------ | ------------------------------------------ |
| 1     | ReAct Prompt             | Reasoning + actions written in prompt text |
| 2     | Tool Calling ReAct       | Structured function calling                |
| 3     | LangGraph ReAct          | Graph-based agent workflows                |
| 4     | LangChain `create_agent` | Simplified interface built on LangGraph    |

---

# Final Key Idea

Modern AI agents are essentially:

```
LLM + Tools + Reasoning Loop
```

And the **ReAct architecture** provides the mechanism for:

```
Think → Act → Observe → Repeat
```

This is the core principle behind most modern agent frameworks like:

* LangChain
* LangGraph
* AutoGPT
* OpenAI Assistants
* CrewAI

---
---
---
---
---


