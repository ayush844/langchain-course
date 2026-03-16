
# LangChain – Core Concepts Notes

## 1. What is LangChain?

### Definition

**LangChain** is a framework for building applications powered by **Large Language Models (LLMs)**.

It helps developers:

* interact with LLMs
* structure prompts
* connect models with external data
* build pipelines of AI operations

Instead of manually handling prompts and API calls, LangChain provides **abstractions and tools** to organize them.

---

### Why LangChain is Needed

Using raw LLM APIs quickly becomes messy:

Example without LangChain:

```python
prompt = f"Summarize this: {text}"
response = openai.chat.completions.create(...)
```

Problems:

* prompts scattered across code
* difficult to reuse
* hard to connect with tools or databases
* no structured pipeline

LangChain solves this by providing:

* **Prompt templates**
* **Chains**
* **Agents**
* **Memory**
* **Retrievers**

---

### Typical LangChain Architecture

```text
User Input
    ↓
Prompt Template
    ↓
LLM (Chat Model)
    ↓
Output Parser / Result
```

In more advanced systems:

```text
User Query
     ↓
Retriever (Vector DB)
     ↓
Relevant Documents
     ↓
Prompt
     ↓
LLM
     ↓
Answer
```

This pattern is called **RAG (Retrieval Augmented Generation)**.

---

# 2. Prompt Templates

## Definition

A **PromptTemplate** is a reusable prompt with **variables/placeholders**.

Instead of writing prompts manually every time, you create a template and fill it with data dynamically.

---

### Example Without PromptTemplate

```python
prompt = f"Summarize the following text: {information}"
```

This becomes messy in large applications.

---

### Example With PromptTemplate

```python
PromptTemplate(
    input_variables=["information"],
    template="Summarize the following text: {information}"
)
```

Now LangChain replaces `{information}` automatically.

---

### Benefits

* reusable prompts
* cleaner code
* dynamic input handling
* easier prompt engineering

---

### Example

Template:

```text
Summarize the following information:

{information}
```

Input:

```text
Elon Musk biography
```

Final prompt sent to the LLM:

```text
Summarize the following information:

Elon Musk biography
```

---

# 3. Chat Models

## Definition

A **Chat Model** is an LLM designed for **conversation-style input and output**.

Examples:

* GPT-4
* GPT-4o
* GPT-5
* Claude
* Gemini

In LangChain, chat models are wrapped using classes like:

```python
ChatOpenAI
```

---

### Example

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(
    model="gpt-5",
    temperature=0
)
```

---

### Important Parameters

#### Model

Specifies the AI model used.

Example:

```python
model="gpt-5"
```

---

#### Temperature

Controls randomness of responses.

| Temperature | Behavior          |
| ----------- | ----------------- |
| 0           | deterministic     |
| 0.3         | slightly creative |
| 0.7         | balanced          |
| 1           | very creative     |

Typical usage:

| Task             | Temperature |
| ---------------- | ----------- |
| Summarization    | 0           |
| Extraction       | 0           |
| Coding           | 0.1         |
| Creative writing | 0.7+        |

---

### Chat Model Input/Output

Chat models exchange **messages** instead of plain text.

Example:

Input:

```text
User: Summarize this text
```

Output:

```text
AI: Here is the summary...
```

LangChain returns objects like:

```python
AIMessage(content="Generated response")
```

---

# 4. Chains

## Definition

A **Chain** is a pipeline that connects multiple components together.

Each component processes the output of the previous one.

---

### Basic Chain Example

```text
Input
 ↓
Prompt Template
 ↓
LLM
 ↓
Response
```

In code:

```python
chain = prompt_template | llm
```

This means:

```text
PromptTemplate → LLM
```

---

### How Chains Work

Steps:

1. Input data is provided
2. PromptTemplate formats the prompt
3. Prompt is sent to the LLM
4. LLM generates output
5. Output is returned

---

### Example

```python
chain.invoke({
    "information": biography_text
})
```

Flow:

```text
information text
      ↓
PromptTemplate
      ↓
formatted prompt
      ↓
Chat Model
      ↓
AI response
```

---

# 5. LangChain Expression Language (LCEL)

LangChain introduced **LCEL** to make pipelines easier.

The operator:

```python
|
```

connects components together.

Example:

```python
chain = prompt | llm
```

Meaning:

```text
Prompt output → LLM input
```

---

### Example Pipeline

```text
Input
 ↓
Prompt Template
 ↓
LLM
 ↓
Output Parser
 ↓
Final Result
```

---

# 6. Key LangChain Components (High Level)

| Component       | Purpose                    |
| --------------- | -------------------------- |
| PromptTemplate  | structure prompts          |
| LLM / ChatModel | generate AI responses      |
| Chain           | connect components         |
| Retriever       | fetch documents            |
| Memory          | store conversation history |
| Agent           | decide which tool to use   |

---

# 7. Simple LangChain Application Structure

Most basic LangChain apps follow this pattern:

```text
Data/Input
     ↓
Prompt Template
     ↓
LLM
     ↓
Output
```

More advanced apps add:

```text
User Query
     ↓
Retriever
     ↓
Relevant Documents
     ↓
Prompt
     ↓
LLM
     ↓
Answer
```

---

# 8. Summary

LangChain provides tools to organize interactions with LLMs.

Core ideas:

| Concept        | Meaning                        |
| -------------- | ------------------------------ |
| LangChain      | framework for LLM apps         |
| PromptTemplate | reusable prompt with variables |
| Chat Model     | AI model for conversations     |
| Chain          | pipeline connecting components |

---
---
---
---

# LangChain Notes – Basic Prompt → LLM Pipeline

## 1. Purpose of the Program

This program uses **LangChain with OpenAI** to process text information and generate:

1. A **short summary**
2. **Two interesting facts**

The input is a biography of **Elon Musk**, and the output is generated by an **LLM (Large Language Model)**.

### Overall Flow

```
Input Text → Prompt Template → LLM → AI Response → Print Output
```

---

# 2. Importing Required Libraries

```python
import os
from dotenv import load_dotenv
from langchain_core.prompts import PromptTemplate
from langchain_openai import ChatOpenAI
```

### Explanation

| Library          | Purpose                                |
| ---------------- | -------------------------------------- |
| `os`             | Access environment variables           |
| `dotenv`         | Load variables from `.env` file        |
| `PromptTemplate` | Create reusable prompts with variables |
| `ChatOpenAI`     | Interface to OpenAI chat models        |

---

# 3. Loading Environment Variables

```python
load_dotenv()
```

### Purpose

Loads environment variables (like API keys) from a `.env` file.

Example `.env` file:

```
OPENAI_API_KEY=your_openai_api_key
```

This allows LangChain to authenticate with OpenAI.

---

# 4. Input Data

```python
information = """ Elon Musk biography """
```

### Purpose

This variable contains **context information** that will be sent to the LLM.

Important concept:

LLMs **do not remember data automatically**, so you must provide all necessary information in the prompt.

---

# 5. Creating the Prompt Template

```python
summary_template = """
given the information {information}, about a person i want you to create:
1. a short summary
2. two interesting facts about the person
"""
```

### Prompt Template Concept

A **PromptTemplate** allows dynamic prompts using placeholders.

Example placeholder:

```
{information}
```

Later, LangChain replaces this with actual data.

Example final prompt sent to the model:

```
given the information Elon Musk biography...

create:
1. short summary
2. interesting facts
```

---

# 6. Building the PromptTemplate Object

```python
summary_prompt_template = PromptTemplate(
    input_variables=["information"],
    template=summary_template
)
```

### Explanation

| Parameter         | Meaning                            |
| ----------------- | ---------------------------------- |
| `input_variables` | Variables required in the template |
| `template`        | The prompt template string         |

This tells LangChain that the template expects **one variable: `information`**.

---

# 7. Creating the LLM

```python
llm = ChatOpenAI(temperature=0, model="gpt-5")
```

### ChatOpenAI

This connects LangChain to an **OpenAI chat model**.

### Parameters

#### Model

```
model="gpt-5"
```

Specifies which AI model will generate the response.

---

#### Temperature

```
temperature=0
```

Controls **creativity/randomness** of the output.

| Temperature | Behavior                  |
| ----------- | ------------------------- |
| 0           | deterministic, consistent |
| 0.5         | balanced                  |
| 1           | creative                  |

Best practice:

| Task               | Temperature |
| ------------------ | ----------- |
| Summarization      | 0           |
| Question answering | 0–0.3       |
| Creative writing   | 0.7+        |

---

# 8. Creating a Chain (LCEL)

```python
chain = summary_prompt_template | llm
```

This uses **LangChain Expression Language (LCEL)**.

The pipe operator `|` creates a **processing pipeline**.

### Meaning

```
PromptTemplate → LLM
```

The output of the prompt template becomes the input to the LLM.

### Pipeline Visualization

```
Input Data
    ↓
PromptTemplate
    ↓
Formatted Prompt
    ↓
ChatOpenAI Model
    ↓
AI Response
```

---

# 9. Running the Chain

```python
response = chain.invoke(input={"information": information})
```

### `invoke()` method

Executes the chain.

### Input format

```
{
  "information": information
}
```

LangChain fills the template variable:

```
{information}
```

Then sends the completed prompt to the LLM.

---

# 10. Printing the Output

```python
print(response.content)
```

### Response Object

The LLM returns an **AIMessage object**.

Example structure:

```
AIMessage(
  content="Generated response text"
)
```

`.content` extracts the actual text.

---

# 11. Program Entry Point

```python
if __name__ == "__main__":
    main()
```

This ensures the `main()` function runs only when the file is executed directly.

---

# 12. Complete Execution Flow

### Step-by-step

1. Load environment variables
2. Define input text (Elon Musk biography)
3. Create a prompt template
4. Initialize the LLM
5. Build a chain (Prompt → LLM)
6. Invoke the chain with input data
7. Receive the LLM response
8. Print the result

---

# 13. Key LangChain Concepts in This Code

| Concept        | Description                       |                          |
| -------------- | --------------------------------- | ------------------------ |
| PromptTemplate | Structured prompts with variables |                          |
| LLM            | AI model generating responses     |                          |
| Chain          | Pipeline connecting components    |                          |
| LCEL (`        | `)                                | Operator to build chains |
| invoke()       | Runs the chain                    |                          |

---

# 14. Expected Output Example

Example output from the model:

```
Summary:
Elon Musk is a billionaire entrepreneur known for leading Tesla, SpaceX, and X.

Interesting Facts:
1. He founded SpaceX in 2002 and pioneered reusable rockets.
2. He co-founded OpenAI before later leaving the organization.
```

---

# 15. Important Beginner Insight

This program demonstrates a **basic LangChain pipeline**.

More advanced applications extend this architecture:

```
Prompt → LLM → Output Parser
Prompt → Memory → LLM
Retriever → Prompt → LLM
```

Example advanced workflow (RAG):

```
User Question
      ↓
Vector Database
      ↓
Relevant Documents
      ↓
Prompt
      ↓
LLM
      ↓
Answer
```

---

✅ **Quick Summary**

This program demonstrates a **simple LangChain chain** that:

* Takes text information
* Uses a prompt template to structure instructions
* Sends the prompt to an LLM
* Returns an AI-generated summary and facts.

---
---
---
---
---

# LangSmith Integration Notes (Using uv)

## 1. What is LangSmith?

**LangSmith** is a debugging and observability platform for **LangChain applications.

It helps developers:

* trace LLM calls
* debug chains and agents
* visualize prompts and responses
* monitor performance
* track failures in AI pipelines

LangSmith records every step of your LangChain execution.

### Example Trace

```
Chain
 ├ PromptTemplate
 └ ChatModel
```

You can inspect these runs in the LangSmith dashboard.

---

# 2. Install Dependencies with uv

When using **uv** (Python package manager), install required packages.

```bash
uv add langchain langchain-openai langsmith python-dotenv
```

This updates:

```
pyproject.toml
uv.lock
```

and installs the dependencies.

---

# 3. Create `.env` File

Add LangSmith environment variables.

```env
LANGSMITH_TRACING=true
LANGSMITH_API_KEY=your_langsmith_api_key
LANGSMITH_PROJECT=langchain-course
```

### Explanation

| Variable          | Purpose                   |
| ----------------- | ------------------------- |
| LANGSMITH_TRACING | enables tracing           |
| LANGSMITH_API_KEY | authentication key        |
| LANGSMITH_PROJECT | project name in dashboard |

⚠️ Do **not commit `.env` to Git**.

Add `.env` to `.gitignore`.

---

# 4. Load Environment Variables

In your Python code, load the `.env` file.

```python
from dotenv import load_dotenv
load_dotenv()
```

This allows LangChain to access the API key.

---

# 5. Example LangChain Code with LangSmith

```python
import os
from dotenv import load_dotenv
from langchain_core.prompts import PromptTemplate
from langchain_openai import ChatOpenAI

load_dotenv()

information = "Elon Musk biography..."

template = """
Given the information {information}, create:
1. Short summary
2. Two interesting facts
"""

prompt = PromptTemplate(
    input_variables=["information"],
    template=template
)

llm = ChatOpenAI(
    model="gpt-5",
    temperature=0
)

chain = prompt | llm

response = chain.invoke({"information": information})

print(response.content)
```

When this program runs, LangSmith automatically logs the run.

---

# 6. Viewing Runs in LangSmith

Open the dashboard:

```
https://smith.langchain.com
```

Navigate to:

```
Projects → langchain-course
```

You will see:

* prompts
* model responses
* execution traces
* token usage

---

# 7. LangSmith Execution Flow

```
LangChain Program
        ↓
Prompt Template
        ↓
Chat Model (LLM)
        ↓
LangSmith Tracing
        ↓
Dashboard Visualization
```

---

# 8. Benefits of LangSmith

| Feature    | Benefit                     |
| ---------- | --------------------------- |
| Tracing    | see each step in a chain    |
| Debugging  | inspect prompts and outputs |
| Monitoring | track production AI apps    |
| Evaluation | compare LLM outputs         |

---

# 9. When LangSmith Becomes Most Useful

LangSmith is particularly valuable when working with:

* **RAG pipelines**
* **Agents**
* **Tools**
* **Multi-step chains**

Example pipeline:

```
User Question
      ↓
Retriever
      ↓
Documents
      ↓
Prompt
      ↓
LLM
      ↓
Answer
```

LangSmith visualizes every step.

---

# 10. Disable LangSmith (Optional)

If you do not want tracing:

```env
LANGSMITH_TRACING=false
```

or remove the variable.

---

# Quick Summary

To integrate LangSmith:

1. Install dependencies using `uv`
2. Create `.env` with API key
3. Load environment variables
4. Run LangChain code
5. View traces in LangSmith dashboard

---

