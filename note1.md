
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

```python
from dotenv import load_dotenv
from langchain.agents import create_agent
from langchain.tools import tool
from langchain_core.messages import HumanMessage
from langchain_openai import ChatOpenAI

load_dotenv()

@tool
def search(query: str) -> str:
    """
    Tool that searches over internet
    Args:
        query: the query to search for
    Returns:
    The search result
    """
    print(f"Searching for {query}")
    return "Tokyo weather is sunny..."

llm = ChatOpenAI()
tools = [search]
agent = create_agent(model=llm, tools=tools)

def main():
    print("Hello from langchain-course!")
    result = agent.invoke({"messages": HumanMessage(content="What is the weather in Tokyo?")})
    print(result)
    # {'messages': [HumanMessage(content='What is the weather in Tokyo?', additional_kwargs={}, response_metadata={}, id='b8f44c28-02f9-4380-9e8c-6aedaa6ea459'), AIMessage(content='', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 15, 'prompt_tokens': 70, 'total_tokens': 85, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_provider': 'openai', 'model_name': 'gpt-3.5-turbo-0125', 'system_fingerprint': None, 'id': 'chatcmpl-DLKx2SjZQL8WWLGxJLP9YV3L09tPk', 'service_tier': 'default', 'finish_reason': 'tool_calls', 'logprobs': None}, id='lc_run--019d0950-f988-78a0-8748-0ccf72a1e3e4-0', tool_calls=[{'name': 'search', 'args': {'query': 'weather in Tokyo'}, 'id': 'call_hPPdeE0NnjGISirFi7jFRsNp', 'type': 'tool_call'}], invalid_tool_calls=[], usage_metadata={'input_tokens': 70, 'output_tokens': 15, 'total_tokens': 85, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}}), ToolMessage(content='Tokyo weather is sunny...', name='search', id='6ab6dc70-75ea-4ffd-81d9-c29c1168900f', tool_call_id='call_hPPdeE0NnjGISirFi7jFRsNp'), AIMessage(content='The weather in Tokyo is currently sunny.', additional_kwargs={'refusal': None}, response_metadata={'token_usage': {'completion_tokens': 9, 'prompt_tokens': 98, 'total_tokens': 107, 'completion_tokens_details': {'accepted_prediction_tokens': 0, 'audio_tokens': 0, 'reasoning_tokens': 0, 'rejected_prediction_tokens': 0}, 'prompt_tokens_details': {'audio_tokens': 0, 'cached_tokens': 0}}, 'model_provider': 'openai', 'model_name': 'gpt-3.5-turbo-0125', 'system_fingerprint': None, 'id': 'chatcmpl-DLKx5vSTaK8gybh6ai8wduZfa2Ngc', 'service_tier': 'default', 'finish_reason': 'stop', 'logprobs': None}, id='lc_run--019d0951-01c8-7ec1-916a-477b74cf09c5-0', tool_calls=[], invalid_tool_calls=[], usage_metadata={'input_tokens': 98, 'output_tokens': 9, 'total_tokens': 107, 'input_token_details': {'audio': 0, 'cache_read': 0}, 'output_token_details': {'audio': 0, 'reasoning': 0}})]}


if __name__ == "__main__":
    main()

```

---
---
---

```python
from dotenv import load_dotenv
from langchain.agents import create_agent
from langchain.tools import tool
from langchain_core.messages import HumanMessage
from langchain_openai import ChatOpenAI
from tavily import TavilyClient

load_dotenv()

tavily_client = TavilyClient()

@tool
def search(query: str) -> str:
    """
    Tool that searches over internet
    Args:
        query: the query to search for
    Returns:
    The search result
    """
    print(f"Searching for {query}")
    return tavily_client.search(query=query)

llm = ChatOpenAI(model="gpt-5")
tools = [search]
agent = create_agent(model=llm, tools=tools)

def main():
    print("Hello from langchain-course!")
    result = agent.invoke({"messages": HumanMessage(content="search for 3 jobb postings for an AI engineer using langchain in tyhe bay area on linkedin and list their details.")})
    print(result)


if __name__ == "__main__":
    main()

```

---
---
---


```python
from dotenv import load_dotenv
from langchain.agents import create_agent
from langchain.tools import tool
from langchain_core.messages import HumanMessage
from langchain_openai import ChatOpenAI
# from tavily import TavilyClient
from langchain_tavily import TavilySearch

load_dotenv()

llm = ChatOpenAI(model="gpt-5")
tools = [TavilySearch()]
agent = create_agent(model=llm, tools=tools)

def main():
    print("Hello from langchain-course!")
    result = agent.invoke({"messages": HumanMessage(content="search for 3 jobb postings for an AI engineer using langchain in tyhe bay area on linkedin and list their details.")})
    print(result)


if __name__ == "__main__":
    main()

```
---
---
---
# EXPLAINATION:


# 🧩 Code Block 1 — Basic Custom Tool Agent

```python
from dotenv import load_dotenv
from langchain.agents import create_agent
from langchain.tools import tool
from langchain_core.messages import HumanMessage
from langchain_openai import ChatOpenAI

load_dotenv()

@tool
def search(query: str) -> str:
    """
    Tool that searches over internet
    Args:
        query: the query to search for
    Returns:
    The search result
    """
    print(f"Searching for {query}")
    return "Tokyo weather is sunny..."

llm = ChatOpenAI()
tools = [search]
agent = create_agent(model=llm, tools=tools)

def main():
    print("Hello from langchain-course!")
    result = agent.invoke({"messages": HumanMessage(content="What is the weather in Tokyo?")})
    print(result)

if __name__ == "__main__":
    main()
```

## 🧠 What this code does

This creates a **basic AI agent using a custom tool**.

### Flow:

1. Loads environment variables (API keys)
2. Defines a **custom tool (`search`)**
3. Creates an LLM (`ChatOpenAI`)
4. Passes tool + LLM into `create_agent`
5. Sends a user query to the agent

---

## 🔄 Internal Execution (ReAct Flow)

```text
User → LLM → decides to use tool → calls search → gets result → final answer
```

---

## ⚠️ Important Points

* `@tool` → makes function usable by agent
* Tool is **fake (hardcoded response)**
* Agent still behaves like real:

  * decides to use tool
  * calls it
  * uses result

---

## 🎯 Key Insight

👉 This is a **learning setup to understand agent loop**, not real-world usage.

---

---

# 🌐 Code Block 2 — Custom Tool with Real API (Tavily)

```python
from dotenv import load_dotenv
from langchain.agents import create_agent
from langchain.tools import tool
from langchain_core.messages import HumanMessage
from langchain_openai import ChatOpenAI
from tavily import TavilyClient

load_dotenv()

tavily_client = TavilyClient()

@tool
def search(query: str) -> str:
    """
    Tool that searches over internet
    Args:
        query: the query to search for
    Returns:
    The search result
    """
    print(f"Searching for {query}")
    return tavily_client.search(query=query)

llm = ChatOpenAI(model="gpt-5")
tools = [search]
agent = create_agent(model=llm, tools=tools)

def main():
    print("Hello from langchain-course!")
    result = agent.invoke({"messages": HumanMessage(content="search for 3 jobb postings for an AI engineer using langchain in tyhe bay area on linkedin and list their details.")})
    print(result)

if __name__ == "__main__":
    main()
```

## 🧠 What this code does

This builds a **real-world AI agent with internet search capability**.

---

## 🔄 Flow

1. User asks a complex query (job search)
2. LLM realizes:

   * “I need external data”
3. Calls `search` tool
4. Tool uses **Tavily API (real search engine)**
5. LLM processes results → generates answer

---

## ⚠️ Key Differences from Code 1

* Tool is **real (API call)** instead of fake
* Uses **Tavily**
* Model upgraded to `"gpt-5"` → better reasoning

---

## 🎯 Key Insight

👉 Now agent becomes:

* **Decision maker**
* **Tool user**
* **Data interpreter**

---

---

# ⚡ Code Block 3 — Prebuilt Tool (LangChain Tavily)

```python
from dotenv import load_dotenv
from langchain.agents import create_agent
from langchain.tools import tool
from langchain_core.messages import HumanMessage
from langchain_openai import ChatOpenAI
# from tavily import TavilyClient
from langchain_tavily import TavilySearch

load_dotenv()

llm = ChatOpenAI(model="gpt-5")
tools = [TavilySearch()]
agent = create_agent(model=llm, tools=tools)

def main():
    print("Hello from langchain-course!")
    result = agent.invoke({"messages": HumanMessage(content="search for 3 jobb postings for an AI engineer using langchain in tyhe bay area on linkedin and list their details.")})
    print(result)

if __name__ == "__main__":
    main()
```

## 🧠 What this code does

This creates the **same agent as Code 2**, but using a **prebuilt tool instead of custom code**.

---

## 🔄 Flow

Same as before:

```text
User → LLM → calls TavilySearch → gets results → final answer
```

---

## ⚠️ Key Differences from Code 2

* No manual tool creation
* Uses **LangChain** integration
* Cleaner and less code

---

## ✅ Why this is better

* Pre-configured tool
* Optimized for agents
* Less chance of errors
* Production-friendly

---

## 🎯 Key Insight

👉 Prefer this approach in real projects.

---

# 🧬 Final Understanding

All three follow the same core:

```text
create_agent → ReAct loop → tool calling → final answer
```

---

## 🧠 Mental Model

* Code 1 → Learn how agents work
* Code 2 → Build your own tools
* Code 3 → Use ecosystem tools (best practice)

---
---
---



# 🧩 Code — Custom Structured Response

```python
from typing import List
from pydantic import BaseModel, Field
from dotenv import load_dotenv
load_dotenv()

from langchain.agents import create_agent
from langchain.tools import tool
from langchain_core.messages import HumanMessage
from langchain_openai import ChatOpenAI
# from tavily import TavilyClient
from langchain_tavily import TavilySearch


class Source(BaseModel):
    """Schema for a source used by the agent"""

    url: str = Field(description="the URL of the source")


class AgentResponse(BaseModel):
    """Schema for agent rewsponse with answers and sources"""

    answer: str = Field(description="the agent's answer to the query")
    sources: List[Source] = Field(default_factory=list, description="List of sources used to generate the answers")


llm = ChatOpenAI(model="gpt-5")
tools = [TavilySearch()]
agent = create_agent(model=llm, tools=tools, response_format=AgentResponse)

def main():
    print("Hello from langchain-course!")
    result = agent.invoke({"messages": HumanMessage(content="search for 3 jobb postings for an AI engineer using langchain in tyhe bay area on linkedin and list their details.")})
    print(result)


if __name__ == "__main__":
    main()
```

---

# 🧠 What this code does (Core Idea)

👉 This agent:

* Searches using Tavily
* BUT instead of returning plain text
* It returns a **structured, typed response**

---

# 🧱 Step-by-step Understanding

## 1. You Define a Schema (VERY IMPORTANT)

### 🔹 Source Model

```python
class Source(BaseModel):
    url: str
```

👉 Represents:

* One source (like a link)

---

### 🔹 AgentResponse Model

```python
class AgentResponse(BaseModel):
    answer: str
    sources: List[Source]
```

👉 This defines:

* What your **final output MUST look like**

---

## 🧠 Think of it like:

```text
You are telling the LLM:

"Don't give random text.
Give output EXACTLY in this structure."
```

---

## 2. Pass Schema to Agent

```python
agent = create_agent(..., response_format=AgentResponse)
```

👉 This is the key line.

It tells the agent:

* “Your final answer must follow this schema”

---

## 3. What Happens Internally

### Without structured output:

```json
"Here are 3 jobs... (messy text)"
```

---

### With structured output:

```json
{
  "answer": "Here are 3 jobs...",
  "sources": [
    {"url": "https://..."},
    {"url": "https://..."}
  ]
}
```

---

## 🔄 Internal Flow (Important)

```text
User → Agent (ReAct loop + tools) → Final step → LLM formats into schema
```

👉 Key insight:

* Tools + reasoning = SAME
* Only **final output formatting changes**

---

# ⚠️ Important Concepts

## 1. Pydantic = Structure Enforcer

Using **Pydantic**

* Validates output
* Ensures correct types
* Prevents broken responses

---

## 2. LLM is Forced to Follow Schema

* Uses **function calling / structured output mode**
* If output is wrong → it retries internally

---

## 3. Why This is Powerful

Without this:

* ❌ Hard to parse
* ❌ Inconsistent output

With this:

* ✅ API-ready responses
* ✅ Predictable format
* ✅ Easy frontend usage

---

# 🎯 Real-World Analogy

👉 Normal agent:

> “Here are some jobs from LinkedIn…”

👉 Structured agent:

```json
{
  "answer": "...",
  "sources": [...]
}
```

---

# 🧬 Simple Explanation of Structured Output (From Docs)

Based on **LangChain docs:

---

## 🧠 What is Structured Output?

👉 It means:

> Forcing the LLM to return data in a **fixed schema (like JSON)** instead of free text.

---

## 🔑 Key Idea

Instead of:

```text
"Paris is the capital of France"
```

You get:

```json
{
  "city": "Paris",
  "country": "France"
}
```

---

## 🧩 How LangChain Does It

1. You define schema (Pydantic)
2. Pass it to agent/LLM
3. LangChain:

   * Converts schema → tool/function format
   * Forces model to respond in that format

---

## ⚙️ Under the Hood

```text
Pydantic Model → JSON Schema → LLM Function Calling → Validated Output
```

---

## 📌 Why It Exists

Because LLMs:

* Are **not deterministic**
* Can return messy text

👉 Structured output fixes that.

---

# 🚀 Final Intuition

👉 Combine everything:

```text
Agent = Thinking + Tool Use + Structured Output
```

* ReAct → decides what to do
* Tools → fetch data
* Structured output → clean final answer

---

# 🧠 One-Line Summary

> Structured output = “Make LLM behave like an API, not a chatbot.”

---

