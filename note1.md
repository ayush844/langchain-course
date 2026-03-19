
# 🧠 AI Agents — Basic Notes

## 1. What are AI Agents?

An **AI Agent** is a system that:

* **Perceives** input (user query, environment, data)
* **Reasons** about what to do
* **Acts** using tools or APIs
* **Iterates** until it reaches a goal

👉 In simple terms:

> An AI agent = **LLM + reasoning + tools + memory + decision-making loop**

### Key Components

* **LLM (Brain)** → e.g., GPT, Claude
* **Tools (Actions)** → APIs, DB queries, search
* **Memory** → past context or history
* **Planner/Controller** → decides next step

---

## 2. Chains vs AI Agents

### 🔗 Chains (Simple Flow)

* Predefined sequence of steps
* No decision-making
* Linear execution

Example:

```
User input → LLM → Output
```

👉 Characteristics:

* Deterministic
* Fixed pipeline
* No tool selection logic

---

### 🤖 AI Agents (Dynamic Flow)

* Can **decide what to do next**
* Choose tools dynamically
* Iterate based on results

Example:

```
User → LLM → Decide → Call Tool → Observe → Repeat → Final Answer
```

👉 Characteristics:

* Non-linear
* Adaptive
* Uses reasoning loop

---

### ⚖️ Key Difference

| Feature         | Chains    | AI Agents         |
| --------------- | --------- | ----------------- |
| Flow            | Fixed     | Dynamic           |
| Decision-making | ❌ No      | ✅ Yes             |
| Tool usage      | Hardcoded | Chosen at runtime |
| Flexibility     | Low       | High              |

---

## 3. ReAct Agent Architecture

ReAct = **Reason + Act**

👉 It combines:

* **Reasoning (Thought)**
* **Action (Tool usage)**

---

### 🔄 Core Loop

```
Thought → Action → Observation → Thought → ...
```

---

### 🧩 Step-by-step Flow

1. **Thought**

   * LLM reasons what to do next
     Example: “I should search for this info”

2. **Action**

   * Calls a tool
     Example: `search("LangChain")`

3. **Observation**

   * Gets result from tool

4. **Repeat**

   * Continues until final answer

---

### 📌 Example

```
User: What is the capital of France?

Thought: I already know this.
Action: (no tool)
Final Answer: Paris
```

OR

```
User: Latest stock price of Tesla

Thought: I need real-time data
Action: call_stock_api("Tesla")
Observation: $210
Final Answer: Tesla is trading at $210
```

---

## 4. Evolution of ReAct Agents

This shows how modern agent systems evolved 👇

---

### 🧾 1. ReAct Prompt (Manual)

* Everything done via **prompt engineering**
* LLM outputs:

  * Thought
  * Action
  * Observation (manually fed back)

👉 Problems:

* Fragile
* Hard to scale
* Manual parsing

---

### 🔧 2. Tool Calling ReAct Agent (Function Calling)

* Introduced **structured tool calling**
* LLM returns JSON → tool executes automatically

👉 Improvements:

* Reliable
* No string parsing
* Better integration

Example:

```json
{
  "tool": "search",
  "arguments": {"query": "LangChain"}
}
```

---

### 🧠 3. LangGraph ReAct Agent (Advanced)

Built using **LangGraph**

👉 Features:

* Graph-based execution
* Stateful workflows
* Cycles (loops)
* Persistent memory

Flow becomes:

```
Nodes (LLM, Tool, Memory) connected as graph
```

👉 Why important:

* Production-grade agents
* More control over execution

---

### ⚙️ 4. LangChain `create_agent` (Simplified API)

Built on top of:

* **LangChain**
* Uses LangGraph internally

👉 What it does:

* Hides complexity
* Easy agent creation

Example:

```python
agent = create_agent(model, tools)
```

👉 Internally:

```
create_agent → LangGraph → ReAct architecture → Tool calling
```

---

## 🧬 Evolution Summary

```
ReAct Prompt
   ↓
Tool Calling (Function Calling)
   ↓
LangGraph ReAct Agent
   ↓
LangChain create_agent (Abstraction)
```

---

## 🎯 Final Intuition

* **Chains** = fixed pipelines
* **Agents** = thinking + decision-making systems
* **ReAct** = core idea behind modern agents
* **LangGraph** = execution engine
* **LangChain** = developer-friendly interface

---

